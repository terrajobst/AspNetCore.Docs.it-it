---
title: Hosting di ASP.NET Core in Linux con Nginx
author: rick-anderson
description: Viene descritto come configurare Nginx come un proxy inverso in Ubuntu 16.04 per inoltrare il traffico HTTP a un'app web ASP.NET Core in esecuzione su Kestrel.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 08/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 9939e420fee41b11e709da911d4051a048e789b3
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="c935a-103">Hosting di ASP.NET Core in Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="c935a-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="c935a-104">Di [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="c935a-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="c935a-105">Questa guida spiega come configurare un ambiente ASP.NET Core pronto per la produzione in un server Ubuntu 16.04.</span><span class="sxs-lookup"><span data-stu-id="c935a-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

<span data-ttu-id="c935a-106">**Nota:** per Ubuntu 14.04, *supervisord* è consigliabile come soluzione per il monitoraggio del processo Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c935a-106">**Note:** For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="c935a-107">*systemd* non è disponibile in Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="c935a-107">*systemd* isn't available on Ubuntu 14.04.</span></span> [<span data-ttu-id="c935a-108">Vedere la versione precedente di questo documento</span><span class="sxs-lookup"><span data-stu-id="c935a-108">See previous version of this document</span></span>](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

<span data-ttu-id="c935a-109">In questa guida:</span><span class="sxs-lookup"><span data-stu-id="c935a-109">This guide:</span></span>

* <span data-ttu-id="c935a-110">Inserisce un'app di ASP.NET Core esistente con un server proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="c935a-110">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="c935a-111">Imposta backup del server proxy inverso per inoltrare le richieste al server web Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c935a-111">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="c935a-112">Assicura che l'app web viene eseguito all'avvio come daemon.</span><span class="sxs-lookup"><span data-stu-id="c935a-112">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="c935a-113">Consente di configurare uno strumento di gestione del processo per riavviare l'app web.</span><span class="sxs-lookup"><span data-stu-id="c935a-113">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c935a-114">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="c935a-114">Prerequisites</span></span>

1. <span data-ttu-id="c935a-115">Accesso a un server Ubuntu 16.04 con un account utente standard con privilegio sudo</span><span class="sxs-lookup"><span data-stu-id="c935a-115">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
1. <span data-ttu-id="c935a-116">Un'app di ASP.NET Core esistente</span><span class="sxs-lookup"><span data-stu-id="c935a-116">An existing ASP.NET Core app</span></span>

## <a name="copy-over-the-app"></a><span data-ttu-id="c935a-117">Copiare l'app</span><span class="sxs-lookup"><span data-stu-id="c935a-117">Copy over the app</span></span>

<span data-ttu-id="c935a-118">Eseguire `dotnet publish` dall'ambiente di sviluppo per creare un pacchetto per un'applicazione in una directory indipendente che può essere eseguita sul server.</span><span class="sxs-lookup"><span data-stu-id="c935a-118">Run `dotnet publish` from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="c935a-119">Copia l'app ASP.NET Core per il server utilizzando qualsiasi strumento integra nel flusso di lavoro dell'organizzazione (ad esempio, SCP, FTP).</span><span class="sxs-lookup"><span data-stu-id="c935a-119">Copy the ASP.NET Core app to the server using whatever tool integrates into the organization's workflow (for example, SCP, FTP).</span></span> <span data-ttu-id="c935a-120">Testare l'applicazione, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c935a-120">Test the app, for example:</span></span>

* <span data-ttu-id="c935a-121">Dalla riga di comando, eseguire `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="c935a-121">From the command line, run `dotnet <app_assembly>.dll`.</span></span>
* <span data-ttu-id="c935a-122">In un browser passare a `http://<serveraddress>:<port>` per verificare se l'applicazione funziona in Linux.</span><span class="sxs-lookup"><span data-stu-id="c935a-122">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="c935a-123">Configurare un server proxy inverso</span><span class="sxs-lookup"><span data-stu-id="c935a-123">Configure a reverse proxy server</span></span>

<span data-ttu-id="c935a-124">Un proxy inverso è una configurazione comune per la gestione delle applicazioni web dinamiche.</span><span class="sxs-lookup"><span data-stu-id="c935a-124">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="c935a-125">Un proxy inverso termina la richiesta HTTP e lo inoltra all'applicazione ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c935a-125">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="c935a-126">Perché usare un server proxy inverso?</span><span class="sxs-lookup"><span data-stu-id="c935a-126">Why use a reverse proxy server?</span></span>

