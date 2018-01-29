---
title: Hosting di ASP.NET Core in Linux con Nginx
description: Viene descritto come configurare Nginx come un proxy inverso in Ubuntu 16.04 per inoltrare il traffico HTTP a un'app web ASP.NET Core in esecuzione su Kestrel.
author: rick-anderson
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 08/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 465f1391ef4ff9492d9aed48cb32da0659ceda41
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="8ac20-103">Hosting di ASP.NET Core in Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="8ac20-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="8ac20-104">Di [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="8ac20-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="8ac20-105">Questa guida spiega come configurare un ambiente ASP.NET Core pronto per la produzione in un server Ubuntu 16.04.</span><span class="sxs-lookup"><span data-stu-id="8ac20-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

<span data-ttu-id="8ac20-106">**Nota:** per Ubuntu 14.04, *supervisord* è consigliabile come soluzione per il monitoraggio del processo Kestrel.</span><span class="sxs-lookup"><span data-stu-id="8ac20-106">**Note:** For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="8ac20-107">*systemd* non è disponibile in Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="8ac20-107">*systemd* isn't available on Ubuntu 14.04.</span></span> [<span data-ttu-id="8ac20-108">Vedere la versione precedente di questo documento</span><span class="sxs-lookup"><span data-stu-id="8ac20-108">See previous version of this document</span></span>](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

<span data-ttu-id="8ac20-109">In questa guida:</span><span class="sxs-lookup"><span data-stu-id="8ac20-109">This guide:</span></span>

* <span data-ttu-id="8ac20-110">Inserisce un'app di ASP.NET Core esistente con un server proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="8ac20-110">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="8ac20-111">Imposta backup del server proxy inverso per inoltrare le richieste al server web Kestrel.</span><span class="sxs-lookup"><span data-stu-id="8ac20-111">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="8ac20-112">Assicura che l'app web viene eseguito all'avvio come daemon.</span><span class="sxs-lookup"><span data-stu-id="8ac20-112">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="8ac20-113">Consente di configurare uno strumento di gestione del processo per riavviare l'app web.</span><span class="sxs-lookup"><span data-stu-id="8ac20-113">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8ac20-114">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="8ac20-114">Prerequisites</span></span>

1. <span data-ttu-id="8ac20-115">Accesso a un server Ubuntu 16.04 con un account utente standard con privilegio sudo</span><span class="sxs-lookup"><span data-stu-id="8ac20-115">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
1. <span data-ttu-id="8ac20-116">Un'app di ASP.NET Core esistente</span><span class="sxs-lookup"><span data-stu-id="8ac20-116">An existing ASP.NET Core app</span></span>

## <a name="copy-over-the-app"></a><span data-ttu-id="8ac20-117">Copiare l'app</span><span class="sxs-lookup"><span data-stu-id="8ac20-117">Copy over the app</span></span>

<span data-ttu-id="8ac20-118">Eseguire `dotnet publish` dall'ambiente di sviluppo per creare un pacchetto per un'applicazione in una directory indipendente che può essere eseguita sul server.</span><span class="sxs-lookup"><span data-stu-id="8ac20-118">Run `dotnet publish` from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="8ac20-119">Copia l'app ASP.NET Core per il server utilizzando qualsiasi strumento integra nel flusso di lavoro dell'organizzazione (ad esempio, SCP, FTP).</span><span class="sxs-lookup"><span data-stu-id="8ac20-119">Copy the ASP.NET Core app to the server using whatever tool integrates into the organization's workflow (for example, SCP, FTP).</span></span> <span data-ttu-id="8ac20-120">Testare l'applicazione, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="8ac20-120">Test the app, for example:</span></span>

* <span data-ttu-id="8ac20-121">Dalla riga di comando, eseguire `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="8ac20-121">From the command line, run `dotnet <app_assembly>.dll`.</span></span>
* <span data-ttu-id="8ac20-122">In un browser passare a `http://<serveraddress>:<port>` per verificare se l'applicazione funziona in Linux.</span><span class="sxs-lookup"><span data-stu-id="8ac20-122">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="8ac20-123">Configurare un server proxy inverso</span><span class="sxs-lookup"><span data-stu-id="8ac20-123">Configure a reverse proxy server</span></span>

<span data-ttu-id="8ac20-124">Un proxy inverso è una configurazione comune per la gestione delle applicazioni web dinamiche.</span><span class="sxs-lookup"><span data-stu-id="8ac20-124">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="8ac20-125">Un proxy inverso termina la richiesta HTTP e lo inoltra all'applicazione ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8ac20-125">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="8ac20-126">Perché usare un server proxy inverso?</span><span class="sxs-lookup"><span data-stu-id="8ac20-126">Why use a reverse proxy server?</span></span>

