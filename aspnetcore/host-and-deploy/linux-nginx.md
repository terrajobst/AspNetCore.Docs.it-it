---
title: Hosting di ASP.NET Core in Linux con Nginx
author: guardrex
description: Informazioni su come configurare Nginx come proxy inverso in Ubuntu 16.04 per inoltrare il traffico HTTP a un'app Web ASP.NET Core in esecuzione su Kestrel.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/05/2019
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: c6ae86ec9ac54ddf2d487fd72156199fbdd029ef
ms.sourcegitcommit: 6628cd23793b66e4ce88788db641a5bbf470c3c1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73659868"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="a5a74-103">Hosting di ASP.NET Core in Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="a5a74-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="a5a74-104">Di [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="a5a74-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="a5a74-105">Questa guida descrive come configurare un ambiente ASP.NET Core pronto per la produzione in un server Ubuntu 16.04.</span><span class="sxs-lookup"><span data-stu-id="a5a74-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="a5a74-106">Queste istruzioni si applicano probabilmente anche alle versioni più recenti di Ubuntu, sebbene non siano state testate con le versioni più recenti.</span><span class="sxs-lookup"><span data-stu-id="a5a74-106">These instructions likely work with newer versions of Ubuntu, but the instructions haven't been tested with newer versions.</span></span>

<span data-ttu-id="a5a74-107">Per informazioni su altre distribuzioni Linux supportate da ASP.NET Core, vedere [Prerequisiti per .NET Core in Linux](/dotnet/core/linux-prerequisites).</span><span class="sxs-lookup"><span data-stu-id="a5a74-107">For information on other Linux distributions supported by ASP.NET Core, see [Prerequisites for .NET Core on Linux](/dotnet/core/linux-prerequisites).</span></span>

> [!NOTE]
> <span data-ttu-id="a5a74-108">Per Ubuntu 14.04, è consigliabile *supervisord* come soluzione per il monitoraggio del processo Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a5a74-108">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="a5a74-109">*systemd* non è disponibile in Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="a5a74-109">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="a5a74-110">Per le istruzioni per Ubuntu 14.04, vedere la [versione precedente di questo argomento](https://github.com/aspnet/AspNetCore.Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="a5a74-110">For Ubuntu 14.04 instructions, see the [previous version of this topic](https://github.com/aspnet/AspNetCore.Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="a5a74-111">In questa guida:</span><span class="sxs-lookup"><span data-stu-id="a5a74-111">This guide:</span></span>

* <span data-ttu-id="a5a74-112">Posizionare un'app ASP.NET Core esistente dietro un server proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="a5a74-112">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="a5a74-113">Configurare il server proxy inverso per l'inoltro delle richieste al server Web Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a5a74-113">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="a5a74-114">Verificare che l'app Web venga eseguita all'avvio come daemon.</span><span class="sxs-lookup"><span data-stu-id="a5a74-114">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="a5a74-115">Configurare uno strumento di gestione del processo per consentire il riavvio dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="a5a74-115">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a5a74-116">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="a5a74-116">Prerequisites</span></span>

1. <span data-ttu-id="a5a74-117">Accedere a un server Ubuntu 16.04 con un account utente standard con privilegio sudo.</span><span class="sxs-lookup"><span data-stu-id="a5a74-117">Access to an Ubuntu 16.04 server with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="a5a74-118">Installare il runtime .NET Core nel server.</span><span class="sxs-lookup"><span data-stu-id="a5a74-118">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="a5a74-119">Visitare la [pagina di tutti i download per .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="a5a74-119">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="a5a74-120">Selezionare il runtime non di anteprima più recente nell'elenco **Runtime**.</span><span class="sxs-lookup"><span data-stu-id="a5a74-120">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="a5a74-121">Selezionare e seguire le istruzioni per Ubuntu relative alla versione di Ubuntu del server.</span><span class="sxs-lookup"><span data-stu-id="a5a74-121">Select and follow the instructions for Ubuntu that match the Ubuntu version of the server.</span></span>
1. <span data-ttu-id="a5a74-122">Un'app ASP.NET Core esistente.</span><span class="sxs-lookup"><span data-stu-id="a5a74-122">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="a5a74-123">Pubblicare e copiare l'app</span><span class="sxs-lookup"><span data-stu-id="a5a74-123">Publish and copy over the app</span></span>

<span data-ttu-id="a5a74-124">Configurare l'app per una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="a5a74-124">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="a5a74-125">Se l'app viene eseguita in locale e non è configurata per connessioni sicure (HTTPS), adottare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="a5a74-125">If the app is run locally and isn't configured to make secure connections (HTTPS), adopt either of the following approaches:</span></span>

* <span data-ttu-id="a5a74-126">Configurare l'app per la gestione di connessioni locali sicure.</span><span class="sxs-lookup"><span data-stu-id="a5a74-126">Configure the app to handle secure local connections.</span></span> <span data-ttu-id="a5a74-127">Per altre informazioni, vedere la sezione [Configurazione HTTPS](#https-configuration).</span><span class="sxs-lookup"><span data-stu-id="a5a74-127">For more information, see the [HTTPS configuration](#https-configuration) section.</span></span>
* <span data-ttu-id="a5a74-128">Rimuovere `https://localhost:5001` (se presente) dalla proprietà `applicationUrl` nel file *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a5a74-128">Remove `https://localhost:5001` (if present) from the `applicationUrl` property in the *Properties/launchSettings.json* file.</span></span>

<span data-ttu-id="a5a74-129">Eseguire [dotnet publish](/dotnet/core/tools/dotnet-publish) dall'ambiente di sviluppo per creare il pacchetto di un'app in una directory (ad esempio, *bin/Release/&lt;target_framework_moniker&gt;/publish*) che può essere eseguita nel server:</span><span class="sxs-lookup"><span data-stu-id="a5a74-129">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```dotnetcli
dotnet publish --configuration Release
```

<span data-ttu-id="a5a74-130">È anche possibile pubblicare l'app come [distribuzione completa](/dotnet/core/deploying/#self-contained-deployments-scd) se si preferisce non mantenere il runtime .NET Core nel server.</span><span class="sxs-lookup"><span data-stu-id="a5a74-130">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="a5a74-131">Copiare l'app ASP.NET Core sul server usando uno strumento integrato nel flusso di lavoro dell'organizzazione, ad esempio SCP o FTP.</span><span class="sxs-lookup"><span data-stu-id="a5a74-131">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="a5a74-132">Le app Web si trovano in genere nella directory *var*, ad esempio, *var/www/helloapp*.</span><span class="sxs-lookup"><span data-stu-id="a5a74-132">It's common to locate web apps under the *var* directory (for example, *var/www/helloapp*).</span></span>

> [!NOTE]
> <span data-ttu-id="a5a74-133">In uno scenario di distribuzione di produzione un flusso di lavoro di integrazione continua esegue le operazioni di pubblicazione dell'app e di copia degli asset nel server.</span><span class="sxs-lookup"><span data-stu-id="a5a74-133">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

<span data-ttu-id="a5a74-134">Eseguire il test dell'app:</span><span class="sxs-lookup"><span data-stu-id="a5a74-134">Test the app:</span></span>

1. <span data-ttu-id="a5a74-135">Dalla riga di comando, eseguire l'app: `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="a5a74-135">From the command line, run the app: `dotnet <app_assembly>.dll`.</span></span>
1. <span data-ttu-id="a5a74-136">In un browser passare a `http://<serveraddress>:<port>` per verificare se l'app funziona in Linux in locale.</span><span class="sxs-lookup"><span data-stu-id="a5a74-136">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux locally.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="a5a74-137">Configurare un server proxy inverso</span><span class="sxs-lookup"><span data-stu-id="a5a74-137">Configure a reverse proxy server</span></span>

<span data-ttu-id="a5a74-138">Un proxy inverso è una configurazione comune per la gestione delle app Web dinamiche.</span><span class="sxs-lookup"><span data-stu-id="a5a74-138">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="a5a74-139">Un proxy inverso termina la richiesta HTTP e la inoltra all'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a5a74-139">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="use-a-reverse-proxy-server"></a><span data-ttu-id="a5a74-140">Usare un server proxy inverso</span><span class="sxs-lookup"><span data-stu-id="a5a74-140">Use a reverse proxy server</span></span>

<span data-ttu-id="a5a74-141">Kestrel funziona perfettamente per la gestione del contenuto dinamico da ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a5a74-141">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="a5a74-142">Tuttavia, le funzionalità di gestione Web sono meno avanzate rispetto a quelle offerte da server come IIS, Apache o Nginx.</span><span class="sxs-lookup"><span data-stu-id="a5a74-142">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="a5a74-143">Un server proxy inverso è in grado di eseguire l'offload di attività come la gestione del contenuto statico, la memorizzazione nella cache delle richieste, la compressione delle richieste e la terminazione HTTPS dal server HTTP.</span><span class="sxs-lookup"><span data-stu-id="a5a74-143">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and HTTPS termination from the HTTP server.</span></span> <span data-ttu-id="a5a74-144">Un server proxy inverso può risiedere in un computer dedicato o essere distribuito insieme a un server HTTP.</span><span class="sxs-lookup"><span data-stu-id="a5a74-144">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="a5a74-145">Ai fini di questa guida viene usata una singola istanza di Nginx.</span><span class="sxs-lookup"><span data-stu-id="a5a74-145">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="a5a74-146">Viene eseguito sullo stesso server, insieme al server HTTP.</span><span class="sxs-lookup"><span data-stu-id="a5a74-146">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="a5a74-147">In base ai requisiti, è possibile scegliere una configurazione diversa.</span><span class="sxs-lookup"><span data-stu-id="a5a74-147">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="a5a74-148">Poiché le richieste vengono inoltrate dal proxy inverso, usare il [middleware delle intestazioni inoltrate](xref:host-and-deploy/proxy-load-balancer) dal pacchetto [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/).</span><span class="sxs-lookup"><span data-stu-id="a5a74-148">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="a5a74-149">Il middleware aggiorna `Request.Scheme` usando l'intestazione `X-Forwarded-Proto`, in modo che gli URI di reindirizzamento e altri criteri di sicurezza funzionino correttamente.</span><span class="sxs-lookup"><span data-stu-id="a5a74-149">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="a5a74-150">Qualsiasi componente che dipende dallo schema, ad esempio l'autenticazione, la generazione di collegamenti, i reindirizzamenti e la georilevazione, deve essere inserito dopo aver richiamato il middleware delle intestazioni inoltrate.</span><span class="sxs-lookup"><span data-stu-id="a5a74-150">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="a5a74-151">Come regola generale,il middleware delle intestazioni inoltrate deve essere eseguito prima degli altri middleware, ad eccezione del middleware di diagnostica e gestione degli errori.</span><span class="sxs-lookup"><span data-stu-id="a5a74-151">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="a5a74-152">Questo ordine garantisce che il middleware basato sulle intestazioni inoltrate possa usare i valori di intestazione per l'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="a5a74-152">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

<span data-ttu-id="a5a74-153">Richiamare il metodo <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> in `Startup.Configure` prima di chiamare <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> o un middleware con schema di autenticazione simile.</span><span class="sxs-lookup"><span data-stu-id="a5a74-153">Invoke the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method in `Startup.Configure` before calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> or similar authentication scheme middleware.</span></span> <span data-ttu-id="a5a74-154">Configurare il middleware per l'inoltro delle intestazioni `X-Forwarded-For` e `X-Forwarded-Proto`:</span><span class="sxs-lookup"><span data-stu-id="a5a74-154">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

<span data-ttu-id="a5a74-155">Se non vengono specificate opzioni <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> per il middleware, le intestazioni predefinite per l'inoltro sono `None`.</span><span class="sxs-lookup"><span data-stu-id="a5a74-155">If no <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="a5a74-156">I proxy in esecuzione su indirizzi di loopback (127.0.0.0/8, [::1]), incluso l'indirizzo localhost standard (127.0.0.1), sono considerati attendibili per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="a5a74-156">Proxies running on loopback addresses (127.0.0.0/8, [::1]), including the standard localhost address (127.0.0.1), are trusted by default.</span></span> <span data-ttu-id="a5a74-157">Se le richieste tra Internet e il server Web vengono gestite anche da altri proxy o reti attendibili all'interno dell'organizzazione, aggiungerli all'elenco di <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> o <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> con <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span><span class="sxs-lookup"><span data-stu-id="a5a74-157">If other trusted proxies or networks within the organization handle requests between the Internet and the web server, add them to the list of <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> or <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span></span> <span data-ttu-id="a5a74-158">L'esempio seguente aggiunge un server proxy attendibile all'indirizzo IP 10.0.0.100 nel middleware delle intestazioni inoltrate `KnownProxies` in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a5a74-158">The following example adds a trusted proxy server at IP address 10.0.0.100 to the Forwarded Headers Middleware `KnownProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

<span data-ttu-id="a5a74-159">Per altre informazioni, vedere <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="a5a74-159">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

### <a name="install-nginx"></a><span data-ttu-id="a5a74-160">Installare Nginx</span><span class="sxs-lookup"><span data-stu-id="a5a74-160">Install Nginx</span></span>

<span data-ttu-id="a5a74-161">Usare `apt-get` per installare Nginx.</span><span class="sxs-lookup"><span data-stu-id="a5a74-161">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="a5a74-162">Il programma di installazione crea uno script di inizializzazione *systemd* che esegue Nginx come daemon all'avvio del sistema.</span><span class="sxs-lookup"><span data-stu-id="a5a74-162">The installer creates a *systemd* init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="a5a74-163">Seguire le istruzioni di installazione per Ubuntu riportate nell'articolo [Nginx: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages) (Nginx: pacchetti Debian/Ubuntu ufficiali).</span><span class="sxs-lookup"><span data-stu-id="a5a74-163">Follow the installation instructions for Ubuntu at [Nginx: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span></span>

> [!NOTE]
> <span data-ttu-id="a5a74-164">Se sono richiesti moduli Nginx facoltativi, potrebbe essere necessario compilare Nginx dall'origine.</span><span class="sxs-lookup"><span data-stu-id="a5a74-164">If optional Nginx modules are required, building Nginx from source might be required.</span></span>

<span data-ttu-id="a5a74-165">Poiché Nginx è stato installato per la prima volta, avviarlo in modo esplicito eseguendo:</span><span class="sxs-lookup"><span data-stu-id="a5a74-165">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="a5a74-166">Verificare che un browser visualizzi la pagina di destinazione predefinita per Nginx.</span><span class="sxs-lookup"><span data-stu-id="a5a74-166">Verify a browser displays the default landing page for Nginx.</span></span> <span data-ttu-id="a5a74-167">La pagina di destinazione è raggiungibile all'indirizzo `http://<server_IP_address>/index.nginx-debian.html`.</span><span class="sxs-lookup"><span data-stu-id="a5a74-167">The landing page is reachable at `http://<server_IP_address>/index.nginx-debian.html`.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="a5a74-168">Configurare Nginx</span><span class="sxs-lookup"><span data-stu-id="a5a74-168">Configure Nginx</span></span>

<span data-ttu-id="a5a74-169">Per configurare Nginx come proxy inverso per inoltrare le richieste all'app ASP.NET Core, modificare */etc/nginx/sites-available/default*.</span><span class="sxs-lookup"><span data-stu-id="a5a74-169">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="a5a74-170">Aprirlo in un editor di testo e sostituire il contenuto con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="a5a74-170">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

<span data-ttu-id="a5a74-171">Se nessun `server_name` corrisponde, Nginx usa il server predefinito.</span><span class="sxs-lookup"><span data-stu-id="a5a74-171">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="a5a74-172">Se non è definito alcun server predefinito, il primo server nel file di configurazione è il server predefinito.</span><span class="sxs-lookup"><span data-stu-id="a5a74-172">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="a5a74-173">Come procedura consigliata, aggiungere un server predefinito specifico che restituisce un codice di stato 444 nel file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="a5a74-173">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="a5a74-174">Un esempio di configurazione del server predefinito è il seguente:</span><span class="sxs-lookup"><span data-stu-id="a5a74-174">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="a5a74-175">Con il file di configurazione precedente e il server predefinito, Nginx accetta il traffico pubblico sulla porta 80 con l'intestazione host `example.com` o `*.example.com`.</span><span class="sxs-lookup"><span data-stu-id="a5a74-175">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="a5a74-176">Le richieste che non corrispondono a questi host non verranno inoltrate a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a5a74-176">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="a5a74-177">Nginx inoltra a Kestrel le richieste corrispondenti a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a5a74-177">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="a5a74-178">Per altre informazioni, vedere [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) (Elaborazione di una richiesta in nginx).</span><span class="sxs-lookup"><span data-stu-id="a5a74-178">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span> <span data-ttu-id="a5a74-179">Per cambiare porta/IP di Kestrel, vedere [Kestrel: Configurazione dell'endpoint](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="a5a74-179">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="a5a74-180">Se non si specifica una [direttiva server_name](https://nginx.org/docs/http/server_names.html) corretta, l'app può risultare esposta a vulnerabilità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="a5a74-180">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="a5a74-181">L'associazione con caratteri jolly del sottodominio (ad esempio, `*.example.com`) non costituisce un rischio per la sicurezza se viene controllato l'intero dominio padre (a differenza di `*.com`, che è vulnerabile).</span><span class="sxs-lookup"><span data-stu-id="a5a74-181">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="a5a74-182">Vedere la [sezione 5.4 di RFC7230](https://tools.ietf.org/html/rfc7230#section-5.4) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="a5a74-182">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="a5a74-183">Una volta stabilita la configurazione di Nginx, eseguire `sudo nginx -t` per verificare la sintassi dei file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="a5a74-183">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="a5a74-184">Se il test dei file di configurazione ha esito positivo, è possibile forzare Nginx ad applicare le modifiche eseguendo `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="a5a74-184">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

<span data-ttu-id="a5a74-185">Per eseguire direttamente l'app nel server:</span><span class="sxs-lookup"><span data-stu-id="a5a74-185">To directly run the app on the server:</span></span>

1. <span data-ttu-id="a5a74-186">Passare alla directory dell'app.</span><span class="sxs-lookup"><span data-stu-id="a5a74-186">Navigate to the app's directory.</span></span>
1. <span data-ttu-id="a5a74-187">Eseguire l'app: `dotnet <app_assembly.dll>`, dove `app_assembly.dll` è il nome di file di assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="a5a74-187">Run the app: `dotnet <app_assembly.dll>`, where `app_assembly.dll` is the assembly file name of the app.</span></span>

<span data-ttu-id="a5a74-188">Se l'app viene eseguita nel server ma non risponde tramite Internet, controllare il firewall del server e verificare che la porta 80 sia aperta.</span><span class="sxs-lookup"><span data-stu-id="a5a74-188">If the app runs on the server but fails to respond over the Internet, check the server's firewall and confirm that port 80 is open.</span></span> <span data-ttu-id="a5a74-189">Se si usa una macchina virtuale Ubuntu di Azure, aggiungere una regola del gruppo di sicurezza di rete (NSG) che abiliti il traffico in ingresso della porta 80.</span><span class="sxs-lookup"><span data-stu-id="a5a74-189">If using an Azure Ubuntu VM, add a Network Security Group (NSG) rule that enables inbound port 80 traffic.</span></span> <span data-ttu-id="a5a74-190">Non è necessario abilitare una regola per il traffico in uscita della porta 80, poiché il traffico in uscita viene autorizzato automaticamente quando viene abilitata la regola in ingresso.</span><span class="sxs-lookup"><span data-stu-id="a5a74-190">There's no need to enable an outbound port 80 rule, as the outbound traffic is automatically granted when the inbound rule is enabled.</span></span>

<span data-ttu-id="a5a74-191">Dopo aver eseguito il test dell'app, arrestare l'app con `Ctrl+C` al prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="a5a74-191">When done testing the app, shut the app down with `Ctrl+C` at the command prompt.</span></span>

## <a name="monitor-the-app"></a><span data-ttu-id="a5a74-192">Monitorare l'app</span><span class="sxs-lookup"><span data-stu-id="a5a74-192">Monitor the app</span></span>

<span data-ttu-id="a5a74-193">Il server è configurato in modo da inoltrare le richieste eseguite a `http://<serveraddress>:80` all'app ASP.NET Core eseguita in Kestrel all'indirizzo `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="a5a74-193">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="a5a74-194">Tuttavia, Nginx non è configurato per la gestione del processo Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a5a74-194">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="a5a74-195">È possibile usare *systemd* e creare un file del servizio per avviare e monitorare l'app Web sottostante.</span><span class="sxs-lookup"><span data-stu-id="a5a74-195">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="a5a74-196">*systemd* è un sistema di inizializzazione che offre molte funzionalità potenti per l'avvio, l'arresto e la gestione dei processi.</span><span class="sxs-lookup"><span data-stu-id="a5a74-196">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="a5a74-197">Creare il file del servizio</span><span class="sxs-lookup"><span data-stu-id="a5a74-197">Create the service file</span></span>

<span data-ttu-id="a5a74-198">Creare il file di definizione del servizio:</span><span class="sxs-lookup"><span data-stu-id="a5a74-198">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

<span data-ttu-id="a5a74-199">Di seguito è riportato un esempio di file del servizio per l'app:</span><span class="sxs-lookup"><span data-stu-id="a5a74-199">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/www/helloapp
ExecStart=/usr/bin/dotnet /var/www/helloapp/helloapp.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="a5a74-200">Se l'utente *www-data* non viene usato dalla configurazione, è necessario creare prima l'utente definito qui e assegnargli correttamente la proprietà dei file.</span><span class="sxs-lookup"><span data-stu-id="a5a74-200">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="a5a74-201">Usare `TimeoutStopSec` per configurare il tempo di attesa prima che l'app si arresti dopo aver ricevuto il segnale di interrupt iniziale.</span><span class="sxs-lookup"><span data-stu-id="a5a74-201">Use `TimeoutStopSec` to configure the duration of time to wait for the app to shut down after it receives the initial interrupt signal.</span></span> <span data-ttu-id="a5a74-202">Se l'app non si arresta in questo periodo, viene emesso il comando SIGKILL per terminare l'app.</span><span class="sxs-lookup"><span data-stu-id="a5a74-202">If the app doesn't shut down in this period, SIGKILL is issued to terminate the app.</span></span> <span data-ttu-id="a5a74-203">Specificare il valore in secondi senza unità di misura (ad esempio, `150`), un valore per l'intervallo di tempo (ad esempio, `2min 30s`) o `infinity` per disabilitare il timeout.</span><span class="sxs-lookup"><span data-stu-id="a5a74-203">Provide the value as unitless seconds (for example, `150`), a time span value (for example, `2min 30s`), or `infinity` to disable the timeout.</span></span> <span data-ttu-id="a5a74-204">Per impostazione predefinita, il valore di `TimeoutStopSec` viene impostato sul valore di `DefaultTimeoutStopSec` nel file di configurazione del sistema di gestione (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*).</span><span class="sxs-lookup"><span data-stu-id="a5a74-204">`TimeoutStopSec` defaults to the value of `DefaultTimeoutStopSec` in the manager configuration file (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*).</span></span> <span data-ttu-id="a5a74-205">Il timeout predefinito per la maggior parte delle distribuzioni è di 90 secondi.</span><span class="sxs-lookup"><span data-stu-id="a5a74-205">The default timeout for most distributions is 90 seconds.</span></span>

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

<span data-ttu-id="a5a74-206">Linux dispone di un file system con distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="a5a74-206">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="a5a74-207">L'impostazione di ASPNETCORE_ENVIRONMENT su "Production" determina la ricerca del file di configurazione *appsettings.Production.json*, non di *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="a5a74-207">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

<span data-ttu-id="a5a74-208">Per alcuni valori (ad esempio, le stringhe di connessione SQL) è necessario usare caratteri di escape, in modo da consentire ai provider di configurazione di leggere le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="a5a74-208">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="a5a74-209">Usare il comando seguente per generare un valore con caratteri di escape corretti per l'uso nel file di configurazione:</span><span class="sxs-lookup"><span data-stu-id="a5a74-209">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>

```console
systemd-escape "<value-to-escape>"
```

<span data-ttu-id="a5a74-210">I due punti (`:`) separatori non sono supportati nei nomi delle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="a5a74-210">Colon (`:`) separators aren't supported in environment variable names.</span></span> <span data-ttu-id="a5a74-211">Usare un doppio carattere di sottolineatura (`__`) al posto dei due punti.</span><span class="sxs-lookup"><span data-stu-id="a5a74-211">Use a double underscore (`__`) in place of a colon.</span></span> <span data-ttu-id="a5a74-212">Il [provider di configurazione delle variabili di ambiente](xref:fundamentals/configuration/index#environment-variables-configuration-provider) converte le doppie sottolineature in due punti quando le variabili di ambiente vengono lette nella configurazione.</span><span class="sxs-lookup"><span data-stu-id="a5a74-212">The [Environment Variables configuration provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider) converts double-underscores into colons when environment variables are read into configuration.</span></span> <span data-ttu-id="a5a74-213">Nell'esempio seguente la chiave della stringa di connessione `ConnectionStrings:DefaultConnection` è impostata nel file di definizione del servizio come `ConnectionStrings__DefaultConnection`:</span><span class="sxs-lookup"><span data-stu-id="a5a74-213">In the following example, the connection string key `ConnectionStrings:DefaultConnection` is set into the service definition file as `ConnectionStrings__DefaultConnection`:</span></span>

```
Environment=ConnectionStrings__DefaultConnection={Connection String}
```

<span data-ttu-id="a5a74-214">Salvare il file e abilitare il servizio.</span><span class="sxs-lookup"><span data-stu-id="a5a74-214">Save the file and enable the service.</span></span>

```bash
sudo systemctl enable kestrel-helloapp.service
```

<span data-ttu-id="a5a74-215">Avviare il servizio e verificare che sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="a5a74-215">Start the service and verify that it's running.</span></span>

```
sudo systemctl start kestrel-helloapp.service
sudo systemctl status kestrel-helloapp.service

● kestrel-helloapp.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-helloapp.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-helloapp.service
            └─9021 /usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
```

<span data-ttu-id="a5a74-216">Con il proxy inverso configurato e Kestrel gestito tramite systemd, l'app Web è completamente configurata ed è possibile accedervi da un browser nel computer locale all'indirizzo `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="a5a74-216">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="a5a74-217">È anche possibile accedervi da un computer remoto, escludendo eventuali firewall che potrebbero impedirlo.</span><span class="sxs-lookup"><span data-stu-id="a5a74-217">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="a5a74-218">Se si esaminano le intestazioni di risposta, l'intestazione `Server` indica che l'app ASP.NET Core è gestita da Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a5a74-218">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="view-logs"></a><span data-ttu-id="a5a74-219">Visualizzare i log</span><span class="sxs-lookup"><span data-stu-id="a5a74-219">View logs</span></span>

<span data-ttu-id="a5a74-220">Poiché l'app Web che usa Kestrel viene gestita con `systemd`, tutti gli eventi e i processi vengono registrati in un giornale di registrazione centralizzato.</span><span class="sxs-lookup"><span data-stu-id="a5a74-220">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="a5a74-221">Tuttavia, questo giornale include tutte le voci per tutti i servizi e i processi gestiti da `systemd`.</span><span class="sxs-lookup"><span data-stu-id="a5a74-221">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="a5a74-222">Per visualizzare le voci specifiche di `kestrel-helloapp.service`, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="a5a74-222">To view the `kestrel-helloapp.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service
```

<span data-ttu-id="a5a74-223">Per filtrare ulteriormente, opzioni come `--since today`, `--until 1 hour ago` o una combinazione delle stesse consentono di ridurre la quantità di voci restituite.</span><span class="sxs-lookup"><span data-stu-id="a5a74-223">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="a5a74-224">Protezione dei dati</span><span class="sxs-lookup"><span data-stu-id="a5a74-224">Data protection</span></span>

<span data-ttu-id="a5a74-225">Lo [stack di protezione dei dati di ASP.NET Core](xref:security/data-protection/introduction) è usato da diversi [middleware](xref:fundamentals/middleware/index) di ASP.NET Core, tra cui i middleware di autenticazione (ad esempio il middleware per i cookie) e le protezioni CSRF (Cross-Site Request Forgery).</span><span class="sxs-lookup"><span data-stu-id="a5a74-225">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="a5a74-226">Anche se le DPAPI (Data Protection API) non vengono chiamate dal codice dell'utente, è consigliabile configurare la protezione dati per la creazione di un [archivio di chiavi](xref:security/data-protection/implementation/key-management) crittografiche permanente.</span><span class="sxs-lookup"><span data-stu-id="a5a74-226">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="a5a74-227">Se non si configura la protezione dei dati, le chiavi vengono mantenute in memoria ed eliminate al riavvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="a5a74-227">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="a5a74-228">Se il gruppo di chiavi viene archiviato in memoria quando l'app viene riavviata:</span><span class="sxs-lookup"><span data-stu-id="a5a74-228">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="a5a74-229">Tutti i token di autenticazione basati su cookie vengono invalidati.</span><span class="sxs-lookup"><span data-stu-id="a5a74-229">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="a5a74-230">Gli utenti devono ripetere l'accesso alla richiesta successiva.</span><span class="sxs-lookup"><span data-stu-id="a5a74-230">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="a5a74-231">Tutti i dati protetti con il gruppo di chiavi non possono più essere decrittografati.</span><span class="sxs-lookup"><span data-stu-id="a5a74-231">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="a5a74-232">Possono essere inclusi i [token CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) e i [cookie TempData di ASP.NET Core MVC](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="a5a74-232">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="a5a74-233">Per configurare la protezione dei dati in modo da rendere persistente il gruppo di chiavi e crittografarlo, vedere:</span><span class="sxs-lookup"><span data-stu-id="a5a74-233">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="long-request-header-fields"></a><span data-ttu-id="a5a74-234">Campi di intestazione della richiesta di grandi dimensioni</span><span class="sxs-lookup"><span data-stu-id="a5a74-234">Long request header fields</span></span>

<span data-ttu-id="a5a74-235">Le impostazioni predefinite del server proxy limitano in genere i campi di intestazione della richiesta a 4 K o 8 K a seconda della piattaforma.</span><span class="sxs-lookup"><span data-stu-id="a5a74-235">Proxy server default settings typically limit request header fields to 4 K or 8 K depending on the platform.</span></span> <span data-ttu-id="a5a74-236">Un'app può richiedere campi più lunghi del valore predefinito, ad esempio le app che usano [Azure Active Directory](https://azure.microsoft.com/services/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="a5a74-236">An app may require fields longer than the default (for example, apps that use [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)).</span></span> <span data-ttu-id="a5a74-237">Se sono necessari campi più lunghi, le impostazioni predefinite del server proxy richiedono la regolazione.</span><span class="sxs-lookup"><span data-stu-id="a5a74-237">If longer fields are required, the proxy server's default settings require adjustment.</span></span> <span data-ttu-id="a5a74-238">I valori da applicare dipendono dallo scenario.</span><span class="sxs-lookup"><span data-stu-id="a5a74-238">The values to apply depend on the scenario.</span></span> <span data-ttu-id="a5a74-239">Per altre informazioni, vedere la documentazione del server.</span><span class="sxs-lookup"><span data-stu-id="a5a74-239">For more information, see your server's documentation.</span></span>

* [<span data-ttu-id="a5a74-240">proxy_buffer_size</span><span class="sxs-lookup"><span data-stu-id="a5a74-240">proxy_buffer_size</span></span>](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_buffer_size)
* [<span data-ttu-id="a5a74-241">proxy_buffers</span><span class="sxs-lookup"><span data-stu-id="a5a74-241">proxy_buffers</span></span>](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_buffers)
* [<span data-ttu-id="a5a74-242">proxy_busy_buffers_size</span><span class="sxs-lookup"><span data-stu-id="a5a74-242">proxy_busy_buffers_size</span></span>](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_busy_buffers_size)
* [<span data-ttu-id="a5a74-243">large_client_header_buffers</span><span class="sxs-lookup"><span data-stu-id="a5a74-243">large_client_header_buffers</span></span>](https://nginx.org/docs/http/ngx_http_core_module.html#large_client_header_buffers)

> [!WARNING]
> <span data-ttu-id="a5a74-244">Aumentare i valori predefiniti dei buffer proxy solo se necessario.</span><span class="sxs-lookup"><span data-stu-id="a5a74-244">Don't increase the default values of proxy buffers unless necessary.</span></span> <span data-ttu-id="a5a74-245">Se si aumentano questi valori, si aumenta il rischio di sovraccarico del buffer (overflow) e di attacchi Denial of Service (DoS) da parte di utenti malintenzionati.</span><span class="sxs-lookup"><span data-stu-id="a5a74-245">Increasing these values increases the risk of buffer overrun (overflow) and Denial of Service (DoS) attacks by malicious users.</span></span>

## <a name="secure-the-app"></a><span data-ttu-id="a5a74-246">Proteggere l'app</span><span class="sxs-lookup"><span data-stu-id="a5a74-246">Secure the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="a5a74-247">Abilitare AppArmor</span><span class="sxs-lookup"><span data-stu-id="a5a74-247">Enable AppArmor</span></span>

<span data-ttu-id="a5a74-248">Linux Security Modules (LSM) è un framework che fa parte del kernel Linux a partire da Linux 2.6.</span><span class="sxs-lookup"><span data-stu-id="a5a74-248">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="a5a74-249">LSM supporta diverse implementazioni di moduli di protezione.</span><span class="sxs-lookup"><span data-stu-id="a5a74-249">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="a5a74-250">[AppArmor](https://wiki.ubuntu.com/AppArmor) è un modulo LSM che implementa un sistema di controllo di accesso obbligatorio che consente di circoscrivere il programma a un set limitato di risorse.</span><span class="sxs-lookup"><span data-stu-id="a5a74-250">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="a5a74-251">Verificare che AppArmor sia abilitato e configurato correttamente.</span><span class="sxs-lookup"><span data-stu-id="a5a74-251">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configure-the-firewall"></a><span data-ttu-id="a5a74-252">Configurare il firewall</span><span class="sxs-lookup"><span data-stu-id="a5a74-252">Configure the firewall</span></span>

<span data-ttu-id="a5a74-253">Chiudere tutte le porte esterne che non sono in uso.</span><span class="sxs-lookup"><span data-stu-id="a5a74-253">Close off all external ports that are not in use.</span></span> <span data-ttu-id="a5a74-254">Il firewall UFW (Uncomplicated Firewall) offre un front-end per `iptables` attraverso un'interfaccia della riga di comando per la configurazione del firewall.</span><span class="sxs-lookup"><span data-stu-id="a5a74-254">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span>

> [!WARNING]
> <span data-ttu-id="a5a74-255">Se non è configurato correttamente, un firewall impedisce l'accesso all'intero sistema.</span><span class="sxs-lookup"><span data-stu-id="a5a74-255">A firewall will prevent access to the whole system if not configured correctly.</span></span> <span data-ttu-id="a5a74-256">Se non si specifica la porta SSH corretta, non sarà possibile accedere al sistema se si usa SSH per la connessione.</span><span class="sxs-lookup"><span data-stu-id="a5a74-256">Failure to specify the correct SSH port will effectively lock you out of the system if you are using SSH to connect to it.</span></span> <span data-ttu-id="a5a74-257">Il numero di porta predefinito è 22.</span><span class="sxs-lookup"><span data-stu-id="a5a74-257">The default port is 22.</span></span> <span data-ttu-id="a5a74-258">Per altre informazioni, vedere l'[introduzione a ufw](https://help.ubuntu.com/community/UFW) e il [manuale](https://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).</span><span class="sxs-lookup"><span data-stu-id="a5a74-258">For more information, see the [introduction to ufw](https://help.ubuntu.com/community/UFW) and the [manual](https://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).</span></span>

<span data-ttu-id="a5a74-259">Installare `ufw` e configurarlo per consentire il traffico su tutte le porte necessarie.</span><span class="sxs-lookup"><span data-stu-id="a5a74-259">Install `ufw` and configure it to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

sudo ufw enable
```

### <a name="secure-nginx"></a><span data-ttu-id="a5a74-260">Proteggere Nginx</span><span class="sxs-lookup"><span data-stu-id="a5a74-260">Secure Nginx</span></span>

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="a5a74-261">Modificare il nome di risposta Nginx</span><span class="sxs-lookup"><span data-stu-id="a5a74-261">Change the Nginx response name</span></span>

<span data-ttu-id="a5a74-262">Modificare *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="a5a74-262">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a><span data-ttu-id="a5a74-263">Configurare le opzioni</span><span class="sxs-lookup"><span data-stu-id="a5a74-263">Configure options</span></span>

<span data-ttu-id="a5a74-264">Configurare il server con moduli aggiuntivi obbligatori.</span><span class="sxs-lookup"><span data-stu-id="a5a74-264">Configure the server with additional required modules.</span></span> <span data-ttu-id="a5a74-265">Può essere utile usare un firewall per app Web, ad esempio [ModSecurity](https://www.modsecurity.org/), per la protezione avanzata dell'app.</span><span class="sxs-lookup"><span data-stu-id="a5a74-265">Consider using a web app firewall, such as [ModSecurity](https://www.modsecurity.org/), to harden the app.</span></span>

#### <a name="https-configuration"></a><span data-ttu-id="a5a74-266">Configurazione HTTPS</span><span class="sxs-lookup"><span data-stu-id="a5a74-266">HTTPS configuration</span></span>

<span data-ttu-id="a5a74-267">**Configurare l'app per connessioni locali sicure (HTTPS)**</span><span class="sxs-lookup"><span data-stu-id="a5a74-267">**Configure the app for secure (HTTPS) local connections**</span></span>

<span data-ttu-id="a5a74-268">Il comando [dotnet run](/dotnet/core/tools/dotnet-run) usa il file *Properties/launchSettings.json* dell'app, che configura l'app per l'ascolto negli URL specificati dalla proprietà `applicationUrl` , ad esempio `https://localhost:5001; http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a5a74-268">The [dotnet run](/dotnet/core/tools/dotnet-run) command uses the app's *Properties/launchSettings.json* file, which configures the app to listen on the URLs provided by the `applicationUrl` property (for example, `https://localhost:5001;http://localhost:5000`).</span></span>

<span data-ttu-id="a5a74-269">Configurare l'app per l'uso di un certificato nello sviluppo per il comando `dotnet run` o l'ambiente di sviluppo (F5 o CTRL+F5 in Visual Studio Code) usando uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="a5a74-269">Configure the app to use a certificate in development for the `dotnet run` command or development environment (F5 or Ctrl+F5 in Visual Studio Code) using one of the following approaches:</span></span>

* <span data-ttu-id="a5a74-270">[Sostituire il certificato predefinito della configurazione](xref:fundamentals/servers/kestrel#configuration) (*consigliato*)</span><span class="sxs-lookup"><span data-stu-id="a5a74-270">[Replace the default certificate from configuration](xref:fundamentals/servers/kestrel#configuration) (*Recommended*)</span></span>
* [<span data-ttu-id="a5a74-271">KestrelServerOptions.ConfigureHttpsDefaults</span><span class="sxs-lookup"><span data-stu-id="a5a74-271">KestrelServerOptions.ConfigureHttpsDefaults</span></span>](xref:fundamentals/servers/kestrel#configurehttpsdefaultsactionhttpsconnectionadapteroptions)

<span data-ttu-id="a5a74-272">**Configurare il proxy inverso per connessioni client sicure (HTTPS)**</span><span class="sxs-lookup"><span data-stu-id="a5a74-272">**Configure the reverse proxy for secure (HTTPS) client connections**</span></span>

* <span data-ttu-id="a5a74-273">Configurare il server in modo che sia in ascolto del traffico HTTPS sulla porta `443` specificando un certificato valido emesso da un'autorità di certificazione attendibile.</span><span class="sxs-lookup"><span data-stu-id="a5a74-273">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="a5a74-274">Rafforzare la protezione adottando alcune delle procedure descritte nel file */etc/nginx/nginx.conf* che segue.</span><span class="sxs-lookup"><span data-stu-id="a5a74-274">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="a5a74-275">Gli esempi includono la scelta di una crittografia più complessa e il reindirizzamento di tutto il traffico su HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a5a74-275">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="a5a74-276">L'aggiunta di un'intestazione `HTTP Strict-Transport-Security` (HSTS) garantisce che tutte le richieste successive vengano effettuate dal client via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a5a74-276">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS.</span></span>

* <span data-ttu-id="a5a74-277">Non aggiungere l'intestazione HSTS o scegliere un elemento `max-age` appropriato se si prevede di disabilitare HTTPS in futuro.</span><span class="sxs-lookup"><span data-stu-id="a5a74-277">Don't add the HSTS header or chose an appropriate `max-age` if HTTPS will be disabled in the future.</span></span>

<span data-ttu-id="a5a74-278">Aggiungere il file di configurazione */etc/nginx/proxy.conf*:</span><span class="sxs-lookup"><span data-stu-id="a5a74-278">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="a5a74-279">Aggiungere il file di configurazione */etc/nginx/proxy.conf*.</span><span class="sxs-lookup"><span data-stu-id="a5a74-279">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="a5a74-280">L'esempio contiene entrambe le sezioni `http` e `server` in un unico file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="a5a74-280">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="a5a74-281">Proteggere Nginx dal clickjacking</span><span class="sxs-lookup"><span data-stu-id="a5a74-281">Secure Nginx from clickjacking</span></span>

<span data-ttu-id="a5a74-282">Il [clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), anche noto come *attacco UI Redress*, è un attacco dannoso in cui un visitatore di un sito Web viene indotto a fare clic su un collegamento o un pulsante in una pagina diversa da quella che sta attualmente visualizzando.</span><span class="sxs-lookup"><span data-stu-id="a5a74-282">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="a5a74-283">Usare `X-FRAME-OPTIONS` per proteggere il sito.</span><span class="sxs-lookup"><span data-stu-id="a5a74-283">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="a5a74-284">Per contrastare gli attacchi di clickjacking:</span><span class="sxs-lookup"><span data-stu-id="a5a74-284">To mitigate clickjacking attacks:</span></span>

1. <span data-ttu-id="a5a74-285">Modificare il file *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="a5a74-285">Edit the *nginx.conf* file:</span></span>

   ```bash
   sudo nano /etc/nginx/nginx.conf
   ```

   <span data-ttu-id="a5a74-286">Aggiungere la riga `add_header X-Frame-Options "SAMEORIGIN";`.</span><span class="sxs-lookup"><span data-stu-id="a5a74-286">Add the line `add_header X-Frame-Options "SAMEORIGIN";`.</span></span>
1. <span data-ttu-id="a5a74-287">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="a5a74-287">Save the file.</span></span>
1. <span data-ttu-id="a5a74-288">Riavviare Nginx.</span><span class="sxs-lookup"><span data-stu-id="a5a74-288">Restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="a5a74-289">Analisi del tipo MIME</span><span class="sxs-lookup"><span data-stu-id="a5a74-289">MIME-type sniffing</span></span>

<span data-ttu-id="a5a74-290">Questa intestazione impedisce alla maggior parte dei browser di dedurre dall'analisi del tipo MIME un tipo di contenuto diverso da quello dichiarato, poiché l'intestazione indica al browser di non eseguire l'override del tipo di contenuto della risposta.</span><span class="sxs-lookup"><span data-stu-id="a5a74-290">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="a5a74-291">Con l'opzione `nosniff`, se il server indica che il contenuto è "text/html", il browser ne esegue il rendering come "text/html".</span><span class="sxs-lookup"><span data-stu-id="a5a74-291">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="a5a74-292">Modificare il file *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="a5a74-292">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="a5a74-293">Aggiungere la riga `add_header X-Content-Type-Options "nosniff";` e salvare il file, quindi riavviare Nginx.</span><span class="sxs-lookup"><span data-stu-id="a5a74-293">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a5a74-294">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="a5a74-294">Additional resources</span></span>

* [<span data-ttu-id="a5a74-295">Prerequisiti per .NET Core in Linux</span><span class="sxs-lookup"><span data-stu-id="a5a74-295">Prerequisites for .NET Core on Linux</span></span>](/dotnet/core/linux-prerequisites)
* <span data-ttu-id="a5a74-296">[Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages) (Nginx: Versioni binarie: Pacchetti ufficiali Debian/Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="a5a74-296">[Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)</span></span>
* <xref:test/troubleshoot>
* <xref:host-and-deploy/proxy-load-balancer>
* <span data-ttu-id="a5a74-297">[NGINX: Using the Forwarded header](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/) (NGINX: Uso dell'intestazione Forwarded)</span><span class="sxs-lookup"><span data-stu-id="a5a74-297">[NGINX: Using the Forwarded header](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)</span></span>