<span data-ttu-id="c935a-127">Kestrel è molto utile per disporre il contenuto dinamico da ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c935a-127">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="c935a-128">Tuttavia, le funzionalità di gestione web non sono come ricco di funzionalità come server, ad esempio IIS, Apache o Nginx.</span><span class="sxs-lookup"><span data-stu-id="c935a-128">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="c935a-129">Un server proxy inverso può scaricare il lavoro, ad esempio del contenuto statico, la memorizzazione nella cache le richieste, la compressione di richieste e la terminazione SSL dal server HTTP.</span><span class="sxs-lookup"><span data-stu-id="c935a-129">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="c935a-130">Un server proxy inverso può risiedere in un computer dedicato o essere distribuito insieme a un server HTTP.</span><span class="sxs-lookup"><span data-stu-id="c935a-130">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="c935a-131">Ai fini di questa guida viene usata una singola istanza di Nginx.</span><span class="sxs-lookup"><span data-stu-id="c935a-131">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="c935a-132">Viene eseguito sullo stesso server, insieme al server HTTP.</span><span class="sxs-lookup"><span data-stu-id="c935a-132">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="c935a-133">In base ai requisiti, un'impostazione diversa potrebbe essere scelto.</span><span class="sxs-lookup"><span data-stu-id="c935a-133">Based on requirements, a different setup may be choosen.</span></span>

<span data-ttu-id="c935a-134">Poiché le richieste vengono inoltrate dal proxy inverso, usare il middleware `ForwardedHeaders` dal pacchetto `Microsoft.AspNetCore.HttpOverrides`.</span><span class="sxs-lookup"><span data-stu-id="c935a-134">Because requests are forwarded by reverse proxy, use the `ForwardedHeaders` middleware from the `Microsoft.AspNetCore.HttpOverrides` package.</span></span> <span data-ttu-id="c935a-135">Questo middleware aggiorna `Request.Scheme` usando l'intestazione `X-Forwarded-Proto`, in modo che gli URI di reindirizzamento e altri criteri di sicurezza funzionino correttamente.</span><span class="sxs-lookup"><span data-stu-id="c935a-135">This middleware updates `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="c935a-136">Quando si configura un server proxy inverso, il middleware di autenticazione richiede `UseForwardedHeaders` per essere eseguito per primo.</span><span class="sxs-lookup"><span data-stu-id="c935a-136">When setting up a reverse proxy server, the authentication middleware needs `UseForwardedHeaders` to run first.</span></span> <span data-ttu-id="c935a-137">Questo ordine garantisce che il middleware di autenticazione sia in grado di usare i valori interessati e di generare gli URI di reindirizzamento corretti.</span><span class="sxs-lookup"><span data-stu-id="c935a-137">This ordering ensures that the authentication middleware can consume the affected values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c935a-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c935a-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c935a-139">Richiamare il metodo `UseForwardedHeaders` (nel metodo `Configure` di *Startup.cs*) prima di chiamare `UseAuthentication` o un middleware con schema di autenticazione simile:</span><span class="sxs-lookup"><span data-stu-id="c935a-139">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseAuthentication` or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c935a-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c935a-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c935a-141">Richiamare il metodo `UseForwardedHeaders` (nel metodo `Configure` di *Startup.cs*) prima di chiamare `UseIdentity` e `UseFacebookAuthentication` o un middleware con schema di autenticazione simile:</span><span class="sxs-lookup"><span data-stu-id="c935a-141">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseIdentity` and `UseFacebookAuthentication` or similar authentication scheme middleware:</span></span>

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

### <a name="install-nginx"></a><span data-ttu-id="c935a-142">Installare Nginx</span><span class="sxs-lookup"><span data-stu-id="c935a-142">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="c935a-143">Se non saranno installati i moduli Nginx facoltativi, potrebbe essere necessario compilazione Nginx dall'origine.</span><span class="sxs-lookup"><span data-stu-id="c935a-143">If optional Nginx modules will be installed, building Nginx from source might be required.</span></span>