<span data-ttu-id="8ac20-127">Kestrel funziona perfettamente per la gestione del contenuto dinamico da ASP.NET Core, tuttavia le parti relative al Web non sono così ricche di funzionalità come i server IIS, Apache o Nginx.</span><span class="sxs-lookup"><span data-stu-id="8ac20-127">Kestrel is great for serving dynamic content from ASP.NET Core; however, the web serving parts aren’t as feature rich as servers like IIS, Apache, or Nginx.</span></span> <span data-ttu-id="8ac20-128">Un server proxy inverso è in grado di eseguire l'offload di attività come la gestione del contenuto statico, le richieste di memorizzazione nella cache, la compressione delle richieste e la terminazione SSL dal server HTTP.</span><span class="sxs-lookup"><span data-stu-id="8ac20-128">A reverse proxy server can offload work like serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="8ac20-129">Un server proxy inverso può risiedere in un computer dedicato o essere distribuito insieme a un server HTTP.</span><span class="sxs-lookup"><span data-stu-id="8ac20-129">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="8ac20-130">Ai fini di questa guida viene usata una singola istanza di Nginx.</span><span class="sxs-lookup"><span data-stu-id="8ac20-130">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="8ac20-131">Viene eseguito sullo stesso server, insieme al server HTTP.</span><span class="sxs-lookup"><span data-stu-id="8ac20-131">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="8ac20-132">In base ai requisiti, un'impostazione diversa potrebbe essere scelto.</span><span class="sxs-lookup"><span data-stu-id="8ac20-132">Based on requirements, a different setup may be choosen.</span></span>

<span data-ttu-id="8ac20-133">Poiché le richieste vengono inoltrate dal proxy inverso, usare il middleware `ForwardedHeaders` dal pacchetto `Microsoft.AspNetCore.HttpOverrides`.</span><span class="sxs-lookup"><span data-stu-id="8ac20-133">Because requests are forwarded by reverse proxy, use the `ForwardedHeaders` middleware from the `Microsoft.AspNetCore.HttpOverrides` package.</span></span> <span data-ttu-id="8ac20-134">Questo middleware aggiorna `Request.Scheme` usando l'intestazione `X-Forwarded-Proto`, in modo che gli URI di reindirizzamento e altri criteri di sicurezza funzionino correttamente.</span><span class="sxs-lookup"><span data-stu-id="8ac20-134">This middleware updates `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="8ac20-135">Quando si configura un server proxy inverso, il middleware di autenticazione richiede `UseForwardedHeaders` per essere eseguito per primo.</span><span class="sxs-lookup"><span data-stu-id="8ac20-135">When setting up a reverse proxy server, the authentication middleware needs `UseForwardedHeaders` to run first.</span></span> <span data-ttu-id="8ac20-136">Questo ordine garantisce che il middleware di autenticazione sia in grado di usare i valori interessati e di generare gli URI di reindirizzamento corretti.</span><span class="sxs-lookup"><span data-stu-id="8ac20-136">This ordering ensures that the authentication middleware can consume the affected values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8ac20-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8ac20-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8ac20-138">Richiamare il metodo `UseForwardedHeaders` (nel metodo `Configure` di *Startup.cs*) prima di chiamare `UseAuthentication` o un middleware con schema di autenticazione simile:</span><span class="sxs-lookup"><span data-stu-id="8ac20-138">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseAuthentication` or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8ac20-139">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8ac20-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8ac20-140">Richiamare il metodo `UseForwardedHeaders` (nel metodo `Configure` di *Startup.cs*) prima di chiamare `UseIdentity` e `UseFacebookAuthentication` o un middleware con schema di autenticazione simile:</span><span class="sxs-lookup"><span data-stu-id="8ac20-140">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseIdentity` and `UseFacebookAuthentication` or similar authentication scheme middleware:</span></span>

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

### <a name="install-nginx"></a><span data-ttu-id="8ac20-141">Installare Nginx</span><span class="sxs-lookup"><span data-stu-id="8ac20-141">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="8ac20-142">Se non saranno installati i moduli Nginx facoltativi, potrebbe essere necessario compilazione Nginx dall'origine.</span><span class="sxs-lookup"><span data-stu-id="8ac20-142">If optional Nginx modules will be installed, building Nginx from source might be required.</span></span>

