---
title: Hosting di ASP.NET Core in Linux con Nginx
author: rick-anderson
description: Informazioni su come configurare Nginx come proxy inverso in Ubuntu 16.04 per inoltrare il traffico HTTP a un'app Web ASP.NET Core in esecuzione su Kestrel.
ms.author: riande
ms.custom: mvc
ms.date: 05/22/2018
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 840a9f98b3409f74b9a41ee24ff7bcb33a875470
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433935"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="c23e7-103">Hosting di ASP.NET Core in Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="c23e7-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="c23e7-104">Di [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="c23e7-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="c23e7-105">Questa guida descrive come configurare un ambiente ASP.NET Core pronto per la produzione in un server Ubuntu 16.04.</span><span class="sxs-lookup"><span data-stu-id="c23e7-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="c23e7-106">Queste istruzioni si applicano probabilmente anche alle versioni più recenti di Ubuntu, sebbene non siano state testate con le versioni più recenti.</span><span class="sxs-lookup"><span data-stu-id="c23e7-106">These instructions likely work with newer versions of Ubuntu, but the instructions haven't been tested with newer versions.</span></span>

> [!NOTE]
> <span data-ttu-id="c23e7-107">Per Ubuntu 14.04, è consigliabile *supervisord* come soluzione per il monitoraggio del processo Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c23e7-107">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="c23e7-108">*systemd* non è disponibile in Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="c23e7-108">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="c23e7-109">Per le istruzioni per Ubuntu 14.04, vedere la [versione precedente di questo argomento](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="c23e7-109">For Ubuntu 14.04 instructions, see the [previous version of this topic](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="c23e7-110">In questa guida:</span><span class="sxs-lookup"><span data-stu-id="c23e7-110">This guide:</span></span>

* <span data-ttu-id="c23e7-111">Posizionare un'app ASP.NET Core esistente dietro un server proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="c23e7-111">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="c23e7-112">Configurare il server proxy inverso per l'inoltro delle richieste al server Web Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c23e7-112">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="c23e7-113">Verificare che l'app Web venga eseguita all'avvio come daemon.</span><span class="sxs-lookup"><span data-stu-id="c23e7-113">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="c23e7-114">Configurare uno strumento di gestione del processo per consentire il riavvio dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="c23e7-114">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c23e7-115">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="c23e7-115">Prerequisites</span></span>

1. <span data-ttu-id="c23e7-116">Accedere a un server Ubuntu 16.04 con un account utente standard con privilegio sudo.</span><span class="sxs-lookup"><span data-stu-id="c23e7-116">Access to an Ubuntu 16.04 server with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="c23e7-117">Installare il runtime .NET Core nel server.</span><span class="sxs-lookup"><span data-stu-id="c23e7-117">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="c23e7-118">Visitare la [pagina di tutti i download per .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="c23e7-118">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="c23e7-119">Selezionare il runtime non di anteprima più recente nell'elenco **Runtime**.</span><span class="sxs-lookup"><span data-stu-id="c23e7-119">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="c23e7-120">Selezionare e seguire le istruzioni per Ubuntu relative alla versione di Ubuntu del server.</span><span class="sxs-lookup"><span data-stu-id="c23e7-120">Select and follow the instructions for Ubuntu that match the Ubuntu version of the server.</span></span>
1. <span data-ttu-id="c23e7-121">Un'app ASP.NET Core esistente.</span><span class="sxs-lookup"><span data-stu-id="c23e7-121">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="c23e7-122">Pubblicare e copiare l'app</span><span class="sxs-lookup"><span data-stu-id="c23e7-122">Publish and copy over the app</span></span>

<span data-ttu-id="c23e7-123">Configurare l'app per una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="c23e7-123">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="c23e7-124">Eseguire [dotnet publish](/dotnet/core/tools/dotnet-publish) dall'ambiente di sviluppo per creare il pacchetto di un'app in una directory (ad esempio, *bin/Release/&lt;target_framework_moniker&gt;/publish*) che può essere eseguita nel server:</span><span class="sxs-lookup"><span data-stu-id="c23e7-124">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="c23e7-125">È anche possibile pubblicare l'app come [distribuzione completa](/dotnet/core/deploying/#self-contained-deployments-scd) se si preferisce non mantenere il runtime .NET Core nel server.</span><span class="sxs-lookup"><span data-stu-id="c23e7-125">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="c23e7-126">Copiare l'app ASP.NET Core sul server usando uno strumento integrato nel flusso di lavoro dell'organizzazione, ad esempio SCP o FTP.</span><span class="sxs-lookup"><span data-stu-id="c23e7-126">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="c23e7-127">Le app Web si trovano in genere nella directory *var*, ad esempio *var/aspnetcore/hellomvc*.</span><span class="sxs-lookup"><span data-stu-id="c23e7-127">It's common to locate web apps under the *var* directory (for example, *var/aspnetcore/hellomvc*).</span></span>

> [!NOTE]
> <span data-ttu-id="c23e7-128">In uno scenario di distribuzione di produzione un flusso di lavoro di integrazione continua esegue le operazioni di pubblicazione dell'app e di copia degli asset nel server.</span><span class="sxs-lookup"><span data-stu-id="c23e7-128">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

<span data-ttu-id="c23e7-129">Eseguire il test dell'app:</span><span class="sxs-lookup"><span data-stu-id="c23e7-129">Test the app:</span></span>

1. <span data-ttu-id="c23e7-130">Dalla riga di comando, eseguire l'app: `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="c23e7-130">From the command line, run the app: `dotnet <app_assembly>.dll`.</span></span>
1. <span data-ttu-id="c23e7-131">In un browser passare a `http://<serveraddress>:<port>` per verificare se l'app funziona in Linux in locale.</span><span class="sxs-lookup"><span data-stu-id="c23e7-131">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux locally.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="c23e7-132">Configurare un server proxy inverso</span><span class="sxs-lookup"><span data-stu-id="c23e7-132">Configure a reverse proxy server</span></span>

<span data-ttu-id="c23e7-133">Un proxy inverso è una configurazione comune per la gestione delle app Web dinamiche.</span><span class="sxs-lookup"><span data-stu-id="c23e7-133">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="c23e7-134">Un proxy inverso termina la richiesta HTTP e la inoltra all'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c23e7-134">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="c23e7-135">Entrambe le configurazioni (con o senza un server proxy inverso) sono configurazioni di hosting valide e supportate per le app ASP.NET Core 2.0 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="c23e7-135">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="c23e7-136">Per altre informazioni, vedere [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Quando usare Kestrel con un proxy inverso).</span><span class="sxs-lookup"><span data-stu-id="c23e7-136">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

### <a name="use-a-reverse-proxy-server"></a><span data-ttu-id="c23e7-137">Usare un server proxy inverso</span><span class="sxs-lookup"><span data-stu-id="c23e7-137">Use a reverse proxy server</span></span>

<span data-ttu-id="c23e7-138">Kestrel funziona perfettamente per la gestione del contenuto dinamico da ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c23e7-138">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="c23e7-139">Tuttavia, le funzionalità di gestione Web sono meno avanzate rispetto a quelle offerte da server come IIS, Apache o Nginx.</span><span class="sxs-lookup"><span data-stu-id="c23e7-139">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="c23e7-140">Un server proxy inverso è in grado di eseguire l'offload di attività come la gestione del contenuto statico, la memorizzazione nella cache delle richieste, la compressione delle richieste e la terminazione SSL dal server HTTP.</span><span class="sxs-lookup"><span data-stu-id="c23e7-140">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="c23e7-141">Un server proxy inverso può risiedere in un computer dedicato o essere distribuito insieme a un server HTTP.</span><span class="sxs-lookup"><span data-stu-id="c23e7-141">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="c23e7-142">Ai fini di questa guida viene usata una singola istanza di Nginx.</span><span class="sxs-lookup"><span data-stu-id="c23e7-142">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="c23e7-143">Viene eseguito sullo stesso server, insieme al server HTTP.</span><span class="sxs-lookup"><span data-stu-id="c23e7-143">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="c23e7-144">In base ai requisiti, è possibile scegliere una configurazione diversa.</span><span class="sxs-lookup"><span data-stu-id="c23e7-144">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="c23e7-145">Poiché le richieste vengono inoltrate dal proxy inverso, usare il [middleware delle intestazioni inoltrate](xref:host-and-deploy/proxy-load-balancer) dal pacchetto [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/).</span><span class="sxs-lookup"><span data-stu-id="c23e7-145">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="c23e7-146">Il middleware aggiorna `Request.Scheme` usando l'intestazione `X-Forwarded-Proto`, in modo che gli URI di reindirizzamento e altri criteri di sicurezza funzionino correttamente.</span><span class="sxs-lookup"><span data-stu-id="c23e7-146">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="c23e7-147">Qualsiasi componente che dipende dallo schema, ad esempio l'autenticazione, la generazione di collegamenti, i reindirizzamenti e la georilevazione, deve essere inserito dopo aver richiamato il middleware delle intestazioni inoltrate.</span><span class="sxs-lookup"><span data-stu-id="c23e7-147">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="c23e7-148">Come regola generale,il middleware delle intestazioni inoltrate deve essere eseguito prima degli altri middleware, ad eccezione del middleware di diagnostica e gestione degli errori.</span><span class="sxs-lookup"><span data-stu-id="c23e7-148">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="c23e7-149">Questo ordine garantisce che il middleware basato sulle intestazioni inoltrate possa usare i valori di intestazione per l'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="c23e7-149">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c23e7-150">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c23e7-150">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c23e7-151">Richiamare il metodo [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) in `Startup.Configure` prima di chiamare [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) o un middleware con schema di autenticazione simile.</span><span class="sxs-lookup"><span data-stu-id="c23e7-151">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="c23e7-152">Configurare il middleware per l'inoltro delle intestazioni `X-Forwarded-For` e `X-Forwarded-Proto`:</span><span class="sxs-lookup"><span data-stu-id="c23e7-152">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c23e7-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c23e7-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c23e7-154">Richiamare il metodo [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) in `Startup.Configure` prima di chiamare [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) e [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) o un middleware con schema di autenticazione simile.</span><span class="sxs-lookup"><span data-stu-id="c23e7-154">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="c23e7-155">Configurare il middleware per l'inoltro delle intestazioni `X-Forwarded-For` e `X-Forwarded-Proto`:</span><span class="sxs-lookup"><span data-stu-id="c23e7-155">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="c23e7-156">Se non sono specificate opzioni [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) per il middleware, le intestazioni predefinite per l'inoltro sono `None`.</span><span class="sxs-lookup"><span data-stu-id="c23e7-156">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="c23e7-157">Potrebbero essere necessari interventi di configurazione aggiuntivi per le app ospitate dietro a server proxy e a servizi di bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="c23e7-157">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="c23e7-158">Per altre informazioni, vedere [Configurare ASP.NET Core per l'utilizzo di server proxy e servizi di bilanciamento del carico](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="c23e7-158">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-nginx"></a><span data-ttu-id="c23e7-159">Installare Nginx</span><span class="sxs-lookup"><span data-stu-id="c23e7-159">Install Nginx</span></span>

<span data-ttu-id="c23e7-160">Usare `apt-get` per installare Nginx.</span><span class="sxs-lookup"><span data-stu-id="c23e7-160">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="c23e7-161">Il programma di installazione crea uno script di inizializzazione *systemd* che esegue Nginx come daemon all'avvio del sistema.</span><span class="sxs-lookup"><span data-stu-id="c23e7-161">The installer creates a *systemd* init script that runs Nginx as daemon on system startup.</span></span> 

```bash
sudo -s
nginx=stable # use nginx=development for latest development version
add-apt-repository ppa:nginx/$nginx
apt-get update
apt-get install nginx
```

<span data-ttu-id="c23e7-162">Il PPA (Personal Package Archive) di Ubuntu è gestito da volontari e non è distribuito da [nginx.org](https://nginx.org/). Per altre informazioni, vedere [Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages) (Nginx: Versioni binarie: Pacchetti ufficiali Debian/Ubuntu).</span><span class="sxs-lookup"><span data-stu-id="c23e7-162">The Ubuntu Personal Package Archive (PPA) is maintained by volunteers and isn't distributed by [nginx.org](https://nginx.org/). For more information, see [Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span></span>

> [!NOTE]
> <span data-ttu-id="c23e7-163">Se sono richiesti moduli Nginx facoltativi, potrebbe essere necessario compilare Nginx dall'origine.</span><span class="sxs-lookup"><span data-stu-id="c23e7-163">If optional Nginx modules are required, building Nginx from source might be required.</span></span>

<span data-ttu-id="c23e7-164">Poiché Nginx è stato installato per la prima volta, avviarlo in modo esplicito eseguendo:</span><span class="sxs-lookup"><span data-stu-id="c23e7-164">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="c23e7-165">Verificare che un browser visualizzi la pagina di destinazione predefinita per Nginx.</span><span class="sxs-lookup"><span data-stu-id="c23e7-165">Verify a browser displays the default landing page for Nginx.</span></span> <span data-ttu-id="c23e7-166">La pagina di destinazione è raggiungibile all'indirizzo `http://<server_IP_address>/index.nginx-debian.html`.</span><span class="sxs-lookup"><span data-stu-id="c23e7-166">The landing page is reachable at `http://<server_IP_address>/index.nginx-debian.html`.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="c23e7-167">Configurare Nginx</span><span class="sxs-lookup"><span data-stu-id="c23e7-167">Configure Nginx</span></span>

<span data-ttu-id="c23e7-168">Per configurare Nginx come proxy inverso per inoltrare le richieste all'app ASP.NET Core, modificare */etc/nginx/sites-available/default*.</span><span class="sxs-lookup"><span data-stu-id="c23e7-168">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="c23e7-169">Aprirlo in un editor di testo e sostituire il contenuto con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="c23e7-169">Open it in a text editor, and replace the contents with the following:</span></span>

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

<span data-ttu-id="c23e7-170">Se nessun `server_name` corrisponde, Nginx usa il server predefinito.</span><span class="sxs-lookup"><span data-stu-id="c23e7-170">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="c23e7-171">Se non è definito alcun server predefinito, il primo server nel file di configurazione è il server predefinito.</span><span class="sxs-lookup"><span data-stu-id="c23e7-171">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="c23e7-172">Come procedura consigliata, aggiungere un server predefinito specifico che restituisce un codice di stato 444 nel file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="c23e7-172">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="c23e7-173">Un esempio di configurazione del server predefinito è il seguente:</span><span class="sxs-lookup"><span data-stu-id="c23e7-173">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="c23e7-174">Con il file di configurazione precedente e il server predefinito, Nginx accetta il traffico pubblico sulla porta 80 con l'intestazione host `example.com` o `*.example.com`.</span><span class="sxs-lookup"><span data-stu-id="c23e7-174">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="c23e7-175">Le richieste che non corrispondono a questi host non verranno inoltrate a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c23e7-175">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="c23e7-176">Nginx inoltra a Kestrel le richieste corrispondenti a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c23e7-176">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="c23e7-177">Per altre informazioni, vedere [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) (Elaborazione di una richiesta in nginx).</span><span class="sxs-lookup"><span data-stu-id="c23e7-177">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span> <span data-ttu-id="c23e7-178">Per cambiare porta/IP di Kestrel, vedere [Kestrel: Configurazione dell'endpoint](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="c23e7-178">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="c23e7-179">Se non si specifica una [direttiva server_name](https://nginx.org/docs/http/server_names.html) corretta, l'app può risultare esposta a vulnerabilità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="c23e7-179">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="c23e7-180">L'associazione con caratteri jolly del sottodominio (ad esempio, `*.example.com`) non costituisce un rischio per la sicurezza se viene controllato l'intero dominio padre (a differenza di `*.com`, che è vulnerabile).</span><span class="sxs-lookup"><span data-stu-id="c23e7-180">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="c23e7-181">Vedere la [sezione 5.4 di RFC7230](https://tools.ietf.org/html/rfc7230#section-5.4) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="c23e7-181">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="c23e7-182">Una volta stabilita la configurazione di Nginx, eseguire `sudo nginx -t` per verificare la sintassi dei file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="c23e7-182">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="c23e7-183">Se il test dei file di configurazione ha esito positivo, è possibile forzare Nginx ad applicare le modifiche eseguendo `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="c23e7-183">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

<span data-ttu-id="c23e7-184">Per eseguire direttamente l'app nel server:</span><span class="sxs-lookup"><span data-stu-id="c23e7-184">To directly run the app on the server:</span></span>

1. <span data-ttu-id="c23e7-185">Passare alla directory dell'app.</span><span class="sxs-lookup"><span data-stu-id="c23e7-185">Navigate to the app's directory.</span></span>
1. <span data-ttu-id="c23e7-186">Eseguire l'eseguibile dell'app: `./<app_executable>`.</span><span class="sxs-lookup"><span data-stu-id="c23e7-186">Run the app's executable: `./<app_executable>`.</span></span>

<span data-ttu-id="c23e7-187">Se si verifica un errore relativo alle autorizzazioni, modificare le autorizzazioni:</span><span class="sxs-lookup"><span data-stu-id="c23e7-187">If a permissions error occurs, change the permissions:</span></span>

```console
chmod u+x <app_executable>
```

<span data-ttu-id="c23e7-188">Se l'app viene eseguita nel server ma non risponde tramite Internet, controllare il firewall del server e verificare che la porta 80 sia aperta.</span><span class="sxs-lookup"><span data-stu-id="c23e7-188">If the app runs on the server but fails to respond over the Internet, check the server's firewall and confirm that port 80 is open.</span></span> <span data-ttu-id="c23e7-189">Se si usa una macchina virtuale Ubuntu di Azure, aggiungere una regola del gruppo di sicurezza di rete (NSG) che abiliti il traffico in ingresso della porta 80.</span><span class="sxs-lookup"><span data-stu-id="c23e7-189">If using an Azure Ubuntu VM, add a Network Security Group (NSG) rule that enables inbound port 80 traffic.</span></span> <span data-ttu-id="c23e7-190">Non è necessario abilitare una regola per il traffico in uscita della porta 80, poiché il traffico in uscita viene autorizzato automaticamente quando viene abilitata la regola in ingresso.</span><span class="sxs-lookup"><span data-stu-id="c23e7-190">There's no need to enable an outbound port 80 rule, as the outbound traffic is automatically granted when the inbound rule is enabled.</span></span>

<span data-ttu-id="c23e7-191">Dopo aver eseguito il test dell'app, arrestare l'app con `Ctrl+C` al prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="c23e7-191">When done testing the app, shut the app down with `Ctrl+C` at the command prompt.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="c23e7-192">Monitoraggio dell'app</span><span class="sxs-lookup"><span data-stu-id="c23e7-192">Monitoring the app</span></span>

<span data-ttu-id="c23e7-193">Il server è configurato in modo da inoltrare le richieste eseguite a `http://<serveraddress>:80` all'app ASP.NET Core eseguita in Kestrel all'indirizzo `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="c23e7-193">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="c23e7-194">Tuttavia, Nginx non è configurato per la gestione del processo Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c23e7-194">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="c23e7-195">È possibile usare *systemd* e creare un file del servizio per avviare e monitorare l'app Web sottostante.</span><span class="sxs-lookup"><span data-stu-id="c23e7-195">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="c23e7-196">*systemd* è un sistema di inizializzazione che offre molte funzionalità potenti per l'avvio, l'arresto e la gestione dei processi.</span><span class="sxs-lookup"><span data-stu-id="c23e7-196">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="c23e7-197">Creare il file del servizio</span><span class="sxs-lookup"><span data-stu-id="c23e7-197">Create the service file</span></span>

<span data-ttu-id="c23e7-198">Creare il file di definizione del servizio:</span><span class="sxs-lookup"><span data-stu-id="c23e7-198">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="c23e7-199">Di seguito è riportato un esempio di file del servizio per l'app:</span><span class="sxs-lookup"><span data-stu-id="c23e7-199">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="c23e7-200">Se l'utente *www-data* non viene usato dalla configurazione, è necessario creare prima l'utente definito qui e assegnargli correttamente la proprietà dei file.</span><span class="sxs-lookup"><span data-stu-id="c23e7-200">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="c23e7-201">Linux dispone di un file system con distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="c23e7-201">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="c23e7-202">L'impostazione di ASPNETCORE_ENVIRONMENT su "Production" determina la ricerca del file di configurazione *appsettings.Production.json*, non di *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="c23e7-202">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="c23e7-203">Per alcuni valori (ad esempio, le stringhe di connessione SQL) è necessario usare caratteri di escape, in modo da consentire ai provider di configurazione di leggere le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="c23e7-203">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="c23e7-204">Usare il comando seguente per generare un valore con caratteri di escape corretti per l'uso nel file di configurazione:</span><span class="sxs-lookup"><span data-stu-id="c23e7-204">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="c23e7-205">Salvare il file e abilitare il servizio.</span><span class="sxs-lookup"><span data-stu-id="c23e7-205">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="c23e7-206">Avviare il servizio e verificare che sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="c23e7-206">Start the service and verify that it's running.</span></span>

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="c23e7-207">Con il proxy inverso configurato e Kestrel gestito tramite systemd, l'app Web è completamente configurata ed è possibile accedervi da un browser nel computer locale all'indirizzo `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="c23e7-207">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="c23e7-208">È anche possibile accedervi da un computer remoto, escludendo eventuali firewall che potrebbero impedirlo.</span><span class="sxs-lookup"><span data-stu-id="c23e7-208">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="c23e7-209">Se si esaminano le intestazioni di risposta, l'intestazione `Server` indica che l'app ASP.NET Core è gestita da Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c23e7-209">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="c23e7-210">Vista dei log</span><span class="sxs-lookup"><span data-stu-id="c23e7-210">Viewing logs</span></span>

<span data-ttu-id="c23e7-211">Poiché l'app Web che usa Kestrel viene gestita con `systemd`, tutti gli eventi e i processi vengono registrati in un giornale di registrazione centralizzato.</span><span class="sxs-lookup"><span data-stu-id="c23e7-211">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="c23e7-212">Tuttavia, questo giornale include tutte le voci per tutti i servizi e i processi gestiti da `systemd`.</span><span class="sxs-lookup"><span data-stu-id="c23e7-212">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="c23e7-213">Per visualizzare le voci specifiche di `kestrel-hellomvc.service`, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c23e7-213">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="c23e7-214">Per filtrare ulteriormente, opzioni come `--since today`, `--until 1 hour ago` o una combinazione delle stesse consentono di ridurre la quantità di voci restituite.</span><span class="sxs-lookup"><span data-stu-id="c23e7-214">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="c23e7-215">Protezione dell'app</span><span class="sxs-lookup"><span data-stu-id="c23e7-215">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="c23e7-216">Abilitare AppArmor</span><span class="sxs-lookup"><span data-stu-id="c23e7-216">Enable AppArmor</span></span>

<span data-ttu-id="c23e7-217">Linux Security Modules (LSM) è un framework che fa parte del kernel Linux a partire da Linux 2.6.</span><span class="sxs-lookup"><span data-stu-id="c23e7-217">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="c23e7-218">LSM supporta diverse implementazioni di moduli di protezione.</span><span class="sxs-lookup"><span data-stu-id="c23e7-218">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="c23e7-219">[AppArmor](https://wiki.ubuntu.com/AppArmor) è un modulo LSM che implementa un sistema di controllo di accesso obbligatorio che consente di circoscrivere il programma a un set limitato di risorse.</span><span class="sxs-lookup"><span data-stu-id="c23e7-219">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="c23e7-220">Verificare che AppArmor sia abilitato e configurato correttamente.</span><span class="sxs-lookup"><span data-stu-id="c23e7-220">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="c23e7-221">Configurazione del firewall</span><span class="sxs-lookup"><span data-stu-id="c23e7-221">Configuring the firewall</span></span>

<span data-ttu-id="c23e7-222">Chiudere tutte le porte esterne che non sono in uso.</span><span class="sxs-lookup"><span data-stu-id="c23e7-222">Close off all external ports that are not in use.</span></span> <span data-ttu-id="c23e7-223">Il firewall UFW (Uncomplicated Firewall) offre un front-end per `iptables` attraverso un'interfaccia della riga di comando per la configurazione del firewall.</span><span class="sxs-lookup"><span data-stu-id="c23e7-223">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="c23e7-224">Verificare che `ufw` sia configurato per consentire il traffico su tutte le porte necessarie.</span><span class="sxs-lookup"><span data-stu-id="c23e7-224">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="c23e7-225">Protezione di Nginx</span><span class="sxs-lookup"><span data-stu-id="c23e7-225">Securing Nginx</span></span>

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="c23e7-226">Modificare il nome di risposta Nginx</span><span class="sxs-lookup"><span data-stu-id="c23e7-226">Change the Nginx response name</span></span>

<span data-ttu-id="c23e7-227">Modificare *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="c23e7-227">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a><span data-ttu-id="c23e7-228">Configurare le opzioni</span><span class="sxs-lookup"><span data-stu-id="c23e7-228">Configure options</span></span>

<span data-ttu-id="c23e7-229">Configurare il server con moduli aggiuntivi obbligatori.</span><span class="sxs-lookup"><span data-stu-id="c23e7-229">Configure the server with additional required modules.</span></span> <span data-ttu-id="c23e7-230">Può essere utile usare un firewall per app Web, ad esempio [ModSecurity](https://www.modsecurity.org/), per la protezione avanzata dell'app.</span><span class="sxs-lookup"><span data-stu-id="c23e7-230">Consider using a web app firewall, such as [ModSecurity](https://www.modsecurity.org/), to harden the app.</span></span>

#### <a name="configure-ssl"></a><span data-ttu-id="c23e7-231">Configurare SSL</span><span class="sxs-lookup"><span data-stu-id="c23e7-231">Configure SSL</span></span>

* <span data-ttu-id="c23e7-232">Configurare il server in modo che sia in ascolto del traffico HTTPS sulla porta `443` specificando un certificato valido emesso da un'autorità di certificazione attendibile.</span><span class="sxs-lookup"><span data-stu-id="c23e7-232">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="c23e7-233">Rafforzare la protezione adottando alcune delle procedure descritte nel file */etc/nginx/nginx.conf* che segue.</span><span class="sxs-lookup"><span data-stu-id="c23e7-233">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="c23e7-234">Gli esempi includono la scelta di una crittografia più complessa e il reindirizzamento di tutto il traffico su HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c23e7-234">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="c23e7-235">L'aggiunta di un'intestazione `HTTP Strict-Transport-Security` (HSTS) garantisce che tutte le richieste successive vengano effettuate dal esclusivamente via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c23e7-235">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="c23e7-236">Non aggiungere l'intestazione Strict-Transport-Security o scegliere un elemento `max-age` appropriato se si prevede di disabilitare SSL in futuro.</span><span class="sxs-lookup"><span data-stu-id="c23e7-236">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="c23e7-237">Aggiungere il file di configurazione */etc/nginx/proxy.conf*:</span><span class="sxs-lookup"><span data-stu-id="c23e7-237">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="c23e7-238">Aggiungere il file di configurazione */etc/nginx/proxy.conf*.</span><span class="sxs-lookup"><span data-stu-id="c23e7-238">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="c23e7-239">L'esempio contiene entrambe le sezioni `http` e `server` in un unico file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="c23e7-239">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="c23e7-240">Proteggere Nginx dal clickjacking</span><span class="sxs-lookup"><span data-stu-id="c23e7-240">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="c23e7-241">Il clickjacking è una tecnica fraudolenta con cui i clic di un utente vengono "catturati"</span><span class="sxs-lookup"><span data-stu-id="c23e7-241">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="c23e7-242">e reindirizzati a un oggetto in un sito infetto.</span><span class="sxs-lookup"><span data-stu-id="c23e7-242">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="c23e7-243">Usare X-FRAME-OPTIONS per proteggere il sito.</span><span class="sxs-lookup"><span data-stu-id="c23e7-243">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="c23e7-244">Modificare il file *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="c23e7-244">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="c23e7-245">Aggiungere la riga `add_header X-Frame-Options "SAMEORIGIN";` e salvare il file, quindi riavviare Nginx.</span><span class="sxs-lookup"><span data-stu-id="c23e7-245">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="c23e7-246">Analisi del tipo MIME</span><span class="sxs-lookup"><span data-stu-id="c23e7-246">MIME-type sniffing</span></span>

<span data-ttu-id="c23e7-247">Questa intestazione impedisce alla maggior parte dei browser di dedurre dall'analisi del tipo MIME un tipo di contenuto diverso da quello dichiarato, poiché l'intestazione indica al browser di non eseguire l'override del tipo di contenuto della risposta.</span><span class="sxs-lookup"><span data-stu-id="c23e7-247">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="c23e7-248">Con l'opzione `nosniff`, se il server indica che il contenuto è "text/html", il browser ne esegue il rendering come "text/html".</span><span class="sxs-lookup"><span data-stu-id="c23e7-248">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="c23e7-249">Modificare il file *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="c23e7-249">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="c23e7-250">Aggiungere la riga `add_header X-Content-Type-Options "nosniff";` e salvare il file, quindi riavviare Nginx.</span><span class="sxs-lookup"><span data-stu-id="c23e7-250">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c23e7-251">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c23e7-251">Additional resources</span></span>

* <span data-ttu-id="c23e7-252">[Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages) (Nginx: Versioni binarie: Pacchetti ufficiali Debian/Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="c23e7-252">[Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)</span></span>
* [<span data-ttu-id="c23e7-253">Configurare ASP.NET Core per l'utilizzo di server proxy e servizi di bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="c23e7-253">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
* <span data-ttu-id="c23e7-254">[NGINX: Using the Forwarded header](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/) (NGINX: Uso dell'intestazione Forwarded)</span><span class="sxs-lookup"><span data-stu-id="c23e7-254">[NGINX: Using the Forwarded header](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)</span></span>