<span data-ttu-id="c935a-144">Usare `apt-get` per installare Nginx.</span><span class="sxs-lookup"><span data-stu-id="c935a-144">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="c935a-145">Il programma di installazione crea uno script di inizializzazione System V che esegue Nginx come daemon all'avvio del sistema.</span><span class="sxs-lookup"><span data-stu-id="c935a-145">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="c935a-146">Poiché Nginx è stato installato per la prima volta, avviarlo in modo esplicito eseguendo:</span><span class="sxs-lookup"><span data-stu-id="c935a-146">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="c935a-147">Verificare che un browser visualizzi la pagina di destinazione predefinita per Nginx.</span><span class="sxs-lookup"><span data-stu-id="c935a-147">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="c935a-148">Configurare Nginx</span><span class="sxs-lookup"><span data-stu-id="c935a-148">Configure Nginx</span></span>

<span data-ttu-id="c935a-149">Per configurare come un proxy inverso per inoltrare le richieste per l'app ASP.NET Core Nginx modificare `/etc/nginx/sites-available/default`.</span><span class="sxs-lookup"><span data-stu-id="c935a-149">To configure Nginx as a reverse proxy to forward requests to our ASP.NET Core app, modify `/etc/nginx/sites-available/default`.</span></span> <span data-ttu-id="c935a-150">Aprirlo in un editor di testo e sostituire il contenuto con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="c935a-150">Open it in a text editor, and replace the contents with the following:</span></span>

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

<span data-ttu-id="c935a-151">Questo file di configurazione Nginx inoltra il traffico pubblico in ingresso dalla porta `80` alla porta `5000`.</span><span class="sxs-lookup"><span data-stu-id="c935a-151">This Nginx configuration file forwards incoming public traffic from port `80` to port `5000`.</span></span>

<span data-ttu-id="c935a-152">Una volta stabilita la configurazione di Nginx, eseguire `sudo nginx -t` per verificare la sintassi dei file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="c935a-152">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="c935a-153">Se il test di file di configurazione ha esito positivo, forzare Nginx per rendere effettive le modifiche eseguendo `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="c935a-153">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="c935a-154">Monitoraggio dell'app</span><span class="sxs-lookup"><span data-stu-id="c935a-154">Monitoring the app</span></span>

<span data-ttu-id="c935a-155">Il server è configurato per inoltrare le richieste effettuate a `http://<serveraddress>:80` al ASP.NET Core in esecuzione l'app in Kestrel in `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="c935a-155">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="c935a-156">Tuttavia, Nginx perché non è configurato per gestire il processo Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c935a-156">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="c935a-157">*systemd* può essere utilizzato per creare un file del servizio per avviare e monitorare l'app web sottostante.</span><span class="sxs-lookup"><span data-stu-id="c935a-157">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="c935a-158">*systemd* è un sistema di inizializzazione che offre molte funzionalità potenti per l'avvio, l'arresto e la gestione dei processi.</span><span class="sxs-lookup"><span data-stu-id="c935a-158">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="c935a-159">Creare il file del servizio</span><span class="sxs-lookup"><span data-stu-id="c935a-159">Create the service file</span></span>

<span data-ttu-id="c935a-160">Creare il file di definizione del servizio:</span><span class="sxs-lookup"><span data-stu-id="c935a-160">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="c935a-161">Di seguito è un esempio di file per l'applicazione del servizio:</span><span class="sxs-lookup"><span data-stu-id="c935a-161">The following is an example service file for the app:</span></span>

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

<span data-ttu-id="c935a-162">**Nota:** se l'utente *www dati* non viene usata dalla configurazione, è necessario creare innanzitutto l'utente definito qui e ha le proprietà appropriate per i file.</span><span class="sxs-lookup"><span data-stu-id="c935a-162">**Note:** If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>
<span data-ttu-id="c935a-163">**Nota:** Linux è un sistema di file tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="c935a-163">**Note:** Linux has a case-sensitive file system.</span></span> <span data-ttu-id="c935a-164">L'impostazione di ASPNETCORE_ENVIRONMENT su "Produzione" produce la ricerca del file di configurazione *appsettings. Production.JSON*, non *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="c935a-164">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

<span data-ttu-id="c935a-165">Salvare il file e abilitare il servizio.</span><span class="sxs-lookup"><span data-stu-id="c935a-165">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="c935a-166">Avviare il servizio e verificare che sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="c935a-166">Start the service and verify that it's running.</span></span>

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

