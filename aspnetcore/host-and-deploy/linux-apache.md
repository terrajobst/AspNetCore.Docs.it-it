---
title: Hosting di ASP.NET Core in Linux con Apache
description: Informazioni su come configurare come server proxy inverso CentOS Apache reindirizzare il traffico HTTP a un'app web ASP.NET Core in esecuzione su Kestrel.
author: spboyer
manager: wpickett
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 5a8a035ff3f127d01655888d4f83a871645b0bf5
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="fd527-103">Hosting di ASP.NET Core in Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="fd527-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="fd527-104">Di [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="fd527-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="fd527-105">Utilizzando questa Guida, informazioni su come configurare [Apache](https://httpd.apache.org/) come server proxy inverso [CentOS 7](https://www.centos.org/) reindirizzare il traffico HTTP a un'app web ASP.NET Core in esecuzione in [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="fd527-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="fd527-106">Il [mod_proxy estensione](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) e moduli correlati creare proxy inverso del server.</span><span class="sxs-lookup"><span data-stu-id="fd527-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fd527-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="fd527-107">Prerequisites</span></span>

1. <span data-ttu-id="fd527-108">Server che esegue CentOS 7 con un account utente standard con privilegi sudo</span><span class="sxs-lookup"><span data-stu-id="fd527-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="fd527-109">App ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fd527-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="fd527-110">Pubblicare l'app</span><span class="sxs-lookup"><span data-stu-id="fd527-110">Publish the app</span></span>

<span data-ttu-id="fd527-111">Pubblicare l'app come un [distribuzione indipendente](/dotnet/core/deploying/#self-contained-deployments-scd) nella configurazione di rilascio per il runtime CentOS 7 (`centos.7-x64`).</span><span class="sxs-lookup"><span data-stu-id="fd527-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="fd527-112">Copiare il contenuto del *bin/Release/netcoreapp2.0/centos.7-x64/publish* cartella al server tramite SCP, FTP o altro metodo di trasferimento di file.</span><span class="sxs-lookup"><span data-stu-id="fd527-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="fd527-113">In uno scenario di distribuzione di produzione, un flusso di lavoro di integrazione continua esegue le operazioni di pubblicazione dell'app e copiare le risorse nel server di.</span><span class="sxs-lookup"><span data-stu-id="fd527-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="fd527-114">Configurazione di un server proxy</span><span class="sxs-lookup"><span data-stu-id="fd527-114">Configure a proxy server</span></span>

<span data-ttu-id="fd527-115">Un proxy inverso è una configurazione comune per la gestione delle applicazioni web dinamiche.</span><span class="sxs-lookup"><span data-stu-id="fd527-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="fd527-116">Il proxy inverso termina la richiesta HTTP e lo inoltra all'applicazione ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fd527-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="fd527-117">È un server proxy che inoltra le richieste client a un altro server invece che soddisfano le richieste di se stesso.</span><span class="sxs-lookup"><span data-stu-id="fd527-117">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="fd527-118">Un proxy inverso inoltra a una destinazione fissa, in genere per conto di client non autorizzati.</span><span class="sxs-lookup"><span data-stu-id="fd527-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="fd527-119">In questa guida Apache è configurato come proxy inverso in esecuzione nello stesso server che gestisce l'applicazione ASP.NET Core Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd527-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="fd527-120">Poiché le richieste vengono inoltrate da proxy inverso, utilizzare il Middleware di intestazioni inoltrati dal [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="fd527-120">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="fd527-121">Gli aggiornamenti di middleware di `Request.Scheme`, usando il `X-Forwarded-Proto` intestazione, in modo che gli URI di reindirizzamento e altri criteri di sicurezza funzionino correttamente.</span><span class="sxs-lookup"><span data-stu-id="fd527-121">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="fd527-122">Quando si utilizza qualsiasi tipo di middleware di autenticazione, il Middleware di intestazioni inoltrati deve eseguiti per primo.</span><span class="sxs-lookup"><span data-stu-id="fd527-122">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="fd527-123">Questo ordinamento garantisce che il middleware di autenticazione può utilizzare i valori di intestazione e generare l'URI di reindirizzamento corretto.</span><span class="sxs-lookup"><span data-stu-id="fd527-123">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd527-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd527-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="fd527-125">Richiamare il [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metodo `Startup.Configure` prima di chiamare [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) o simile middleware di schema di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="fd527-125">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="fd527-126">Configurare il middleware di inoltrare il `X-Forwarded-For` e `X-Forwarded-Proto` intestazioni:</span><span class="sxs-lookup"><span data-stu-id="fd527-126">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd527-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fd527-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="fd527-128">Richiamare il [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metodo `Startup.Configure` prima di chiamare [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) e [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) o schema di autenticazione simili middleware.</span><span class="sxs-lookup"><span data-stu-id="fd527-128">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="fd527-129">Configurare il middleware di inoltrare il `X-Forwarded-For` e `X-Forwarded-Proto` intestazioni:</span><span class="sxs-lookup"><span data-stu-id="fd527-129">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="fd527-130">Se non [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) specificato per il middleware, le intestazioni predefinite per l'inoltro sono `None`.</span><span class="sxs-lookup"><span data-stu-id="fd527-130">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="fd527-131">Configurazione aggiuntiva potrebbe essere necessaria per le app ospitate dietro a servizi di bilanciamento del carico e server proxy.</span><span class="sxs-lookup"><span data-stu-id="fd527-131">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="fd527-132">Per altre informazioni, vedere [configurare ASP.NET Core per lavorare con i server proxy e bilanciamento del carico](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="fd527-132">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-apache"></a><span data-ttu-id="fd527-133">Installare Apache</span><span class="sxs-lookup"><span data-stu-id="fd527-133">Install Apache</span></span>

<span data-ttu-id="fd527-134">CentOS i pacchetti di aggiornamento alle versioni stabili più recenti:</span><span class="sxs-lookup"><span data-stu-id="fd527-134">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="fd527-135">Installare il server web Apache in CentOS con un singolo `yum` comando:</span><span class="sxs-lookup"><span data-stu-id="fd527-135">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="fd527-136">Esempio di output dopo l'esecuzione del comando:</span><span class="sxs-lookup"><span data-stu-id="fd527-136">Sample output after running the command:</span></span>

```bash
Downloading packages:
httpd-2.4.6-40.el7.centos.4.x86_64.rpm               | 2.7 MB  00:00:01
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
Installing : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 
Verifying  : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 

Installed:
httpd.x86_64 0:2.4.6-40.el7.centos.4

Complete!
```

> [!NOTE]
> <span data-ttu-id="fd527-137">In questo esempio, l'output rispecchi httpd.86_64 poiché la versione CentOS 7 a 64 bit.</span><span class="sxs-lookup"><span data-stu-id="fd527-137">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="fd527-138">Per verificare dove è installato Apache, eseguire `whereis httpd` da un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="fd527-138">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="fd527-139">Configurare Apache per il proxy inverso</span><span class="sxs-lookup"><span data-stu-id="fd527-139">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="fd527-140">I file di configurazione per Apache si trovano all'interno della directory `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="fd527-140">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="fd527-141">Qualsiasi file con il *.conf* estensione viene elaborata in ordine alfabetico oltre i file di configurazione del modulo in `/etc/httpd/conf.modules.d/`, che contiene tutte le configurazioni di file necessarie per caricare i moduli.</span><span class="sxs-lookup"><span data-stu-id="fd527-141">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="fd527-142">Creare un file di configurazione denominato *hellomvc.conf*, per l'app:</span><span class="sxs-lookup"><span data-stu-id="fd527-142">Create a configuration file, named *hellomvc.conf*, for the app:</span></span>

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ServerName www.example.com
    ServerAlias *.example.com
    ErrorLog ${APACHE_LOG_DIR}hellomvc-error.log
    CustomLog ${APACHE_LOG_DIR}hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="fd527-143">Il `VirtualHost` blocco può comparire più volte, in uno o più file in un server.</span><span class="sxs-lookup"><span data-stu-id="fd527-143">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="fd527-144">Nel file di configurazione precedente Apache accetta traffico pubblico sulla porta 80.</span><span class="sxs-lookup"><span data-stu-id="fd527-144">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="fd527-145">Il dominio `www.example.com` viene gestita e `*.example.com` alias risolve allo stesso sito.</span><span class="sxs-lookup"><span data-stu-id="fd527-145">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="fd527-146">Vedere [supporto basato sul nome host virtuale](https://httpd.apache.org/docs/current/vhosts/name-based.html) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="fd527-146">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="fd527-147">Le richieste vengono elaborate dal proxy alla radice alla porta 5000 del server 127.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="fd527-147">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="fd527-148">Per la comunicazione bidirezionale, `ProxyPass` e `ProxyPassReverse` sono necessari.</span><span class="sxs-lookup"><span data-stu-id="fd527-148">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span>

> [!WARNING]
> <span data-ttu-id="fd527-149">L'impossibilità di specificare una corretta [ServerName direttiva](https://httpd.apache.org/docs/current/mod/core.html#servername) nel **VirtualHost** blocco espone l'app a vulnerabilità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="fd527-149">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="fd527-150">Associazione con caratteri jolly sottodominio (ad esempio, `*.example.com`) non comporta il rischio di sicurezza se è possibile controllare il dominio padre intero (in contrapposizione a `*.com`, che è vulnerabile).</span><span class="sxs-lookup"><span data-stu-id="fd527-150">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="fd527-151">Vedere la [sezione 5.4 di RFC7230](https://tools.ietf.org/html/rfc7230#section-5.4) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="fd527-151">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="fd527-152">Registrazione può essere configurata `VirtualHost` utilizzando `ErrorLog` e `CustomLog` direttive.</span><span class="sxs-lookup"><span data-stu-id="fd527-152">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="fd527-153">`ErrorLog` è il percorso in cui i registri del server, errori e `CustomLog` imposta il nome e il formato del file di log.</span><span class="sxs-lookup"><span data-stu-id="fd527-153">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="fd527-154">In questo caso, si tratta in cui vengono registrate informazioni di richiesta.</span><span class="sxs-lookup"><span data-stu-id="fd527-154">In this case, this is where request information is logged.</span></span> <span data-ttu-id="fd527-155">È presente una riga per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="fd527-155">There's one line for each request.</span></span>

<span data-ttu-id="fd527-156">Salvare il file e il test della configurazione.</span><span class="sxs-lookup"><span data-stu-id="fd527-156">Save the file and test the configuration.</span></span> <span data-ttu-id="fd527-157">Se tutto ciò viene superato, la risposta deve essere `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="fd527-157">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="fd527-158">Riavviare Apache:</span><span class="sxs-lookup"><span data-stu-id="fd527-158">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="fd527-159">Monitoraggio dell'app</span><span class="sxs-lookup"><span data-stu-id="fd527-159">Monitoring the app</span></span>

<span data-ttu-id="fd527-160">Apache è ora configurato per inoltrare le richieste effettuate a `http://localhost:80` all'app ASP.NET Core in esecuzione in Kestrel in `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="fd527-160">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="fd527-161">Tuttavia, Apache perché non è configurato per gestire il processo Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd527-161">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="fd527-162">Utilizzare *systemd* e creare un file del servizio per avviare e monitorare l'app web sottostante.</span><span class="sxs-lookup"><span data-stu-id="fd527-162">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="fd527-163">*systemd* è un sistema di inizializzazione che offre molte funzionalità potenti per l'avvio, l'arresto e la gestione dei processi.</span><span class="sxs-lookup"><span data-stu-id="fd527-163">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="fd527-164">Creare il file del servizio</span><span class="sxs-lookup"><span data-stu-id="fd527-164">Create the service file</span></span>

<span data-ttu-id="fd527-165">Creare il file di definizione del servizio:</span><span class="sxs-lookup"><span data-stu-id="fd527-165">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="fd527-166">Un file del servizio di esempio per l'app:</span><span class="sxs-lookup"><span data-stu-id="fd527-166">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if dotnet service crashes
RestartSec=10
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

> [!NOTE]
> <span data-ttu-id="fd527-167">**Utente** &mdash; se l'utente *apache* non viene usata dalla configurazione, è necessario creare prima l'utente e specificato le proprietà appropriate per i file.</span><span class="sxs-lookup"><span data-stu-id="fd527-167">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="fd527-168">Salvare il file e attivare il servizio:</span><span class="sxs-lookup"><span data-stu-id="fd527-168">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="fd527-169">Avviare il servizio e verificare che sia in esecuzione:</span><span class="sxs-lookup"><span data-stu-id="fd527-169">Start the service and verify that it's running:</span></span>

```bash
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="fd527-170">Con il proxy inverso configurato e gestite tramite Kestrel *systemd*, l'app web è completamente configurato ed è possibile accedervi da un browser del computer locale in `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="fd527-170">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="fd527-171">Esaminare le intestazioni di risposta, il **Server** intestazione indica che l'applicazione ASP.NET Core viene gestita da Kestrel:</span><span class="sxs-lookup"><span data-stu-id="fd527-171">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="fd527-172">Vista dei log</span><span class="sxs-lookup"><span data-stu-id="fd527-172">Viewing logs</span></span>

<span data-ttu-id="fd527-173">Poiché l'app web utilizzando Kestrel viene gestita mediante *systemd*, processi e gli eventi vengono registrati in una registrazione centralizzata.</span><span class="sxs-lookup"><span data-stu-id="fd527-173">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="fd527-174">Tuttavia, queste registrazioni includono voci per tutti i servizi e processi gestiti da *systemd*.</span><span class="sxs-lookup"><span data-stu-id="fd527-174">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="fd527-175">Per visualizzare le voci specifiche di `kestrel-hellomvc.service`, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="fd527-175">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="fd527-176">Per il filtro di tempo, specificare opzioni della fase con il comando.</span><span class="sxs-lookup"><span data-stu-id="fd527-176">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="fd527-177">Ad esempio, utilizzare `--since today` per filtrare per il giorno corrente o `--until 1 hour ago` per visualizzare le voci dell'ora precedente.</span><span class="sxs-lookup"><span data-stu-id="fd527-177">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="fd527-178">Per ulteriori informazioni, vedere il [pagina man per journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="fd527-178">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="fd527-179">Protezione dell'app</span><span class="sxs-lookup"><span data-stu-id="fd527-179">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="fd527-180">Configurare firewall</span><span class="sxs-lookup"><span data-stu-id="fd527-180">Configure firewall</span></span>

<span data-ttu-id="fd527-181">*Firewalld* è un daemon dinamico per gestire il firewall con supporto per aree di rete.</span><span class="sxs-lookup"><span data-stu-id="fd527-181">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="fd527-182">Porte e il filtraggio dei pacchetti può comunque essere gestiti da iptables.</span><span class="sxs-lookup"><span data-stu-id="fd527-182">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="fd527-183">*Firewalld* deve essere installato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="fd527-183">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="fd527-184">`yum` può essere utilizzato per installare il pacchetto o verificare che sia installato.</span><span class="sxs-lookup"><span data-stu-id="fd527-184">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="fd527-185">Utilizzare `firewalld` aprire solo le porte necessarie per l'app.</span><span class="sxs-lookup"><span data-stu-id="fd527-185">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="fd527-186">In questo caso vengono usate le porte 80 e 443.</span><span class="sxs-lookup"><span data-stu-id="fd527-186">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="fd527-187">I seguenti comandi set in modo permanente le porte 80 e 443 per aprire:</span><span class="sxs-lookup"><span data-stu-id="fd527-187">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="fd527-188">Ricaricare le impostazioni del firewall.</span><span class="sxs-lookup"><span data-stu-id="fd527-188">Reload the firewall settings.</span></span> <span data-ttu-id="fd527-189">Controllare i servizi disponibili e le porte nell'area predefinita.</span><span class="sxs-lookup"><span data-stu-id="fd527-189">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="fd527-190">Sono disponibili le opzioni controllando `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="fd527-190">Options are available by inspecting `firewall-cmd -h`.</span></span>

```bash 
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

```bash
public (default, active)
interfaces: eth0
sources: 
services: dhcpv6-client
ports: 443/tcp 80/tcp
masquerade: no
forward-ports: 
icmp-blocks: 
rich rules: 
```

### <a name="ssl-configuration"></a><span data-ttu-id="fd527-191">Configurazione SSL</span><span class="sxs-lookup"><span data-stu-id="fd527-191">SSL configuration</span></span>

<span data-ttu-id="fd527-192">Per configurare Apache per SSL, il *mod_ssl* modulo è utilizzato.</span><span class="sxs-lookup"><span data-stu-id="fd527-192">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="fd527-193">Quando il *httpd* modulo è stato installato, il *mod_ssl* modulo è stato anche installato.</span><span class="sxs-lookup"><span data-stu-id="fd527-193">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="fd527-194">Se non è stato installato, utilizzare `yum` per aggiungerlo alla configurazione.</span><span class="sxs-lookup"><span data-stu-id="fd527-194">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="fd527-195">Per attivare SSL, installare il `mod_rewrite` per abilitare la riscrittura URL:</span><span class="sxs-lookup"><span data-stu-id="fd527-195">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="fd527-196">Modificare il *hellomvc.conf* file per abilitare la riscrittura degli URL e proteggere la comunicazione sulla porta 443:</span><span class="sxs-lookup"><span data-stu-id="fd527-196">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

> [!NOTE]
> <span data-ttu-id="fd527-197">In questo esempio utilizza un certificato generato localmente.</span><span class="sxs-lookup"><span data-stu-id="fd527-197">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="fd527-198">**SSLCertificateFile** deve essere il file del certificato primario per il nome di dominio.</span><span class="sxs-lookup"><span data-stu-id="fd527-198">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="fd527-199">**SSLCertificateKeyFile** deve essere il file di chiave generato CSR viene creato.</span><span class="sxs-lookup"><span data-stu-id="fd527-199">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="fd527-200">**SSLCertificateChainFile** deve essere il file di certificato intermedio (se presente) che è stato fornito dall'autorità di certificazione.</span><span class="sxs-lookup"><span data-stu-id="fd527-200">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="fd527-201">Salvare il file e la configurazione di test:</span><span class="sxs-lookup"><span data-stu-id="fd527-201">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="fd527-202">Riavviare Apache:</span><span class="sxs-lookup"><span data-stu-id="fd527-202">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="fd527-203">Altri suggerimenti di Apache</span><span class="sxs-lookup"><span data-stu-id="fd527-203">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="fd527-204">Intestazioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="fd527-204">Additional headers</span></span>

<span data-ttu-id="fd527-205">Per proteggere contro gli attacchi, esistono alcune intestazioni che devono essere modificate o aggiunta.</span><span class="sxs-lookup"><span data-stu-id="fd527-205">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="fd527-206">Verificare che il `mod_headers` modulo è installato:</span><span class="sxs-lookup"><span data-stu-id="fd527-206">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="fd527-207">Apache protetto da attacchi clickjacking</span><span class="sxs-lookup"><span data-stu-id="fd527-207">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="fd527-208">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), nota anche come un *dell'interfaccia utente ricorso attacco*, costituisce un attacco dannoso, in cui un visitatore di un sito Web sia indotto a fare clic su un collegamento o un pulsante in una pagina diversa rispetto a cui sta attualmente visitando.</span><span class="sxs-lookup"><span data-stu-id="fd527-208">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="fd527-209">Utilizzare `X-FRAME-OPTIONS` per proteggere il sito.</span><span class="sxs-lookup"><span data-stu-id="fd527-209">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="fd527-210">Modificare il *httpd. conf* file:</span><span class="sxs-lookup"><span data-stu-id="fd527-210">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="fd527-211">Aggiungere la riga `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="fd527-211">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="fd527-212">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="fd527-212">Save the file.</span></span> <span data-ttu-id="fd527-213">Riavviare Apache.</span><span class="sxs-lookup"><span data-stu-id="fd527-213">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="fd527-214">Analisi del tipo MIME</span><span class="sxs-lookup"><span data-stu-id="fd527-214">MIME-type sniffing</span></span>

<span data-ttu-id="fd527-215">Il `X-Content-Type-Options` intestazione impedisce di Internet Explorer a *analisi MIME* (determinazione di un file `Content-Type` dal contenuto del file).</span><span class="sxs-lookup"><span data-stu-id="fd527-215">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="fd527-216">Se il server imposta il `Content-Type` intestazione `text/html` con il `nosniff` set di opzioni, Internet Explorer visualizza il contenuto come `text/html` indipendentemente dal contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="fd527-216">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="fd527-217">Modificare il *httpd. conf* file:</span><span class="sxs-lookup"><span data-stu-id="fd527-217">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="fd527-218">Aggiungere la riga `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="fd527-218">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="fd527-219">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="fd527-219">Save the file.</span></span> <span data-ttu-id="fd527-220">Riavviare Apache.</span><span class="sxs-lookup"><span data-stu-id="fd527-220">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="fd527-221">Bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="fd527-221">Load Balancing</span></span> 

<span data-ttu-id="fd527-222">Questo esempio illustra come impostare e configurare Apache CentOS 7 e Kestrel nello stesso computer dell'istanza.</span><span class="sxs-lookup"><span data-stu-id="fd527-222">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="fd527-223">Per non disporre di un singolo punto di errore. utilizzando *mod_proxy_balancer* e la modifica di **VirtualHost** consentirebbe la gestione di più istanze delle App web dietro il server proxy di Apache.</span><span class="sxs-lookup"><span data-stu-id="fd527-223">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="fd527-224">Nel file di configurazione illustrato di seguito, un'istanza aggiuntiva del `hellomvc` app è configurato per l'esecuzione sulla porta 5001.</span><span class="sxs-lookup"><span data-stu-id="fd527-224">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="fd527-225">Il *Proxy* sezione è impostata con una configurazione di bilanciamento del carico con due membri per il bilanciamento del carico *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="fd527-225">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPass / balancer://mycluster/ 

    ProxyPassReverse / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5001/

    <Proxy balancer://mycluster>
        BalancerMember http://127.0.0.1:5000
        BalancerMember http://127.0.0.1:5001 
        ProxySet lbmethod=byrequests
    </Proxy>

    <Location />
        SetHandler balancer
    </Location>
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a><span data-ttu-id="fd527-226">Limiti di velocità</span><span class="sxs-lookup"><span data-stu-id="fd527-226">Rate Limits</span></span>
<span data-ttu-id="fd527-227">Utilizzando *mod_ratelimit*, incluso nel *httpd* modulo, la larghezza di banda del client può essere limitato:</span><span class="sxs-lookup"><span data-stu-id="fd527-227">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="fd527-228">Il file di esempio Limita larghezza di banda come 600 KB al secondo nel percorso radice:</span><span class="sxs-lookup"><span data-stu-id="fd527-228">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
