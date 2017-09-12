---
title: Implementazione del server web WebListener in ASP.NET Core
author: rick-anderson
description: "Introduce WebListener, un server web per ASP.NET Core in Windows. Basato sul driver in modalità kernel HTTP. sys, WebListener è un'alternativa a Kestrel che può essere utilizzato per la connessione diretta a Internet senza IIS."
keywords: ASP.NET Core, WebListener, HttpListener, prefissi url, SSL
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: 0a7286e4-6428-424e-b5c4-5c98815cf61c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/weblistener
ms.openlocfilehash: 93e8b99e7fbac88aabd347c077d923214ba7aebe
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="b2fbc-105">Implementazione del server web WebListener in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b2fbc-105">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="b2fbc-106">Da [Tom Dykstra](https://github.com/tdykstra) e [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="b2fbc-106">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="b2fbc-107">In questo argomento si applica solo a ASP.NET di base 1. x.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-107">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="b2fbc-108">In ASP.NET 2.0 Core, è denominata WebListener [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="b2fbc-108">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="b2fbc-109">WebListener è un [server web per ASP.NET Core](index.md) che viene eseguita solo su Windows.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-109">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="b2fbc-110">Si basa sul [driver in modalità kernel HTTP. sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="b2fbc-110">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="b2fbc-111">WebListener è un'alternativa a [Kestrel](kestrel.md) che può essere utilizzato per la connessione diretta a Internet senza basarsi su IIS come server proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-111">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="b2fbc-112">In effetti, **WebListener non può essere utilizzato con IIS o IIS Express, che non è compatibile con il [ASP.NET Core modulo](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="b2fbc-112">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="b2fbc-113">Anche se WebListener è stata sviluppata per ASP.NET Core, può essere utilizzato direttamente in qualsiasi applicazione .NET Core o .NET Framework tramite il [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-113">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="b2fbc-114">WebListener supporta le funzionalità seguenti:</span><span class="sxs-lookup"><span data-stu-id="b2fbc-114">WebListener supports the following features:</span></span>

- [<span data-ttu-id="b2fbc-115">Autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="b2fbc-115">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="b2fbc-116">Condivisione delle porte</span><span class="sxs-lookup"><span data-stu-id="b2fbc-116">Port sharing</span></span>
- <span data-ttu-id="b2fbc-117">Utilizzo di HTTPS con SNI</span><span class="sxs-lookup"><span data-stu-id="b2fbc-117">HTTPS with SNI</span></span>
- <span data-ttu-id="b2fbc-118">HTTP/2 tramite TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="b2fbc-118">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="b2fbc-119">Trasmissione diretta del file</span><span class="sxs-lookup"><span data-stu-id="b2fbc-119">Direct file transmission</span></span>
- <span data-ttu-id="b2fbc-120">La memorizzazione nella cache di risposta</span><span class="sxs-lookup"><span data-stu-id="b2fbc-120">Response caching</span></span>
- <span data-ttu-id="b2fbc-121">WebSocket (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="b2fbc-121">WebSockets (Windows 8)</span></span>

<span data-ttu-id="b2fbc-122">Versioni supportate di Windows:</span><span class="sxs-lookup"><span data-stu-id="b2fbc-122">Supported Windows versions:</span></span>

- <span data-ttu-id="b2fbc-123">Windows 7 e Windows Server 2008 R2 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="b2fbc-123">Windows 7 and Windows Server 2008 R2 and later</span></span>

[<span data-ttu-id="b2fbc-124">Visualizzare o scaricare codice di esempio</span><span class="sxs-lookup"><span data-stu-id="b2fbc-124">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)

## <a name="when-to-use-weblistener"></a><span data-ttu-id="b2fbc-125">Quando utilizzare WebListener</span><span class="sxs-lookup"><span data-stu-id="b2fbc-125">When to use WebListener</span></span>

<span data-ttu-id="b2fbc-126">WebListener è utile per le distribuzioni in cui è necessario esporre il server direttamente a Internet senza l'utilizzo di IIS.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-126">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![Weblistener comunica direttamente con Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="b2fbc-128">Poiché è stato progettato su Http.Sys, WebListener non richiede un server proxy inverso per la protezione contro gli attacchi.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-128">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="b2fbc-129">Http.Sys è una tecnologia avanzata che consente di proteggere molti tipi di attacchi e fornisce l'affidabilità, sicurezza e la scalabilità di un server web con caratteristiche complete.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-129">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="b2fbc-130">IIS viene eseguito come un listener HTTP in Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-130">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="b2fbc-131">WebListener è anche una buona scelta per le distribuzioni interne quando è necessaria una delle funzionalità che offerte che non è possibile ottenere utilizzando Kestrel.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-131">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![Weblistener comunica direttamente con la rete interna](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="b2fbc-133">Come usare WebListener</span><span class="sxs-lookup"><span data-stu-id="b2fbc-133">How to use WebListener</span></span>

<span data-ttu-id="b2fbc-134">Di seguito viene fornita una panoramica delle attività di configurazione per il sistema operativo host e l'applicazione ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-134">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="b2fbc-135">Configurare Windows Server</span><span class="sxs-lookup"><span data-stu-id="b2fbc-135">Configure Windows Server</span></span>

* <span data-ttu-id="b2fbc-136">Installare la versione di .NET che richiede l'applicazione, ad esempio [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) o .NET Framework 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-136">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="b2fbc-137">Pre-registrare prefissi URL per associare WebListener e configurare i certificati SSL</span><span class="sxs-lookup"><span data-stu-id="b2fbc-137">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="b2fbc-138">Se si non pre-registrare prefissi URL in Windows, è necessario eseguire l'applicazione con privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-138">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="b2fbc-139">L'unica eccezione è se si associa a localhost tramite HTTP (non HTTPS) con un numero di porta maggiore di 1024; In questo caso i privilegi di amministratore non sono necessari.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-139">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="b2fbc-140">Per informazioni dettagliate, vedere [come pre-registrare prefissi e configurare SSL](#preregister-url-prefixes-and-configure-ssl) più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-140">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="b2fbc-141">Aprire le porte del firewall per consentire il traffico raggiungere WebListener.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-141">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="b2fbc-142">È possibile utilizzare netsh.exe o [i cmdlet di PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="b2fbc-142">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="b2fbc-143">Sono inoltre disponibili [impostazioni del Registro di sistema Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="b2fbc-143">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="b2fbc-144">Configurare l'applicazione ASP.NET di base</span><span class="sxs-lookup"><span data-stu-id="b2fbc-144">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="b2fbc-145">Installare il pacchetto NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span><span class="sxs-lookup"><span data-stu-id="b2fbc-145">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="b2fbc-146">Viene inoltre installato [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) come dipendenza.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-146">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="b2fbc-147">Chiamare il [ `UseWebListener` ](https://docs.microsoft.com/aspnet/core/api) metodo di estensione su [WebHostBuilder](https://docs.microsoft.com/aspnet/core/api) nel `Main` metodo, specificando qualsiasi WebListener [opzioni](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) e [ impostazioni](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) che è necessario, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="b2fbc-147">Call the [`UseWebListener`](https://docs.microsoft.com/aspnet/core/api) extension method on [WebHostBuilder](https://docs.microsoft.com/aspnet/core/api) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="b2fbc-148">Configurare gli URL e le porte per l'attesa su</span><span class="sxs-lookup"><span data-stu-id="b2fbc-148">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="b2fbc-149">Per impostazione predefinita ASP.NET Core associa a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-149">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="b2fbc-150">Per configurare le porte e i prefissi URL, è possibile utilizzare il `UseURLs` metodo di estensione, il `urls` argomento della riga di comando o il sistema di configurazione di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-150">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="b2fbc-151">Per ulteriori informazioni, vedere [Hosting](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="b2fbc-151">For more information, see [Hosting](../../fundamentals/hosting.md).</span></span>

  <span data-ttu-id="b2fbc-152">Usa Listener Web il [formati di stringa di prefisso Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="b2fbc-152">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="b2fbc-153">Non sono previsti requisiti del formato di stringa di prefisso WebListener specifici.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-153">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!NOTE]
  > <span data-ttu-id="b2fbc-154">Assicurarsi di specificare le stesse stringhe di prefisso in `UseUrls` che pre-registrare il server.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-154">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="b2fbc-155">Verificare che l'applicazione non è configurato per l'esecuzione di IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-155">Make sure your application is not configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="b2fbc-156">In Visual Studio, il profilo di avvio predefinito è per IIS Express.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-156">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="b2fbc-157">Per eseguire il progetto come applicazione console è necessario modificare manualmente il profilo selezionato, come illustrato nella schermata seguente.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-157">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![Selezionare il profilo di applicazione console](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="b2fbc-159">Come usare WebListener all'esterno di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b2fbc-159">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="b2fbc-160">Installare il [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-160">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="b2fbc-161">[Pre-registrare prefissi URL per associare WebListener e configurare i certificati SSL](#preregister-url-prefixes-and-configure-ssl) come farebbe per l'uso in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-161">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="b2fbc-162">Sono inoltre disponibili [impostazioni del Registro di sistema Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="b2fbc-162">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="b2fbc-163">Di seguito è riportato un esempio di codice che illustra WebListener uso all'esterno di ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="b2fbc-163">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

```csharp
var settings = new WebListenerSettings();
settings.UrlPrefixes.Add("http://localhost:8080");

using (WebListener listener = new WebListener(settings))
{
    listener.Start();

    while (true)
    {
        var context = await listener.AcceptAsync();
        byte[] bytes = Encoding.ASCII.GetBytes("Hello World: " + DateTime.Now);
        context.Response.ContentLength = bytes.Length;
        context.Response.ContentType = "text/plain";

        await context.Response.Body.WriteAsync(bytes, 0, bytes.Length);
        context.Dispose();
    }
}
```

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="b2fbc-164">Pre-registrare prefissi URL e configurare SSL</span><span class="sxs-lookup"><span data-stu-id="b2fbc-164">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="b2fbc-165">Sia IIS sia WebListener si basano su driver sottostante in modalità kernel HTTP. sys in ascolto delle richieste e di elaborazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-165">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="b2fbc-166">In IIS, la gestione dell'interfaccia utente fornisce un modo semplice per configurare tutti gli elementi.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-166">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="b2fbc-167">Tuttavia, se si utilizza WebListener è necessario configurare manualmente Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-167">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="b2fbc-168">Lo strumento predefinito per questo scopo è netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-168">The built-in tool for doing that is netsh.exe.</span></span> 

<span data-ttu-id="b2fbc-169">È necessario utilizzare netsh.exe per attività più comuni sono riservare i prefissi URL e l'assegnazione di certificati SSL.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-169">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="b2fbc-170">NetSh.exe non è uno strumento facile da usare per utenti meno esperti.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-170">NetSh.exe is not an easy tool to use for beginners.</span></span> <span data-ttu-id="b2fbc-171">L'esempio seguente mostra il livello minimo necessario per riservare i prefissi URL per le porte 80 e 443:</span><span class="sxs-lookup"><span data-stu-id="b2fbc-171">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="b2fbc-172">Nell'esempio seguente viene illustrato come assegnare un certificato SSL:</span><span class="sxs-lookup"><span data-stu-id="b2fbc-172">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="b2fbc-173">Di seguito è riportata la documentazione di riferimento ufficiale:</span><span class="sxs-lookup"><span data-stu-id="b2fbc-173">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="b2fbc-174">Comandi Netsh per Hypertext Transfer Protocol (HTTP)</span><span class="sxs-lookup"><span data-stu-id="b2fbc-174">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="b2fbc-175">UrlPrefix stringhe</span><span class="sxs-lookup"><span data-stu-id="b2fbc-175">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="b2fbc-176">Le risorse seguenti forniscono istruzioni dettagliate per scenari diversi.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-176">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="b2fbc-177">Gli articoli che fanno riferimento a `HttpListener` si applicano ugualmente a `WebListener`, come entrambe basate su HTTP. sys.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-177">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="b2fbc-178">Procedura: configurare una porta con un certificato SSL</span><span class="sxs-lookup"><span data-stu-id="b2fbc-178">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="b2fbc-179">[Le comunicazioni HTTPS - HttpListener basato su host e la certificazione Client](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) questo è un blog di terze parti e si è piuttosto precedente ma si dispone ancora di informazioni utili.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-179">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="b2fbc-180">[Procedura: HttpListener utilizzando questa procedura dettagliata o un Http Server codice non gestito (C++) come Server semplice SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) anche questo va un blog precedente con informazioni utili.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-180">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="b2fbc-181">Come impostare un WebListener Core .NET con SSL?</span><span class="sxs-lookup"><span data-stu-id="b2fbc-181">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="b2fbc-182">Ecco alcuni strumenti di terze parti che possono essere più la riga di comando netsh.exe semplici da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-182">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="b2fbc-183">Questi non vengono forniti da o approvati da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-183">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="b2fbc-184">Gli strumenti di Esegui come amministratore per impostazione predefinita, poiché netsh.exe stesso richiede privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-184">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="b2fbc-185">[http.sys Manager](http://httpsysmanager.codeplex.com/) fornisce un'interfaccia utente per l'elenco e la configurazione di opzioni e i certificati SSL, le prenotazioni del prefisso ed elenchi di certificati attendibili.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-185">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="b2fbc-186">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) consente di elencare o configurare i certificati SSL e i prefissi URL.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-186">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="b2fbc-187">L'interfaccia utente è più precise di http.sys Manager ed espone alcuni ulteriori opzioni di configurazione, ma in caso contrario fornisce funzionalità simili.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-187">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="b2fbc-188">Non è possibile creare un nuovo elenco di certificati attendibili (CTL), ma può assegnare a quelli esistenti.</span><span class="sxs-lookup"><span data-stu-id="b2fbc-188">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="b2fbc-189">Per la generazione di certificati SSL autofirmati, Microsoft fornisce gli strumenti da riga di comando: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) e il cmdlet PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="b2fbc-189">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="b2fbc-190">Sono inoltre strumenti di terze parti dell'interfaccia utente che rendono più semplice per generare i certificati SSL autofirmati:</span><span class="sxs-lookup"><span data-stu-id="b2fbc-190">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="b2fbc-191">SelfCert</span><span class="sxs-lookup"><span data-stu-id="b2fbc-191">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="b2fbc-192">Interfaccia utente di Makecert</span><span class="sxs-lookup"><span data-stu-id="b2fbc-192">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="b2fbc-193">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="b2fbc-193">Next steps</span></span>

<span data-ttu-id="b2fbc-194">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="b2fbc-194">For more information, see the following resources:</span></span>

* [<span data-ttu-id="b2fbc-195">Applicazione di esempio per questo articolo</span><span class="sxs-lookup"><span data-stu-id="b2fbc-195">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="b2fbc-196">Codice sorgente WebListener</span><span class="sxs-lookup"><span data-stu-id="b2fbc-196">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="b2fbc-197">Hosting</span><span class="sxs-lookup"><span data-stu-id="b2fbc-197">Hosting</span></span>](../hosting.md)