<span data-ttu-id="c935a-167">Con il proxy inverso configurate e gestite tramite systemd Kestrel, l'app web è completamente configurato ed è possibile accedervi da un browser del computer locale in `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="c935a-167">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="c935a-168">È inoltre possibile accedere da un computer remoto, impedendo qualsiasi firewall che potrebbe essere bloccato.</span><span class="sxs-lookup"><span data-stu-id="c935a-168">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="c935a-169">Esaminare le intestazioni di risposta, il `Server` intestazione Mostra l'app ASP.NET Core servito dal Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c935a-169">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="c935a-170">Vista dei log</span><span class="sxs-lookup"><span data-stu-id="c935a-170">Viewing logs</span></span>

<span data-ttu-id="c935a-171">Poiché l'app web utilizzando Kestrel viene gestita mediante `systemd`, tutti gli eventi e i processi vengono registrati in una registrazione centralizzata.</span><span class="sxs-lookup"><span data-stu-id="c935a-171">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="c935a-172">Tuttavia, questo giornale include tutte le voci per tutti i servizi e i processi gestiti da `systemd`.</span><span class="sxs-lookup"><span data-stu-id="c935a-172">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="c935a-173">Per visualizzare le voci specifiche di `kestrel-hellomvc.service`, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c935a-173">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="c935a-174">Per filtrare ulteriormente, opzioni come `--since today`, `--until 1 hour ago` o una combinazione delle stesse consentono di ridurre la quantità di voci restituite.</span><span class="sxs-lookup"><span data-stu-id="c935a-174">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="c935a-175">Protezione dell'app</span><span class="sxs-lookup"><span data-stu-id="c935a-175">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="c935a-176">Abilitare AppArmor</span><span class="sxs-lookup"><span data-stu-id="c935a-176">Enable AppArmor</span></span>

<span data-ttu-id="c935a-177">I moduli di protezione Linux (LSM) è un framework che fa parte del kernel Linux dal 2.6 Linux.</span><span class="sxs-lookup"><span data-stu-id="c935a-177">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="c935a-178">LSM supporta diverse implementazioni di moduli di protezione.</span><span class="sxs-lookup"><span data-stu-id="c935a-178">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="c935a-179">[AppArmor](https://wiki.ubuntu.com/AppArmor) è un modulo LSM che implementa un sistema di controllo di accesso obbligatorio che consente di circoscrivere il programma a un set limitato di risorse.</span><span class="sxs-lookup"><span data-stu-id="c935a-179">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="c935a-180">Verificare che AppArmor sia abilitato e configurato correttamente.</span><span class="sxs-lookup"><span data-stu-id="c935a-180">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="c935a-181">Configurazione del firewall</span><span class="sxs-lookup"><span data-stu-id="c935a-181">Configuring the firewall</span></span>

<span data-ttu-id="c935a-182">Chiudere tutte le porte esterne che non sono in uso.</span><span class="sxs-lookup"><span data-stu-id="c935a-182">Close off all external ports that are not in use.</span></span> <span data-ttu-id="c935a-183">Il firewall UFW (Uncomplicated Firewall) offre un front-end per `iptables` attraverso un'interfaccia della riga di comando per la configurazione del firewall.</span><span class="sxs-lookup"><span data-stu-id="c935a-183">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="c935a-184">Verificare che `ufw` è configurato per consentire il traffico su tutte le porte necessita.</span><span class="sxs-lookup"><span data-stu-id="c935a-184">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="c935a-185">Protezione di Nginx</span><span class="sxs-lookup"><span data-stu-id="c935a-185">Securing Nginx</span></span>

<span data-ttu-id="c935a-186">La distribuzione predefinita di Nginx non abilita SSL.</span><span class="sxs-lookup"><span data-stu-id="c935a-186">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="c935a-187">Per abilitare ulteriori funzionalità di sicurezza, compilarle dal codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="c935a-187">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="c935a-188">Scaricare il codice sorgente e installare le dipendenze di compilazione</span><span class="sxs-lookup"><span data-stu-id="c935a-188">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="c935a-189">Modificare il nome di risposta Nginx</span><span class="sxs-lookup"><span data-stu-id="c935a-189">Change the Nginx response name</span></span>

<span data-ttu-id="c935a-190">Modificare *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="c935a-190">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="c935a-191">Configurare le opzioni e compilare</span><span class="sxs-lookup"><span data-stu-id="c935a-191">Configure the options and build</span></span>

<span data-ttu-id="c935a-192">La libreria PCRE è obbligatoria per le espressioni regolari.</span><span class="sxs-lookup"><span data-stu-id="c935a-192">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="c935a-193">Le espressioni regolari sono usate nella direttiva ubicazione per ngx_http_rewrite_module.</span><span class="sxs-lookup"><span data-stu-id="c935a-193">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="c935a-194">http_ssl_module aggiunge il supporto del protocollo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c935a-194">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="c935a-195">È consigliabile utilizzare un firewall, app web come *ModSecurity* per finalizzare l'app.</span><span class="sxs-lookup"><span data-stu-id="c935a-195">Consider using a web app firewall like *ModSecurity* to harden the app.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="c935a-196">Configurare SSL</span><span class="sxs-lookup"><span data-stu-id="c935a-196">Configure SSL</span></span>

* <span data-ttu-id="c935a-197">Configurare il server per ascoltare il traffico HTTPS sulla porta `443` specificando un certificato valido emesso da un'autorità di certificati attendibili (CA).</span><span class="sxs-lookup"><span data-stu-id="c935a-197">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="c935a-198">Rafforzare la sicurezza mediante l'utilizzo di alcune delle procedure di cui è illustrate nell'esempio seguente */etc/nginx/nginx.conf* file.</span><span class="sxs-lookup"><span data-stu-id="c935a-198">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="c935a-199">Gli esempi includono la scelta di una crittografia più complessa e il reindirizzamento di tutto il traffico su HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c935a-199">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="c935a-200">L'aggiunta di un'intestazione `HTTP Strict-Transport-Security` (HSTS) garantisce che tutte le richieste successive vengano effettuate dal esclusivamente via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c935a-200">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="c935a-201">Non aggiungere l'intestazione di sicurezza-trasporto-Strict o scelto un appropriato `max-age` se SSL verrà disabilitata in futuro.</span><span class="sxs-lookup"><span data-stu-id="c935a-201">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="c935a-202">Aggiungere il file di configurazione */etc/nginx/proxy.conf*:</span><span class="sxs-lookup"><span data-stu-id="c935a-202">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[Main](linux-nginx/proxy.conf)]

