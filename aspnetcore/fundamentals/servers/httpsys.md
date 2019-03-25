---
title: Implementazione del server Web HTTP.sys in ASP.NET Core
author: guardrex
description: Informazioni su HTTP.sys, un server Web per ASP.NET Core in Windows. Basato sul driver in modalità kernel HTTP.sys, HTTP.sys è un'alternativa a Kestrel che consente la connessione diretta a Internet senza IIS.
monikerRange: '>= aspnetcore-2.0'
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/21/2019
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 6ec9b7bf3da0015b8ac3918a4d47644fffc14cdb
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209174"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="855ad-104">Implementazione del server Web HTTP.sys in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="855ad-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="855ad-105">Di [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="855ad-105">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="855ad-106">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) è un [server Web per ASP.NET Core](xref:fundamentals/servers/index) che può essere eseguito solo in Windows.</span><span class="sxs-lookup"><span data-stu-id="855ad-106">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) is a [web server for ASP.NET Core](xref:fundamentals/servers/index) that only runs on Windows.</span></span> <span data-ttu-id="855ad-107">HTTP.sys è un'alternativa al server [Kestrel](xref:fundamentals/servers/kestrel) e offre alcune funzionalità non presenti in Kestrel.</span><span class="sxs-lookup"><span data-stu-id="855ad-107">HTTP.sys is an alternative to [Kestrel](xref:fundamentals/servers/kestrel) server and offers some features that Kestrel doesn't provide.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="855ad-108">HTTP.sys non è compatibile con il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) e non può essere usato con IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="855ad-108">HTTP.sys isn't compatible with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and can't be used with IIS or IIS Express.</span></span>

<span data-ttu-id="855ad-109">HTTP.sys supporta le funzionalità seguenti:</span><span class="sxs-lookup"><span data-stu-id="855ad-109">HTTP.sys supports the following features:</span></span>