<span data-ttu-id="8ac20-143">Usare `apt-get` per installare Nginx.</span><span class="sxs-lookup"><span data-stu-id="8ac20-143">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="8ac20-144">Il programma di installazione crea uno script di inizializzazione System V che esegue Nginx come daemon all'avvio del sistema.</span><span class="sxs-lookup"><span data-stu-id="8ac20-144">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="8ac20-145">Poiché Nginx è stato installato per la prima volta, avviarlo in modo esplicito eseguendo:</span><span class="sxs-lookup"><span data-stu-id="8ac20-145">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="8ac20-146">Verificare che un browser visualizzi la pagina di destinazione predefinita per Nginx.</span><span class="sxs-lookup"><span data-stu-id="8ac20-146">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="8ac20-147">Configurare Nginx</span><span class="sxs-lookup"><span data-stu-id="8ac20-147">Configure Nginx</span></span>

<span data-ttu-id="8ac20-148">Per configurare come un proxy inverso per inoltrare le richieste per l'app ASP.NET Core Nginx modificare `/etc/nginx/sites-available/default`.</span><span class="sxs-lookup"><span data-stu-id="8ac20-148">To configure Nginx as a reverse proxy to forward requests to our ASP.NET Core app, modify `/etc/nginx/sites-available/default`.</span></span> <span data-ttu-id="8ac20-149">Aprirlo in un editor di testo e sostituire il contenuto con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="8ac20-149">Open it in a text editor, and replace the contents with the following:</span></span>

```
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

<span data-ttu-id="8ac20-150">Questo file di configurazione Nginx inoltra il traffico pubblico in ingresso dalla porta `80` alla porta `5000`.</span><span class="sxs-lookup"><span data-stu-id="8ac20-150">This Nginx configuration file forwards incoming public traffic from port `80` to port `5000`.</span></span>

<span data-ttu-id="8ac20-151">Una volta stabilita la configurazione di Nginx, eseguire `sudo nginx -t` per verificare la sintassi dei file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="8ac20-151">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="8ac20-152">Se il test di file di configurazione ha esito positivo, forzare Nginx per rendere effettive le modifiche eseguendo `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="8ac20-152">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="8ac20-153">Monitoraggio dell'app</span><span class="sxs-lookup"><span data-stu-id="8ac20-153">Monitoring the app</span></span>

<span data-ttu-id="8ac20-154">Il server è configurato per inoltrare le richieste effettuate a `http://<serveraddress>:80` al ASP.NET Core in esecuzione l'app in Kestrel in `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="8ac20-154">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="8ac20-155">Tuttavia, Nginx perché non è configurato per gestire il processo Kestrel.</span><span class="sxs-lookup"><span data-stu-id="8ac20-155">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="8ac20-156">*systemd* può essere utilizzato per creare un file del servizio per avviare e monitorare l'app web sottostante.</span><span class="sxs-lookup"><span data-stu-id="8ac20-156">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="8ac20-157">*systemd* è un sistema di inizializzazione che offre molte funzionalità potenti per l'avvio, l'arresto e la gestione dei processi.</span><span class="sxs-lookup"><span data-stu-id="8ac20-157">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="8ac20-158">Creare il file del servizio</span><span class="sxs-lookup"><span data-stu-id="8ac20-158">Create the service file</span></span>

<span data-ttu-id="8ac20-159">Creare il file di definizione del servizio:</span><span class="sxs-lookup"><span data-stu-id="8ac20-159">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="8ac20-160">Di seguito è un esempio di file per l'applicazione del servizio:</span><span class="sxs-lookup"><span data-stu-id="8ac20-160">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
RestartSec=10  # Restart service after 10 seconds if dotnet service crashes
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="8ac20-161">**Nota:** se l'utente *www dati* non viene usata dalla configurazione, è necessario creare innanzitutto l'utente definito qui e ha le proprietà appropriate per i file.</span><span class="sxs-lookup"><span data-stu-id="8ac20-161">**Note:** If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>
<span data-ttu-id="8ac20-162">**Nota:** Linux è un sistema di file tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="8ac20-162">**Note:** Linux has a case-sensitive file system.</span></span> <span data-ttu-id="8ac20-163">L'impostazione di ASPNETCORE_ENVIRONMENT su "Produzione" produce la ricerca del file di configurazione *appsettings. Production.JSON*, non *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="8ac20-163">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

<span data-ttu-id="8ac20-164">Salvare il file e abilitare il servizio.</span><span class="sxs-lookup"><span data-stu-id="8ac20-164">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="8ac20-165">Avviare il servizio e verificare che sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="8ac20-165">Start the service and verify that it's running.</span></span>

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