<span data-ttu-id="c935a-203">Aggiungere il file di configurazione */etc/nginx/proxy.conf*.</span><span class="sxs-lookup"><span data-stu-id="c935a-203">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="c935a-204">L'esempio contiene entrambe le sezioni `http` e `server` in un unico file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="c935a-204">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[Main](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="c935a-205">Proteggere Nginx dal clickjacking</span><span class="sxs-lookup"><span data-stu-id="c935a-205">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="c935a-206">Il clickjacking è una tecnica fraudolenta con cui i clic di un utente vengono "catturati"</span><span class="sxs-lookup"><span data-stu-id="c935a-206">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="c935a-207">e reindirizzati a un oggetto in un sito infetto.</span><span class="sxs-lookup"><span data-stu-id="c935a-207">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="c935a-208">Utilizzare X-FRAME-OPTIONS per proteggere il sito.</span><span class="sxs-lookup"><span data-stu-id="c935a-208">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="c935a-209">Modificare il file *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="c935a-209">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="c935a-210">Aggiungere la riga `add_header X-Frame-Options "SAMEORIGIN";` e salvare il file, quindi riavviare Nginx.</span><span class="sxs-lookup"><span data-stu-id="c935a-210">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="c935a-211">Analisi del tipo MIME</span><span class="sxs-lookup"><span data-stu-id="c935a-211">MIME-type sniffing</span></span>

<span data-ttu-id="c935a-212">Questa intestazione impedisce alla maggior parte dei browser di dedurre dall'analisi del tipo MIME un tipo di contenuto diverso da quello dichiarato, poiché l'intestazione indica al browser di non eseguire l'override del tipo di contenuto della risposta.</span><span class="sxs-lookup"><span data-stu-id="c935a-212">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="c935a-213">Con l'opzione `nosniff`, se il server indica che il contenuto è "text/html", il browser ne esegue il rendering come "text/html".</span><span class="sxs-lookup"><span data-stu-id="c935a-213">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="c935a-214">Modificare il file *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="c935a-214">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="c935a-215">Aggiungere la riga `add_header X-Content-Type-Options "nosniff";` e salvare il file, quindi riavviare Nginx.</span><span class="sxs-lookup"><span data-stu-id="c935a-215">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