* [<span data-ttu-id="855ad-110">Autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="855ad-110">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
* <span data-ttu-id="855ad-111">Condivisione delle porte</span><span class="sxs-lookup"><span data-stu-id="855ad-111">Port sharing</span></span>
* <span data-ttu-id="855ad-112">HTTPS con SNI</span><span class="sxs-lookup"><span data-stu-id="855ad-112">HTTPS with SNI</span></span>
* <span data-ttu-id="855ad-113">HTTP/2 su TLS (Windows 10 o versioni successive)</span><span class="sxs-lookup"><span data-stu-id="855ad-113">HTTP/2 over TLS (Windows 10 or later)</span></span>
* <span data-ttu-id="855ad-114">Trasmissione diretta dei file</span><span class="sxs-lookup"><span data-stu-id="855ad-114">Direct file transmission</span></span>
* <span data-ttu-id="855ad-115">Memorizzazione nella cache delle risposte</span><span class="sxs-lookup"><span data-stu-id="855ad-115">Response caching</span></span>
* <span data-ttu-id="855ad-116">WebSockets (Windows 8 o versioni successive)</span><span class="sxs-lookup"><span data-stu-id="855ad-116">WebSockets (Windows 8 or later)</span></span>

<span data-ttu-id="855ad-117">Versioni di Windows supportate:</span><span class="sxs-lookup"><span data-stu-id="855ad-117">Supported Windows versions:</span></span>

* <span data-ttu-id="855ad-118">Windows 7 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="855ad-118">Windows 7 or later</span></span>
* <span data-ttu-id="855ad-119">Windows Server 2008 R2 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="855ad-119">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="855ad-120">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="855ad-120">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="855ad-121">Quando usare HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="855ad-121">When to use HTTP.sys</span></span>

<span data-ttu-id="855ad-122">HTTP.sys è utile per le distribuzioni nei casi seguenti:</span><span class="sxs-lookup"><span data-stu-id="855ad-122">HTTP.sys is useful for deployments where:</span></span>

* <span data-ttu-id="855ad-123">È necessario esporre il server direttamente a Internet senza usare IIS.</span><span class="sxs-lookup"><span data-stu-id="855ad-123">There's a need to expose the server directly to the Internet without using IIS.</span></span>

  ![HTTP.sys comunica direttamente con Internet](httpsys/_static/httpsys-to-internet.png)

* <span data-ttu-id="855ad-125">Una distribuzione interna richiede una funzionalità non disponibile in Kestrel, ad esempio l'[autenticazione di Windows](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="855ad-125">An internal deployment requires a feature not available in Kestrel, such as [Windows Authentication](xref:security/authentication/windowsauth).</span></span>

  ![HTTP.sys comunica direttamente con la rete interna](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="855ad-127">HTTP.sys è una tecnologia consolidata che protegge da molti tipi di attacchi e offre l'affidabilità, la sicurezza e la scalabilità di un server Web con funzionalità complete.</span><span class="sxs-lookup"><span data-stu-id="855ad-127">HTTP.sys is mature technology that protects against many types of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="855ad-128">IIS viene eseguito come listener HTTP in HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="855ad-128">IIS itself runs as an HTTP listener on top of HTTP.sys.</span></span>

## <a name="http2-support"></a><span data-ttu-id="855ad-129">Supporto per HTTP/2</span><span class="sxs-lookup"><span data-stu-id="855ad-129">HTTP/2 support</span></span>

<span data-ttu-id="855ad-130">[HTTP/2](https://httpwg.org/specs/rfc7540.html) è abilitato per le app ASP.NET Core se vengono soddisfatti i requisiti di base seguenti:</span><span class="sxs-lookup"><span data-stu-id="855ad-130">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is enabled for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="855ad-131">Windows Server 2016/Windows 10 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="855ad-131">Windows Server 2016/Windows 10 or later</span></span>
* <span data-ttu-id="855ad-132">Connessione [ALPN (Application-Layer Protocol Negotiation)](https://tools.ietf.org/html/rfc7301#section-3)</span><span class="sxs-lookup"><span data-stu-id="855ad-132">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="855ad-133">Connessione TLS 1.2 o successiva</span><span class="sxs-lookup"><span data-stu-id="855ad-133">TLS 1.2 or later connection</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="855ad-134">Se viene stabilita una connessione HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) corrisponde a `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="855ad-134">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="855ad-135">Se viene stabilita una connessione HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) corrisponde a `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="855ad-135">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="855ad-136">HTTP/2 è abilitato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="855ad-136">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="855ad-137">Se non viene stabilita una connessione HTTP/2, la connessione esegue il fallback a HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="855ad-137">If an HTTP/2 connection isn't established, the connection falls back to HTTP/1.1.</span></span> <span data-ttu-id="855ad-138">In una versione futura di Windows sarà disponibile il flag di configurazione di HTTP/2 e sarà possibile disabilitare il protocollo HTTP/2 con HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="855ad-138">In a future release of Windows, HTTP/2 configuration flags will be available, including the ability to disable HTTP/2 with HTTP.sys.</span></span>

## <a name="kernel-mode-authentication-with-kerberos"></a><span data-ttu-id="855ad-139">Autenticazione in modalità kernel con Kerberos</span><span class="sxs-lookup"><span data-stu-id="855ad-139">Kernel mode authentication with Kerberos</span></span>

<span data-ttu-id="855ad-140">Per la delega all'autenticazione in modalità kernel, HTTP.sys usa il protocollo di autenticazione Kerberos.</span><span class="sxs-lookup"><span data-stu-id="855ad-140">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="855ad-141">L'autenticazione in modalità utente non è supportata con Kerberos e HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="855ad-141">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="855ad-142">È necessario usare l'account del computer per decrittografare il token/ticket Kerberos ottenuto da Active Directory e inoltrato dal client al server per autenticare l'utente.</span><span class="sxs-lookup"><span data-stu-id="855ad-142">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="855ad-143">Registrare il nome dell'entità servizio per l'host, non l'utente dell'app.</span><span class="sxs-lookup"><span data-stu-id="855ad-143">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

## <a name="how-to-use-httpsys"></a><span data-ttu-id="855ad-144">Come usare HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="855ad-144">How to use HTTP.sys</span></span>

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a><span data-ttu-id="855ad-145">Configurare l'app ASP.NET Core per l'uso di HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="855ad-145">Configure the ASP.NET Core app to use HTTP.sys</span></span>

1. <span data-ttu-id="855ad-146">Non è necessario un riferimento al pacchetto nel file di progetto quando si usa il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (ASP.NET Core 2.1 o versioni successive).</span><span class="sxs-lookup"><span data-stu-id="855ad-146">A package reference in the project file isn't required when using the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="855ad-147">Se non si usa il metapacchetto `Microsoft.AspNetCore.App` aggiungere un riferimento al pacchetto a [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).</span><span class="sxs-lookup"><span data-stu-id="855ad-147">When not using the `Microsoft.AspNetCore.App` metapackage, add a package reference to [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).</span></span>

2. <span data-ttu-id="855ad-148">Chiamare il metodo di estensione <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> quando si compila l'host Web, specificando le eventuali <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions> necessarie:</span><span class="sxs-lookup"><span data-stu-id="855ad-148">Call the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> extension method when building Web Host, specifying any required <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>:</span></span>

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   <span data-ttu-id="855ad-149">Le configurazioni aggiuntive di HTTP.sys vengono gestite tramite [impostazioni del Registro di sistema](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).</span><span class="sxs-lookup"><span data-stu-id="855ad-149">Additional HTTP.sys configuration is handled through [registry settings](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).</span></span>

   <span data-ttu-id="855ad-150">**Opzioni di HTTP.sys**</span><span class="sxs-lookup"><span data-stu-id="855ad-150">**HTTP.sys options**</span></span>

   | <span data-ttu-id="855ad-151">Proprietà</span><span class="sxs-lookup"><span data-stu-id="855ad-151">Property</span></span> | <span data-ttu-id="855ad-152">Description</span><span class="sxs-lookup"><span data-stu-id="855ad-152">Description</span></span> | <span data-ttu-id="855ad-153">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="855ad-153">Default</span></span> |
   | -------- | ----------- | :-----: |
   | [<span data-ttu-id="855ad-154">AllowSynchronousIO</span><span class="sxs-lookup"><span data-stu-id="855ad-154">AllowSynchronousIO</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | <span data-ttu-id="855ad-155">Controllare se l'input e/o l'output sincroni sono consentiti per `HttpContext.Request.Body` e `HttpContext.Response.Body`.</span><span class="sxs-lookup"><span data-stu-id="855ad-155">Control whether synchronous input/output is allowed for the `HttpContext.Request.Body` and `HttpContext.Response.Body`.</span></span> | `true` |
   | [<span data-ttu-id="855ad-156">Authentication.AllowAnonymous</span><span class="sxs-lookup"><span data-stu-id="855ad-156">Authentication.AllowAnonymous</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | <span data-ttu-id="855ad-157">Consentire richieste anonime.</span><span class="sxs-lookup"><span data-stu-id="855ad-157">Allow anonymous requests.</span></span> | `true` |
   | [<span data-ttu-id="855ad-158">Authentication.Schemes</span><span class="sxs-lookup"><span data-stu-id="855ad-158">Authentication.Schemes</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | <span data-ttu-id="855ad-159">Specificare gli schemi di autenticazione consentiti.</span><span class="sxs-lookup"><span data-stu-id="855ad-159">Specify the allowed authentication schemes.</span></span> <span data-ttu-id="855ad-160">Può essere modificata in qualsiasi momento prima dell'eliminazione del listener.</span><span class="sxs-lookup"><span data-stu-id="855ad-160">May be modified at any time prior to disposing the listener.</span></span> <span data-ttu-id="855ad-161">I valori sono forniti dall'[enumerazione AuthenticationSchemes](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None` e `NTLM`.</span><span class="sxs-lookup"><span data-stu-id="855ad-161">Values are provided by the [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None`, and `NTLM`.</span></span> | `None` |
   | [<span data-ttu-id="855ad-162">EnableResponseCaching</span><span class="sxs-lookup"><span data-stu-id="855ad-162">EnableResponseCaching</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | <span data-ttu-id="855ad-163">Tentare la memorizzazione nella cache in [modalità kernel](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) per le risposte con intestazioni idonee.</span><span class="sxs-lookup"><span data-stu-id="855ad-163">Attempt [kernel-mode](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) caching for responses with eligible headers.</span></span> <span data-ttu-id="855ad-164">La risposta potrebbe non includere intestazioni `Set-Cookie`, `Vary` o `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="855ad-164">The response may not include `Set-Cookie`, `Vary`, or `Pragma` headers.</span></span> <span data-ttu-id="855ad-165">Deve includere un'intestazione `Cache-Control` `public` con valore `shared-max-age` o `max-age` o un'intestazione `Expires`.</span><span class="sxs-lookup"><span data-stu-id="855ad-165">It must include a `Cache-Control` header that's `public` and either a `shared-max-age` or `max-age` value, or an `Expires` header.</span></span> | `true` |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | <span data-ttu-id="855ad-166">Numero massimo di accettazioni simultanee.</span><span class="sxs-lookup"><span data-stu-id="855ad-166">The maximum number of concurrent accepts.</span></span> | <span data-ttu-id="855ad-167">5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span><span class="sxs-lookup"><span data-stu-id="855ad-167">5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | <span data-ttu-id="855ad-168">Numero massimo di connessioni simultanee da accettare.</span><span class="sxs-lookup"><span data-stu-id="855ad-168">The maximum number of concurrent connections to accept.</span></span> <span data-ttu-id="855ad-169">Usare `-1` per un numero infinito.</span><span class="sxs-lookup"><span data-stu-id="855ad-169">Use `-1` for infinite.</span></span> <span data-ttu-id="855ad-170">Usare `null` per usare l'impostazione a livello di computer del Registro di sistema.</span><span class="sxs-lookup"><span data-stu-id="855ad-170">Use `null` to use the registry's machine-wide setting.</span></span> | `null`<br><span data-ttu-id="855ad-171">(illimitato)</span><span class="sxs-lookup"><span data-stu-id="855ad-171">(unlimited)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | <span data-ttu-id="855ad-172">Vedere la sezione <a href="#maxrequestbodysize">MaxRequestBodySize</a>.</span><span class="sxs-lookup"><span data-stu-id="855ad-172">See the <a href="#maxrequestbodysize">MaxRequestBodySize</a> section.</span></span> | <span data-ttu-id="855ad-173">30000000 byte</span><span class="sxs-lookup"><span data-stu-id="855ad-173">30000000 bytes</span></span><br><span data-ttu-id="855ad-174">(~28,6 MB)</span><span class="sxs-lookup"><span data-stu-id="855ad-174">(~28.6 MB)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | <span data-ttu-id="855ad-175">Numero massimo di richieste che è possibile accodare.</span><span class="sxs-lookup"><span data-stu-id="855ad-175">The maximum number of requests that can be queued.</span></span> | <span data-ttu-id="855ad-176">1000</span><span class="sxs-lookup"><span data-stu-id="855ad-176">1000</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | <span data-ttu-id="855ad-177">Indica se le scritture del corpo della risposta che hanno esito negativo a causa di disconnessioni del client devono generare eccezioni o vengono completate normalmente.</span><span class="sxs-lookup"><span data-stu-id="855ad-177">Indicate if response body writes that fail due to client disconnects should throw exceptions or complete normally.</span></span> | `false`<br><span data-ttu-id="855ad-178">(completamento normale)</span><span class="sxs-lookup"><span data-stu-id="855ad-178">(complete normally)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | <span data-ttu-id="855ad-179">Espone la configurazione di <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> HTTP.sys, che può essere configurata anche nel Registro di sistema.</span><span class="sxs-lookup"><span data-stu-id="855ad-179">Expose the HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> configuration, which may also be configured in the registry.</span></span> <span data-ttu-id="855ad-180">Seguire i collegamenti API per altre informazioni su ogni impostazione, inclusi i valori predefiniti:</span><span class="sxs-lookup"><span data-stu-id="855ad-180">Follow the API links to learn more about each setting, including default values:</span></span><ul><li><span data-ttu-id="855ad-181">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; Tempo consentito all'API HTTP Server per svuotare il corpo dell'entità in una connessione keep-alive.</span><span class="sxs-lookup"><span data-stu-id="855ad-181">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; Time allowed for the HTTP Server API to drain the entity body on a Keep-Alive connection.</span></span></li><li><span data-ttu-id="855ad-182">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; Tempo consentito per l'arrivo del corpo dell'entità della richiesta.</span><span class="sxs-lookup"><span data-stu-id="855ad-182">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; Time allowed for the request entity body to arrive.</span></span></li><li><span data-ttu-id="855ad-183">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; Tempo consentito all'API del server HTTP per analizzare l'intestazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="855ad-183">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; Time allowed for the HTTP Server API to parse the request header.</span></span></li><li><span data-ttu-id="855ad-184">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; Tempo consentito per una connessione inattiva.</span><span class="sxs-lookup"><span data-stu-id="855ad-184">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; Time allowed for an idle connection.</span></span></li><li><span data-ttu-id="855ad-185">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; Velocità di invio minima per la risposta.</span><span class="sxs-lookup"><span data-stu-id="855ad-185">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; The minimum send rate for the response.</span></span></li><li><span data-ttu-id="855ad-186">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; Tempo consentito alla richiesta per rimanere in coda prima che sia selezionata dall'app.</span><span class="sxs-lookup"><span data-stu-id="855ad-186">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; Time allowed for the request to remain in the request queue before the app picks it up.</span></span></li></ul> |  |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | <span data-ttu-id="855ad-187">Specificare l'elemento <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> da registrare con HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="855ad-187">Specify the <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> to register with HTTP.sys.</span></span> <span data-ttu-id="855ad-188">Il più utile è il metodo [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*) usato per aggiungere un prefisso alla raccolta.</span><span class="sxs-lookup"><span data-stu-id="855ad-188">The most useful is [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), which is used to add a prefix to the collection.</span></span> <span data-ttu-id="855ad-189">Queste impostazioni possono essere modificate in qualsiasi momento prima dell'eliminazione del listener.</span><span class="sxs-lookup"><span data-stu-id="855ad-189">These may be modified at any time prior to disposing the listener.</span></span> |  |

   <a name="maxrequestbodysize"></a>

   <span data-ttu-id="855ad-190">**MaxRequestBodySize**</span><span class="sxs-lookup"><span data-stu-id="855ad-190">**MaxRequestBodySize**</span></span>

   <span data-ttu-id="855ad-191">Dimensioni massime consentite per qualsiasi corpo della richiesta in byte.</span><span class="sxs-lookup"><span data-stu-id="855ad-191">The maximum allowed size of any request body in bytes.</span></span> <span data-ttu-id="855ad-192">Con l'impostazione `null`, le dimensioni massime del corpo della richiesta sono illimitate.</span><span class="sxs-lookup"><span data-stu-id="855ad-192">When set to `null`, the maximum request body size is unlimited.</span></span> <span data-ttu-id="855ad-193">Questo limite non ha effetto sulle connessioni aggiornate, che sono sempre illimitate.</span><span class="sxs-lookup"><span data-stu-id="855ad-193">This limit has no effect on upgraded connections, which are always unlimited.</span></span>

   <span data-ttu-id="855ad-194">Il metodo consigliato per ignorare il limite in un'applicazione ASP.NET Core MVC per un singolo `IActionResult` prevede l'uso dell'attributo <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> in un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="855ad-194">The recommended method to override the limit in an ASP.NET Core MVC app for a single `IActionResult` is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   <span data-ttu-id="855ad-195">Se l'app tenta di configurare il limite per una richiesta dopo che l'app ha avviato la lettura della richiesta stessa, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="855ad-195">An exception is thrown if the app attempts to configure the limit on a request after the app has started reading the request.</span></span> <span data-ttu-id="855ad-196">È possibile usare una proprietà `IsReadOnly` per indicare se la proprietà `MaxRequestBodySize` è in stato di sola lettura e pertanto è troppo tardi per configurare il limite.</span><span class="sxs-lookup"><span data-stu-id="855ad-196">An `IsReadOnly` property can be used to indicate if the `MaxRequestBodySize` property is in a read-only state, meaning it's too late to configure the limit.</span></span>

   <span data-ttu-id="855ad-197">Se l'app deve eseguire l'override di <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> per ogni richiesta, usare <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>:</span><span class="sxs-lookup"><span data-stu-id="855ad-197">If the app should override <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> per-request, use the <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>:</span></span>

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

3. <span data-ttu-id="855ad-198">Se si usa Visual Studio, assicurarsi che l'app non sia configurata per l'esecuzione di IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="855ad-198">If using Visual Studio, make sure the app isn't configured to run IIS or IIS Express.</span></span>

   <span data-ttu-id="855ad-199">In Visual Studio il profilo di avvio predefinito è per IIS Express.</span><span class="sxs-lookup"><span data-stu-id="855ad-199">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="855ad-200">Per eseguire il progetto come app console, modificare manualmente il profilo selezionato, come illustrato nello screenshot seguente:</span><span class="sxs-lookup"><span data-stu-id="855ad-200">To run the project as a console app, manually change the selected profile, as shown in the following screen shot:</span></span>

   ![Selezionare il profilo dell'applicazione console](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a><span data-ttu-id="855ad-202">Configurare Windows Server</span><span class="sxs-lookup"><span data-stu-id="855ad-202">Configure Windows Server</span></span>

1. <span data-ttu-id="855ad-203">Determinare le porte da aprire per l'app e usare [Windows Firewall](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) o il cmdlet di PowerShell [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) per aprire le porte del firewall per consentire al traffico di raggiungere HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="855ad-203">Determine the ports to open for the app and use [Windows Firewall](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) or the [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) PowerShell cmdlet to open firewall ports to allow traffic to reach HTTP.sys.</span></span> <span data-ttu-id="855ad-204">Nei comandi seguenti e nella configurazione dell'app, viene usata la porta 443.</span><span class="sxs-lookup"><span data-stu-id="855ad-204">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="855ad-205">Quando si distribuisce una macchina virtuale di Azure, aprire le porte nel [gruppo di sicurezza di rete](/azure/virtual-machines/windows/nsg-quickstart-portal).</span><span class="sxs-lookup"><span data-stu-id="855ad-205">When deploying to an Azure VM, open the ports in the [Network Security Group](/azure/virtual-machines/windows/nsg-quickstart-portal).</span></span> <span data-ttu-id="855ad-206">Nei comandi seguenti e nella configurazione dell'app, viene usata la porta 443.</span><span class="sxs-lookup"><span data-stu-id="855ad-206">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="855ad-207">Ottenere e installare i certificati X.509, se necessario.</span><span class="sxs-lookup"><span data-stu-id="855ad-207">Obtain and install X.509 certificates, if required.</span></span>

   <span data-ttu-id="855ad-208">In Windows, creare certificati autofirmati con il [cmdlet di PowerShell New-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="855ad-208">On Windows, create self-signed certificates using the [New-SelfSignedCertificate PowerShell cmdlet](/powershell/module/pkiclient/new-selfsignedcertificate).</span></span> <span data-ttu-id="855ad-209">Per un esempio non supportato, vedere [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).</span><span class="sxs-lookup"><span data-stu-id="855ad-209">For an unsupported example, see [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).</span></span>

   <span data-ttu-id="855ad-210">Installare i certificati autofirmati o con firma nell'archivio **Computer locale** > **Personale** del server.</span><span class="sxs-lookup"><span data-stu-id="855ad-210">Install either self-signed or CA-signed certificates in the server's **Local Machine** > **Personal** store.</span></span>

1. <span data-ttu-id="855ad-211">Se l'app è una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd), installare .NET Core, .NET Framework o entrambi (se l'app è un'app .NET Core destinata a .NET Framework).</span><span class="sxs-lookup"><span data-stu-id="855ad-211">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd), install .NET Core, .NET Framework, or both (if the app is a .NET Core app targeting the .NET Framework).</span></span>

   * <span data-ttu-id="855ad-212">**.NET Core** &ndash; Se l'app richiede .NET Core, ottenere ed eseguire il programma di installazione di **.NET Core Runtime** dalla [pagina di download per .NET Core](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="855ad-212">**.NET Core** &ndash; If the app requires .NET Core, obtain and run the **.NET Core Runtime** installer from [.NET Core Downloads](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="855ad-213">Non installare l'SDK completo nel server.</span><span class="sxs-lookup"><span data-stu-id="855ad-213">Don't install the full SDK on the server.</span></span>
   * <span data-ttu-id="855ad-214">**.NET Framework** &ndash; Se l'app richiede .NET Framework, vedere la [guida all'installazione di .NET Framework](/dotnet/framework/install/).</span><span class="sxs-lookup"><span data-stu-id="855ad-214">**.NET Framework** &ndash; If the app requires .NET Framework, see the [.NET Framework installation guide](/dotnet/framework/install/).</span></span> <span data-ttu-id="855ad-215">Installare la versione di .NET Framework richiesta.</span><span class="sxs-lookup"><span data-stu-id="855ad-215">Install the required .NET Framework.</span></span> <span data-ttu-id="855ad-216">Il programma di installazione per la versione più recente di .NET Framework è disponibile dalla pagina di [download per .NET Core](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="855ad-216">The installer for the latest .NET Framework is available from the [.NET Core Downloads](https://dotnet.microsoft.com/download) page.</span></span>

   <span data-ttu-id="855ad-217">Se l'app è una [distribuzione autonoma](/dotnet/core/deploying/#framework-dependent-deployments-scd) include il runtime nella distribuzione.</span><span class="sxs-lookup"><span data-stu-id="855ad-217">If the app is a [self-contained deployment](/dotnet/core/deploying/#framework-dependent-deployments-scd), the app includes the runtime in its deployment.</span></span> <span data-ttu-id="855ad-218">Non è richiesta l'installazione di un framework nel server.</span><span class="sxs-lookup"><span data-stu-id="855ad-218">No framework installation is required on the server.</span></span>

1. <span data-ttu-id="855ad-219">Configurare gli URL e le porte nell'app.</span><span class="sxs-lookup"><span data-stu-id="855ad-219">Configure URLs and ports in the app.</span></span>

   <span data-ttu-id="855ad-220">Per impostazione predefinita, ASP.NET Core è associato a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="855ad-220">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="855ad-221">Per configurare le porte e i prefissi URL, è possibile usare:</span><span class="sxs-lookup"><span data-stu-id="855ad-221">To configure URL prefixes and ports, options include:</span></span>

   * <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
   * <span data-ttu-id="855ad-222">L'argomento della riga di comando `urls`</span><span class="sxs-lookup"><span data-stu-id="855ad-222">`urls` command-line argument</span></span>
   * <span data-ttu-id="855ad-223">La variabile di ambiente `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="855ad-223">`ASPNETCORE_URLS` environment variable</span></span>
   * <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes>

   <span data-ttu-id="855ad-224">L'esempio di codice seguente mostra come usare <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> con l'indirizzo IP locale del server `10.0.0.4` sulla porta 443:</span><span class="sxs-lookup"><span data-stu-id="855ad-224">The following code example shows how to use <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> with the server's local IP address `10.0.0.4` on port 443:</span></span>

   [!code-csharp[](httpsys/sample_snapshot/Program.cs?name=snippet1&highlight=11)]

   <span data-ttu-id="855ad-225">Un vantaggio offerto da `UrlPrefixes` è che viene generato immediatamente un messaggio di errore per i prefissi non formattati correttamente.</span><span class="sxs-lookup"><span data-stu-id="855ad-225">An advantage of `UrlPrefixes` is that an error message is generated immediately for improperly formatted prefixes.</span></span>

   <span data-ttu-id="855ad-226">Le impostazioni di `UrlPrefixes` sostituiscono le impostazioni `UseUrls`/`urls`/`ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="855ad-226">The settings in `UrlPrefixes` override `UseUrls`/`urls`/`ASPNETCORE_URLS` settings.</span></span> <span data-ttu-id="855ad-227">Pertanto, un vantaggio offerto da `UseUrls`, `urls` e dalla variabile di ambiente `ASPNETCORE_URLS` è che risulta più semplice alternare Kestrel e HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="855ad-227">Therefore, an advantage of `UseUrls`, `urls`, and the `ASPNETCORE_URLS` environment variable is that it's easier to switch between Kestrel and HTTP.sys.</span></span> <span data-ttu-id="855ad-228">Per ulteriori informazioni, vedere <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="855ad-228">For more information, see <xref:fundamentals/host/web-host>.</span></span>

   <span data-ttu-id="855ad-229">HTTP.sys usa i [formati di stringa UrlPrefix dell'API del server HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="855ad-229">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

   > [!WARNING]
   > <span data-ttu-id="855ad-230">Le associazioni con caratteri jolly di livello superiore (`http://*:80/` e `http://+:80`) **non** devono essere usate,</span><span class="sxs-lookup"><span data-stu-id="855ad-230">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="855ad-231">poiché possono creare vulnerabilità a livello di sicurezza nell'app.</span><span class="sxs-lookup"><span data-stu-id="855ad-231">Top-level wildcard bindings create app security vulnerabilities.</span></span> <span data-ttu-id="855ad-232">Questo concetto vale sia per i caratteri jolly sicuri che vulnerabili.</span><span class="sxs-lookup"><span data-stu-id="855ad-232">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="855ad-233">Usare nomi host o indirizzi IP espliciti al posto di caratteri jolly.</span><span class="sxs-lookup"><span data-stu-id="855ad-233">Use explicit host names or IP addresses rather than wildcards.</span></span> <span data-ttu-id="855ad-234">L'associazione con caratteri jolly del sottodominio (ad esempio, `*.mysub.com`) non costituisce un rischio per la sicurezza se viene controllato l'intero dominio padre (a differenza di `*.com`, che è vulnerabile).</span><span class="sxs-lookup"><span data-stu-id="855ad-234">Subdomain wildcard binding (for example, `*.mysub.com`) isn't a security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="855ad-235">Per altre informazioni, vedere [RFC 7230: sezione 5.4: Host](https://tools.ietf.org/html/rfc7230#section-5.4).</span><span class="sxs-lookup"><span data-stu-id="855ad-235">For more information, see [RFC 7230: Section 5.4: Host](https://tools.ietf.org/html/rfc7230#section-5.4).</span></span>

1. <span data-ttu-id="855ad-236">Pre-registrare i prefissi URL nel server.</span><span class="sxs-lookup"><span data-stu-id="855ad-236">Preregister URL prefixes on the server.</span></span>

   <span data-ttu-id="855ad-237">Lo strumento predefinito per la configurazione di HTTP.sys è *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="855ad-237">The built-in tool for configuring HTTP.sys is *netsh.exe*.</span></span> <span data-ttu-id="855ad-238">*Netsh.exe* viene usato per riservare i prefissi URL e assegnare i certificati X.509.</span><span class="sxs-lookup"><span data-stu-id="855ad-238">*netsh.exe* is used to reserve URL prefixes and assign X.509 certificates.</span></span> <span data-ttu-id="855ad-239">Per questo strumento sono necessari privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="855ad-239">The tool requires administrator privileges.</span></span>

   <span data-ttu-id="855ad-240">Usare lo strumento *netsh.exe* per registrare gli URL per l'app:</span><span class="sxs-lookup"><span data-stu-id="855ad-240">Use the *netsh.exe* tool to register URLs for the app:</span></span>

   ```console
   netsh http add urlacl url=<URL> user=<USER>
   ```

   * <span data-ttu-id="855ad-241">`<URL>` &ndash; L'URL (Uniform Resource Locator) completo.</span><span class="sxs-lookup"><span data-stu-id="855ad-241">`<URL>` &ndash; The fully qualified Uniform Resource Locator (URL).</span></span> <span data-ttu-id="855ad-242">Non usare un'associazione con caratteri jolly.</span><span class="sxs-lookup"><span data-stu-id="855ad-242">Don't use a wildcard binding.</span></span> <span data-ttu-id="855ad-243">Usare un nome host valido o un indirizzo IP locale.</span><span class="sxs-lookup"><span data-stu-id="855ad-243">Use a valid hostname or local IP address.</span></span> <span data-ttu-id="855ad-244">*L'URL deve includere una barra finale.*</span><span class="sxs-lookup"><span data-stu-id="855ad-244">*The URL must include a trailing slash.*</span></span>
   * <span data-ttu-id="855ad-245">`<USER>` &ndash; Specifica il nome dell'utente o del gruppo di utenti.</span><span class="sxs-lookup"><span data-stu-id="855ad-245">`<USER>` &ndash; Specifies the user or user-group name.</span></span>

   <span data-ttu-id="855ad-246">Nell'esempio seguente, l'indirizzo IP locale del server è `10.0.0.4`:</span><span class="sxs-lookup"><span data-stu-id="855ad-246">In the following example, the local IP address of the server is `10.0.0.4`:</span></span>

   ```console
   netsh http add urlacl url=https://10.0.0.4:443/ user=Users
   ```

   <span data-ttu-id="855ad-247">Quando viene registrato un URL, lo strumento risponde con `URL reservation successfully added`.</span><span class="sxs-lookup"><span data-stu-id="855ad-247">When a URL is registered, the tool responds with `URL reservation successfully added`.</span></span>

   <span data-ttu-id="855ad-248">Per eliminare un URL registrato, usare il comando `delete urlacl`:</span><span class="sxs-lookup"><span data-stu-id="855ad-248">To delete a registered URL, use the `delete urlacl` command:</span></span>

   ```console
   netsh http delete urlacl url=<URL>
   ```

1. <span data-ttu-id="855ad-249">Registrare i certificati X.509 nel server.</span><span class="sxs-lookup"><span data-stu-id="855ad-249">Register X.509 certificates on the server.</span></span>

   <span data-ttu-id="855ad-250">Usare lo strumento *netsh.exe* per registrare i certificati per l'app:</span><span class="sxs-lookup"><span data-stu-id="855ad-250">Use the *netsh.exe* tool to register certificates for the app:</span></span>

   ```console
   netsh http add sslcert ipport=<IP>:<PORT> certhash=<THUMBPRINT> appid="{<GUID>}"
   ```

   * <span data-ttu-id="855ad-251">`<IP>` &ndash; Specifica l'indirizzo IP locale per l'associazione.</span><span class="sxs-lookup"><span data-stu-id="855ad-251">`<IP>` &ndash; Specifies the local IP address for the binding.</span></span> <span data-ttu-id="855ad-252">Non usare un'associazione con caratteri jolly.</span><span class="sxs-lookup"><span data-stu-id="855ad-252">Don't use a wildcard binding.</span></span> <span data-ttu-id="855ad-253">Usare un indirizzo IP valido.</span><span class="sxs-lookup"><span data-stu-id="855ad-253">Use a valid IP address.</span></span>
   * <span data-ttu-id="855ad-254">`<PORT>` &ndash; Specifica la porta per l'associazione.</span><span class="sxs-lookup"><span data-stu-id="855ad-254">`<PORT>` &ndash; Specifies the port for the binding.</span></span>
   * <span data-ttu-id="855ad-255">`<THUMBPRINT>` &ndash; Identificazione digitale del certificato X.509.</span><span class="sxs-lookup"><span data-stu-id="855ad-255">`<THUMBPRINT>` &ndash; The X.509 certificate thumbprint.</span></span>
   * <span data-ttu-id="855ad-256">`<GUID>` &ndash; Un GUID generato per gli sviluppatori per rappresentare l'app a scopo informativo.</span><span class="sxs-lookup"><span data-stu-id="855ad-256">`<GUID>` &ndash; A developer-generated GUID to represent the app for informational purposes.</span></span>

   <span data-ttu-id="855ad-257">A scopo di riferimento, archiviare il GUID nell'app come tag di pacchetto:</span><span class="sxs-lookup"><span data-stu-id="855ad-257">For reference purposes, store the GUID in the app as a package tag:</span></span>

   * <span data-ttu-id="855ad-258">In Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="855ad-258">In Visual Studio:</span></span>
     * <span data-ttu-id="855ad-259">Aprire le proprietà del progetto dell'app facendo clic sull'app con il pulsante destro del mouse in **Esplora soluzioni** e scegliendo **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="855ad-259">Open the app's project properties by right-clicking on the app in **Solution Explorer** and selecting **Properties**.</span></span>
     * <span data-ttu-id="855ad-260">Selezionare la scheda **Pacchetto**.</span><span class="sxs-lookup"><span data-stu-id="855ad-260">Select the **Package** tab.</span></span>
     * <span data-ttu-id="855ad-261">Immettere il GUID creato nel campo **Tag**.</span><span class="sxs-lookup"><span data-stu-id="855ad-261">Enter the GUID that you created in the **Tags** field.</span></span>
   * <span data-ttu-id="855ad-262">Se non si usa Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="855ad-262">When not using Visual Studio:</span></span>
     * <span data-ttu-id="855ad-263">Aprire il file di progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="855ad-263">Open the app's project file.</span></span>
     * <span data-ttu-id="855ad-264">Aggiungere una proprietà `<PackageTags>` a un `<PropertyGroup>` nuovo o esistente con il GUID creato:</span><span class="sxs-lookup"><span data-stu-id="855ad-264">Add a `<PackageTags>` property to a new or existing `<PropertyGroup>` with the GUID that you created:</span></span>

       ```xml
       <PropertyGroup>
         <PackageTags>9412ee86-c21b-4eb8-bd89-f650fbf44931</PackageTags>
       </PropertyGroup>
       ```

   <span data-ttu-id="855ad-265">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="855ad-265">In the following example:</span></span>

   * <span data-ttu-id="855ad-266">L'indirizzo IP locale del server è `10.0.0.4`.</span><span class="sxs-lookup"><span data-stu-id="855ad-266">The local IP address of the server is `10.0.0.4`.</span></span>
   * <span data-ttu-id="855ad-267">Un generatore di GUID casuali online fornisce il valore `appid`.</span><span class="sxs-lookup"><span data-stu-id="855ad-267">An online random GUID generator provides the `appid` value.</span></span>

   ```console
   netsh http add sslcert 
       ipport=10.0.0.4:443 
       certhash=b66ee04419d4ee37464ab8785ff02449980eae10 
       appid="{9412ee86-c21b-4eb8-bd89-f650fbf44931}"
   ```

   <span data-ttu-id="855ad-268">Quando viene registrato un certificato, lo strumento risponde con `SSL Certificate successfully added`.</span><span class="sxs-lookup"><span data-stu-id="855ad-268">When a certificate is registered, the tool responds with `SSL Certificate successfully added`.</span></span>

   <span data-ttu-id="855ad-269">Per eliminare la registrazione di un certificato, usare il comando `delete sslcert`:</span><span class="sxs-lookup"><span data-stu-id="855ad-269">To delete a certificate registration, use the `delete sslcert` command:</span></span>

   ```console
   netsh http delete sslcert ipport=<IP>:<PORT>
   ```

   <span data-ttu-id="855ad-270">Documentazione di riferimento per *netsh.exe*:</span><span class="sxs-lookup"><span data-stu-id="855ad-270">Reference documentation for *netsh.exe*:</span></span>

   * <span data-ttu-id="855ad-271">[Netsh Commands for Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx) (Comandi di Netsh per il protocollo HTTP)</span><span class="sxs-lookup"><span data-stu-id="855ad-271">[Netsh Commands for Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)</span></span>
   * <span data-ttu-id="855ad-272">[UrlPrefix Strings](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx) (Stringhe UrlPrefix)</span><span class="sxs-lookup"><span data-stu-id="855ad-272">[UrlPrefix Strings](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)</span></span>

1. <span data-ttu-id="855ad-273">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="855ad-273">Run the app.</span></span>

   <span data-ttu-id="855ad-274">Non sono necessari i privilegi di amministratore per eseguire l'app con binding a localhost tramite HTTP (non HTTPS) con un numero di porta maggiore di 1024.</span><span class="sxs-lookup"><span data-stu-id="855ad-274">Administrator privileges aren't required to run the app when binding to localhost using HTTP (not HTTPS) with a port number greater than 1024.</span></span> <span data-ttu-id="855ad-275">Per altre configurazioni (ad esempio, l'uso di un indirizzo IP locale o il binding sulla porta 443), eseguire l'app con i privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="855ad-275">For other configurations (for example, using a local IP address or binding to port 443), run the app with administrator privileges.</span></span>

   <span data-ttu-id="855ad-276">L'app risponde all'indirizzo IP pubblico del server.</span><span class="sxs-lookup"><span data-stu-id="855ad-276">The app responds at the server's public IP address.</span></span> <span data-ttu-id="855ad-277">In questo esempio, il server è raggiungibile da Internet al relativo indirizzo IP pubblico `104.214.79.47`.</span><span class="sxs-lookup"><span data-stu-id="855ad-277">In this example, the server is reached from the Internet at its public IP address of `104.214.79.47`.</span></span>

   <span data-ttu-id="855ad-278">In questo esempio viene usato un certificato di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="855ad-278">A development certificate is used in this example.</span></span> <span data-ttu-id="855ad-279">La pagina viene caricata in modo sicuro dopo aver ignorato l'avviso di certificato non attendibile del browser.</span><span class="sxs-lookup"><span data-stu-id="855ad-279">The page loads securely after bypassing the browser's untrusted certificate warning.</span></span>

   ![Finestra del browser che mostra la pagina di indice dell'app caricata](httpsys/_static/browser.png)

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="855ad-281">Scenari con server proxy e servizi di bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="855ad-281">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="855ad-282">Per le app ospitate da HTTP.sys che interagiscono con richieste da Internet o da una rete aziendale, potrebbero essere necessari interventi di configurazione aggiuntivi in caso di hosting dietro server proxy e servizi di bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="855ad-282">For apps hosted by HTTP.sys that interact with requests from the Internet or a corporate network, additional configuration might be required when hosting behind proxy servers and load balancers.</span></span> <span data-ttu-id="855ad-283">Per altre informazioni, vedere [Configurare ASP.NET Core per l'utilizzo di server proxy e servizi di bilanciamento del carico](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="855ad-283">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="855ad-284">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="855ad-284">Additional resources</span></span>

* [<span data-ttu-id="855ad-285">Abilitare l'autenticazione di Windows con HTTP. sys</span><span class="sxs-lookup"><span data-stu-id="855ad-285">Enable Windows Authentication with HTTP.sys</span></span>](xref:security/authentication/windowsauth#enable-windows-authentication-with-httpsys)
* [<span data-ttu-id="855ad-286">API di HTTP Server</span><span class="sxs-lookup"><span data-stu-id="855ad-286">HTTP Server API</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [<span data-ttu-id="855ad-287">Repository di GitHub aspnet/HttpSysServer (codice sorgente)</span><span class="sxs-lookup"><span data-stu-id="855ad-287">aspnet/HttpSysServer GitHub repository (source code)</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="855ad-288">L'host</span><span class="sxs-lookup"><span data-stu-id="855ad-288">The host</span></span>](xref:fundamentals/index#host)
* <xref:test/troubleshoot>
