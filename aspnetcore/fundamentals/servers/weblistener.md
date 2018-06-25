---
title: Implementazione del server Web WebListener in ASP.NET Core
author: rick-anderson
description: Informazioni su WebListener, un server Web per ASP.NET Core in Windows che può essere usato per la connessione diretta a Internet senza IIS.
ms.author: riande
ms.date: 03/13/2018
uid: fundamentals/servers/weblistener
ms.openlocfilehash: 68aea99d6ce6af12655ef5fdb13130e9279e448a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274869"
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="a232f-103">Implementazione del server Web WebListener in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a232f-103">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="a232f-104">[Tom Dykstra](https://github.com/tdykstra) e [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="a232f-104">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="a232f-105">Questo argomento si applica solo ad ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="a232f-105">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="a232f-106">In ASP.NET Core 2.0 WebListener è denominato [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="a232f-106">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="a232f-107">WebListener è un [server Web per ASP.NET Core](index.md) che può essere eseguito solo in Windows.</span><span class="sxs-lookup"><span data-stu-id="a232f-107">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="a232f-108">È basato sul [driver in modalità kernel Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="a232f-108">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="a232f-109">WebListener è un'alternativa a [Kestrel](kestrel.md) che consente la connessione diretta a Internet dover usare IIS come server proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="a232f-109">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="a232f-110">Infatti **WebListener non può essere usato con IIS o IIS Express, poiché non è compatibile con il [modulo ASP.NET Core](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="a232f-110">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="a232f-111">Nonostante WebListener sia stato sviluppato per ASP.NET Core, può essere usato direttamente in tutte le applicazioni .NET Core o .NET Framework tramite il pacchetto NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).</span><span class="sxs-lookup"><span data-stu-id="a232f-111">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="a232f-112">WebListener supporta le funzionalità seguenti:</span><span class="sxs-lookup"><span data-stu-id="a232f-112">WebListener supports the following features:</span></span>