<span data-ttu-id="8ac20-166">Con il proxy inverso configurate e gestite tramite systemd Kestrel, l'app web è completamente configurato ed è possibile accedervi da un browser del computer locale in `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="8ac20-166">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="8ac20-167">È inoltre possibile accedere da un computer remoto, impedendo qualsiasi firewall che potrebbe essere bloccato.</span><span class="sxs-lookup"><span data-stu-id="8ac20-167">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="8ac20-168">Esaminare le intestazioni di risposta, il `Server` intestazione Mostra l'app ASP.NET Core servito dal Kestrel.</span><span class="sxs-lookup"><span data-stu-id="8ac20-168">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="8ac20-169">Vista dei log</span><span class="sxs-lookup"><span data-stu-id="8ac20-169">Viewing logs</span></span>

<span data-ttu-id="8ac20-170">Poiché l'app web utilizzando Kestrel viene gestita mediante `systemd`, tutti gli eventi e i processi vengono registrati in una registrazione centralizzata.</span><span class="sxs-lookup"><span data-stu-id="8ac20-170">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="8ac20-171">Tuttavia, questo giornale include tutte le voci per tutti i servizi e i processi gestiti da `systemd`.</span><span class="sxs-lookup"><span data-stu-id="8ac20-171">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="8ac20-172">Per visualizzare le voci specifiche di `kestrel-hellomvc.service`, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="8ac20-172">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="8ac20-173">Per filtrare ulteriormente, opzioni come `--since today`, `--until 1 hour ago` o una combinazione delle stesse consentono di ridurre la quantità di voci restituite.</span><span class="sxs-lookup"><span data-stu-id="8ac20-173">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="8ac20-174">Protezione dell'app</span><span class="sxs-lookup"><span data-stu-id="8ac20-174">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="8ac20-175">Abilitare AppArmor</span><span class="sxs-lookup"><span data-stu-id="8ac20-175">Enable AppArmor</span></span>

<span data-ttu-id="8ac20-176">I moduli di protezione Linux (LSM) è un framework che fa parte del kernel Linux dal 2.6 Linux.</span><span class="sxs-lookup"><span data-stu-id="8ac20-176">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="8ac20-177">LSM supporta diverse implementazioni di moduli di protezione.</span><span class="sxs-lookup"><span data-stu-id="8ac20-177">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="8ac20-178">[AppArmor](https://wiki.ubuntu.com/AppArmor) è un modulo LSM che implementa un sistema di controllo di accesso obbligatorio che consente di circoscrivere il programma a un set limitato di risorse.</span><span class="sxs-lookup"><span data-stu-id="8ac20-178">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="8ac20-179">Verificare che AppArmor sia abilitato e configurato correttamente.</span><span class="sxs-lookup"><span data-stu-id="8ac20-179">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="8ac20-180">Configurazione del firewall</span><span class="sxs-lookup"><span data-stu-id="8ac20-180">Configuring the firewall</span></span>

<span data-ttu-id="8ac20-181">Chiudere tutte le porte esterne che non sono in uso.</span><span class="sxs-lookup"><span data-stu-id="8ac20-181">Close off all external ports that are not in use.</span></span> <span data-ttu-id="8ac20-182">Il firewall UFW (Uncomplicated Firewall) offre un front-end per `iptables` attraverso un'interfaccia della riga di comando per la configurazione del firewall.</span><span class="sxs-lookup"><span data-stu-id="8ac20-182">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="8ac20-183">Verificare che `ufw` è configurato per consentire il traffico su tutte le porte necessita.</span><span class="sxs-lookup"><span data-stu-id="8ac20-183">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="8ac20-184">Protezione di Nginx</span><span class="sxs-lookup"><span data-stu-id="8ac20-184">Securing Nginx</span></span>

<span data-ttu-id="8ac20-185">La distribuzione predefinita di Nginx non abilita SSL.</span><span class="sxs-lookup"><span data-stu-id="8ac20-185">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="8ac20-186">Per abilitare ulteriori funzionalità di sicurezza, compilarle dal codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="8ac20-186">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="8ac20-187">Scaricare il codice sorgente e installare le dipendenze di compilazione</span><span class="sxs-lookup"><span data-stu-id="8ac20-187">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="8ac20-188">Modificare il nome di risposta Nginx</span><span class="sxs-lookup"><span data-stu-id="8ac20-188">Change the Nginx response name</span></span>

<span data-ttu-id="8ac20-189">Modificare *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="8ac20-189">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="8ac20-190">Configurare le opzioni e compilare</span><span class="sxs-lookup"><span data-stu-id="8ac20-190">Configure the options and build</span></span>

<span data-ttu-id="8ac20-191">La libreria PCRE è obbligatoria per le espressioni regolari.</span><span class="sxs-lookup"><span data-stu-id="8ac20-191">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="8ac20-192">Le espressioni regolari sono usate nella direttiva ubicazione per ngx_http_rewrite_module.</span><span class="sxs-lookup"><span data-stu-id="8ac20-192">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="8ac20-193">http_ssl_module aggiunge il supporto del protocollo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8ac20-193">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="8ac20-194">È consigliabile utilizzare un firewall, app web come *ModSecurity* per finalizzare l'app.</span><span class="sxs-lookup"><span data-stu-id="8ac20-194">Consider using a web app firewall like *ModSecurity* to harden the app.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="8ac20-195">Configurare SSL</span><span class="sxs-lookup"><span data-stu-id="8ac20-195">Configure SSL</span></span>

* <span data-ttu-id="8ac20-196">Configurare il server per ascoltare il traffico HTTPS sulla porta `443` specificando un certificato valido emesso da un'autorità di certificati attendibili (CA).</span><span class="sxs-lookup"><span data-stu-id="8ac20-196">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="8ac20-197">Rafforzare la sicurezza mediante l'utilizzo di alcune delle procedure di cui è illustrate nell'esempio seguente */etc/nginx/nginx.conf* file.</span><span class="sxs-lookup"><span data-stu-id="8ac20-197">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="8ac20-198">Gli esempi includono la scelta di una crittografia più complessa e il reindirizzamento di tutto il traffico su HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8ac20-198">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="8ac20-199">L'aggiunta di un'intestazione `HTTP Strict-Transport-Security` (HSTS) garantisce che tutte le richieste successive vengano effettuate dal esclusivamente via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8ac20-199">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="8ac20-200">Non aggiungere l'intestazione di sicurezza-trasporto-Strict o scelto un appropriato `max-age` se SSL verrà disabilitata in futuro.</span><span class="sxs-lookup"><span data-stu-id="8ac20-200">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="8ac20-201">Aggiungere il file di configurazione */etc/nginx/proxy.conf*:</span><span class="sxs-lookup"><span data-stu-id="8ac20-201">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[Main](linux-nginx/proxy.conf)]

<span data-ttu-id="8ac20-202">Aggiungere il file di configurazione */etc/nginx/proxy.conf*.</span><span class="sxs-lookup"><span data-stu-id="8ac20-202">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="8ac20-203">L'esempio contiene entrambe le sezioni `http` e `server` in un unico file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="8ac20-203">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[Main](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="8ac20-204">Proteggere Nginx dal clickjacking</span><span class="sxs-lookup"><span data-stu-id="8ac20-204">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="8ac20-205">Il clickjacking è una tecnica fraudolenta con cui i clic di un utente vengono "catturati"</span><span class="sxs-lookup"><span data-stu-id="8ac20-205">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="8ac20-206">e reindirizzati a un oggetto in un sito infetto.</span><span class="sxs-lookup"><span data-stu-id="8ac20-206">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="8ac20-207">Utilizzare X-FRAME-OPTIONS per proteggere il sito.</span><span class="sxs-lookup"><span data-stu-id="8ac20-207">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="8ac20-208">Modificare il file *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="8ac20-208">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="8ac20-209">Aggiungere la riga `add_header X-Frame-Options "SAMEORIGIN";` e salvare il file, quindi riavviare Nginx.</span><span class="sxs-lookup"><span data-stu-id="8ac20-209">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="8ac20-210">Analisi del tipo MIME</span><span class="sxs-lookup"><span data-stu-id="8ac20-210">MIME-type sniffing</span></span>

<span data-ttu-id="8ac20-211">Questa intestazione impedisce alla maggior parte dei browser di dedurre dall'analisi del tipo MIME un tipo di contenuto diverso da quello dichiarato, poiché l'intestazione indica al browser di non eseguire l'override del tipo di contenuto della risposta.</span><span class="sxs-lookup"><span data-stu-id="8ac20-211">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="8ac20-212">Con l'opzione `nosniff`, se il server indica che il contenuto è "text/html", il browser ne esegue il rendering come "text/html".</span><span class="sxs-lookup"><span data-stu-id="8ac20-212">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="8ac20-213">Modificare il file *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="8ac20-213">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="8ac20-214">Aggiungere la riga `add_header X-Content-Type-Options "nosniff";` e salvare il file, quindi riavviare Nginx.</span><span class="sxs-lookup"><span data-stu-id="8ac20-214">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