- [<span data-ttu-id="a232f-113">Autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="a232f-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="a232f-114">Condivisione delle porte</span><span class="sxs-lookup"><span data-stu-id="a232f-114">Port sharing</span></span>
- <span data-ttu-id="a232f-115">HTTPS con SNI</span><span class="sxs-lookup"><span data-stu-id="a232f-115">HTTPS with SNI</span></span>
- <span data-ttu-id="a232f-116">HTTP/2 su TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="a232f-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="a232f-117">Trasmissione diretta dei file</span><span class="sxs-lookup"><span data-stu-id="a232f-117">Direct file transmission</span></span>
- <span data-ttu-id="a232f-118">Memorizzazione nella cache delle risposte</span><span class="sxs-lookup"><span data-stu-id="a232f-118">Response caching</span></span>
- <span data-ttu-id="a232f-119">WebSocket (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="a232f-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="a232f-120">Versioni di Windows supportate:</span><span class="sxs-lookup"><span data-stu-id="a232f-120">Supported Windows versions:</span></span>

- <span data-ttu-id="a232f-121">Windows 7 e Windows Server 2008 R2 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="a232f-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="a232f-122">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a232f-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="a232f-123">Quando usare WebListener</span><span class="sxs-lookup"><span data-stu-id="a232f-123">When to use WebListener</span></span>

<span data-ttu-id="a232f-124">WebListener è utile per le distribuzioni in cui è necessario esporre il server direttamente a Internet senza usare IIS.</span><span class="sxs-lookup"><span data-stu-id="a232f-124">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![WebListener comunica direttamente con Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="a232f-126">Poiché è basato su Http.Sys, WebListener non richiede un server proxy inverso per la protezione dagli attacchi.</span><span class="sxs-lookup"><span data-stu-id="a232f-126">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="a232f-127">WebListener è una tecnologia avanzata che protegge da molti tipi di attacchi e offre l'affidabilità, la sicurezza e la scalabilità di un server Web con funzionalità complete.</span><span class="sxs-lookup"><span data-stu-id="a232f-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="a232f-128">IIS viene eseguito come listener HTTP in Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="a232f-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="a232f-129">WebListener è anche una scelta ottimale per le distribuzioni interne quando è necessario usare una funzionalità non disponibile in Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a232f-129">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![Weblistener comunica direttamente con la rete interna](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="a232f-131">Come usare WebListener</span><span class="sxs-lookup"><span data-stu-id="a232f-131">How to use WebListener</span></span>

<span data-ttu-id="a232f-132">Di seguito sono descritte le attività di configurazione per il sistema operativo host e l'applicazione ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a232f-132">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="a232f-133">Configurare Windows Server</span><span class="sxs-lookup"><span data-stu-id="a232f-133">Configure Windows Server</span></span>

* <span data-ttu-id="a232f-134">Installare la versione di .NET richiesta dall'applicazione, ad esempio [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) o .NET Framework 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="a232f-134">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="a232f-135">Pre-registrare i prefissi URL per l'associazione a WebListener e configurare i certificati SSL</span><span class="sxs-lookup"><span data-stu-id="a232f-135">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="a232f-136">Se non si pre-registrano i prefissi URL in Windows, è necessario eseguire l'applicazione con i privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="a232f-136">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="a232f-137">L'unica eccezione si verifica quando si esegue l'associazione a localhost usando HTTP (non HTTPS) con un numero di porta maggiore di 1024; in tal caso, i privilegi di amministratore non sono necessari.</span><span class="sxs-lookup"><span data-stu-id="a232f-137">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="a232f-138">Per informazioni dettagliate, vedere [Pre-registrare i prefissi URL e configurare SSL](#preregister-url-prefixes-and-configure-ssl) più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="a232f-138">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="a232f-139">Aprire le porte del firewall per consentire al traffico di raggiungere WebListener.</span><span class="sxs-lookup"><span data-stu-id="a232f-139">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="a232f-140">È possibile usare netsh.exe o i [cmdlet di PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="a232f-140">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="a232f-141">Sono anche disponibili le [Impostazioni del Registro di sistema HTTP. sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="a232f-141">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="a232f-142">Configurare l'applicazione ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a232f-142">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="a232f-143">Installare il pacchetto NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/),</span><span class="sxs-lookup"><span data-stu-id="a232f-143">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="a232f-144">che installa anche [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) come dipendenza.</span><span class="sxs-lookup"><span data-stu-id="a232f-144">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="a232f-145">Chiamare il metodo di estensione `UseWebListener` in [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) nel metodo `Main`, specificando le [opzioni](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) e le [impostazioni](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) necessarie di WebListener, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="a232f-145">Call the `UseWebListener` extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="a232f-146">Configurare gli URL e le porte su cui eseguire l'ascolto</span><span class="sxs-lookup"><span data-stu-id="a232f-146">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="a232f-147">Per impostazione predefinita, ASP.NET Core è associato a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a232f-147">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="a232f-148">Per configurare le porte e i prefissi URL, è possibile usare il metodo di estensione `UseURLs`, l'argomento della riga di comando `urls` o il sistema di configurazione di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a232f-148">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="a232f-149">Per altre informazioni, vedere [Hosting in ASP.NET Core(xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="a232f-149">For more information, see [Host in ASP.NET Core(xref:fundamentals/host/index).</span></span>

  <span data-ttu-id="a232f-150">WebListener usa i [formati di stringa di prefisso di Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="a232f-150">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="a232f-151">WebListener non richiede un formato specifico per le stringhe di prefisso.</span><span class="sxs-lookup"><span data-stu-id="a232f-151">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!WARNING]
  > <span data-ttu-id="a232f-152">Le associazioni con caratteri jolly di livello superiore (`http://*:80/` e `http://+:80`) **non** devono essere usate,</span><span class="sxs-lookup"><span data-stu-id="a232f-152">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="a232f-153">poiché possono introdurre vulnerabilità a livello di sicurezza nell'app.</span><span class="sxs-lookup"><span data-stu-id="a232f-153">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="a232f-154">Questo concetto vale sia per i caratteri jolly sicuri che vulnerabili.</span><span class="sxs-lookup"><span data-stu-id="a232f-154">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="a232f-155">Usare nomi host espliciti al posto di caratteri jolly.</span><span class="sxs-lookup"><span data-stu-id="a232f-155">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="a232f-156">L'associazione con caratteri jolly del sottodominio (ad esempio, `*.mysub.com`) non costituisce un rischio per la sicurezza se viene controllato l'intero dominio padre (a differenza di `*.com`, che è vulnerabile).</span><span class="sxs-lookup"><span data-stu-id="a232f-156">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="a232f-157">Vedere la [sezione 5.4 di RFC7230](https://tools.ietf.org/html/rfc7230#section-5.4) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="a232f-157">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

  > [!NOTE]
  > <span data-ttu-id="a232f-158">Assicurarsi di specificare le stesse stringhe di prefisso in `UseUrls` che vengono pre-registrate nel server.</span><span class="sxs-lookup"><span data-stu-id="a232f-158">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="a232f-159">Verificare che l'applicazione non sia configurata per l'esecuzione di IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="a232f-159">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="a232f-160">In Visual Studio il profilo di avvio predefinito è per IIS Express.</span><span class="sxs-lookup"><span data-stu-id="a232f-160">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="a232f-161">Per eseguire il progetto come applicazione console, modificare manualmente il profilo selezionato, come illustrato nello screenshot seguente.</span><span class="sxs-lookup"><span data-stu-id="a232f-161">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![Selezionare il profilo dell'applicazione console](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="a232f-163">Come usare WebListener all'esterno di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a232f-163">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="a232f-164">Installare il pacchetto NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).</span><span class="sxs-lookup"><span data-stu-id="a232f-164">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="a232f-165">[Pre-registrare i prefissi URL per l'associazione a WebListener e configurare i certificati SSL](#preregister-url-prefixes-and-configure-ssl), come se venissero usati in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a232f-165">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="a232f-166">Sono anche disponibili le [Impostazioni del Registro di sistema HTTP. sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="a232f-166">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="a232f-167">Di seguito è riportato un esempio di codice che illustra l'uso di WebListener all'esterno di ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="a232f-167">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="a232f-168">Pre-registrare i prefissi URL e configurare SSL</span><span class="sxs-lookup"><span data-stu-id="a232f-168">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="a232f-169">Sia IIS sia WebListener si basano sul driver in modalità kernel Http. sys sottostante per l'ascolto delle richieste e l'elaborazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="a232f-169">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="a232f-170">In IIS l'interfaccia utente di gestione consente di configurare tutti gli elementi in modo relativamente semplice.</span><span class="sxs-lookup"><span data-stu-id="a232f-170">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="a232f-171">Se si usa WebListener, Http.Sys deve comunque essere configurato manualmente.</span><span class="sxs-lookup"><span data-stu-id="a232f-171">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="a232f-172">Lo strumento predefinito per questo scopo è netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="a232f-172">The built-in tool for doing that's netsh.exe.</span></span> 

<span data-ttu-id="a232f-173">Netsh.exe viene comunemente usato per riservare i prefissi URL e assegnare i certificati SSL.</span><span class="sxs-lookup"><span data-stu-id="a232f-173">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="a232f-174">NetSh.exe non è uno strumento facile da usare per gli utenti meno esperti.</span><span class="sxs-lookup"><span data-stu-id="a232f-174">NetSh.exe isn't an easy tool to use for beginners.</span></span> <span data-ttu-id="a232f-175">L'esempio seguente illustra i prefissi URL minimi necessari da riservare per le porte 80 e 443:</span><span class="sxs-lookup"><span data-stu-id="a232f-175">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="a232f-176">L'esempio seguente illustra come assegnare un certificato SSL:</span><span class="sxs-lookup"><span data-stu-id="a232f-176">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="a232f-177">Di seguito è riportata la documentazione di riferimento ufficiale:</span><span class="sxs-lookup"><span data-stu-id="a232f-177">Here is the official reference documentation:</span></span>

* <span data-ttu-id="a232f-178">[Netsh Commands for Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx) (Comandi di Netsh per il protocollo HTTP)</span><span class="sxs-lookup"><span data-stu-id="a232f-178">[Netsh Commands for Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)</span></span>
* <span data-ttu-id="a232f-179">[UrlPrefix Strings](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx) (Stringhe UrlPrefix)</span><span class="sxs-lookup"><span data-stu-id="a232f-179">[UrlPrefix Strings](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)</span></span>

<span data-ttu-id="a232f-180">Le risorse seguenti offrono istruzioni dettagliate per scenari diversi.</span><span class="sxs-lookup"><span data-stu-id="a232f-180">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="a232f-181">Gli articoli che fanno riferimento a `HttpListener` si applicano anche a `WebListener` poiché sono entrambi basati su Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="a232f-181">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="a232f-182">Procedura: Configurare una porta con un certificato SSL</span><span class="sxs-lookup"><span data-stu-id="a232f-182">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="a232f-183">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) (Comunicazione HTTPS - Hosting e certificazione client basati su HttpListener) Blog di terze parti non recente ma che contiene informazioni utili.</span><span class="sxs-lookup"><span data-stu-id="a232f-183">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="a232f-184">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) (Procedura: Procedura dettagliata per l'uso di HttpListener o di codice non gestito del server HTTP (C++) come SSL Simple Server) Blog non recente contenente informazioni utili.</span><span class="sxs-lookup"><span data-stu-id="a232f-184">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* <span data-ttu-id="a232f-185">[How Do I Set Up A .NET Core WebListener With SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/) (Come configurare WebListener in .NET Core con SSL)</span><span class="sxs-lookup"><span data-stu-id="a232f-185">[How Do I Set Up A .NET Core WebListener With SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)</span></span>

<span data-ttu-id="a232f-186">Di seguito sono elencati alcuni strumenti di terze parti che possono risultare più semplici da usare rispetto alla riga di comando di netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="a232f-186">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="a232f-187">Questi strumenti non sono offerti o promossi da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="a232f-187">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="a232f-188">Gli strumenti vengono eseguiti come amministratore per impostazione predefinita, poiché anche netsh.exe richiede i privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="a232f-188">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="a232f-189">[http.sys Manager](http://httpsysmanager.codeplex.com/) offre l'interfaccia utente per visualizzare e configurare i certificati e le opzioni SSL, le prenotazioni dei prefissi e gli elenchi scopi consentiti ai certificati.</span><span class="sxs-lookup"><span data-stu-id="a232f-189">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="a232f-190">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) consente di visualizzare o configurare i certificati SSL e i prefissi URL.</span><span class="sxs-lookup"><span data-stu-id="a232f-190">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="a232f-191">L'interfaccia utente è più avanzata di http.sys Manager ed espone un numero maggiore di opzioni di configurazione ma offre comunque una funzionalità simile.</span><span class="sxs-lookup"><span data-stu-id="a232f-191">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="a232f-192">Non consente di creare un nuovo elenco scopi consentiti ai certificati (CTL), ma consente di assegnare elenchi esistenti.</span><span class="sxs-lookup"><span data-stu-id="a232f-192">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="a232f-193">Per la generazione di certificati SSL autofirmati, Microsoft offre gli strumenti da riga di comando: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) e il cmdlet di PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="a232f-193">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="a232f-194">Per generare facilmente i certificati SSL autofirmati, è possibile usare anche gli strumenti di interfaccia utente di terze parti:</span><span class="sxs-lookup"><span data-stu-id="a232f-194">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="a232f-195">SelfCert</span><span class="sxs-lookup"><span data-stu-id="a232f-195">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="a232f-196">Makecert UI</span><span class="sxs-lookup"><span data-stu-id="a232f-196">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="a232f-197">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="a232f-197">Next steps</span></span>

<span data-ttu-id="a232f-198">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="a232f-198">For more information, see the following resources:</span></span>

* [<span data-ttu-id="a232f-199">App di esempio per questo articolo</span><span class="sxs-lookup"><span data-stu-id="a232f-199">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="a232f-200">Codice sorgente di WebListener</span><span class="sxs-lookup"><span data-stu-id="a232f-200">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="a232f-201">Hosting</span><span class="sxs-lookup"><span data-stu-id="a232f-201">Hosting</span></span>](xref:fundamentals/host/index)
