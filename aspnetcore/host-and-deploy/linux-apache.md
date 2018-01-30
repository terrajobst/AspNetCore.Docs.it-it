---
title: Hosting di ASP.NET Core in Linux con Apache
description: Informazioni su come configurare come server proxy inverso CentOS Apache reindirizzare il traffico HTTP a un'app web ASP.NET Core in esecuzione su Kestrel.
author: spboyer
manager: wpickett
ms.author: spboyer
ms.custom: mvc
ms.date: 10/19/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-apache
ms.openlocfilehash: aa55ecd6dc8169e0e77b3899389ec924b1e1ae4a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="5cab2-103">Hosting di ASP.NET Core in Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="5cab2-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="5cab2-104">Di [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="5cab2-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="5cab2-105">Utilizzando questa Guida, informazioni su come configurare [Apache](https://httpd.apache.org/) come server proxy inverso [CentOS 7](https://www.centos.org/) reindirizzare il traffico HTTP a un'app web ASP.NET Core in esecuzione in [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="5cab2-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="5cab2-106">Il [mod_proxy estensione](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) e moduli correlati creare proxy inverso del server.</span><span class="sxs-lookup"><span data-stu-id="5cab2-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5cab2-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="5cab2-107">Prerequisites</span></span>

1. <span data-ttu-id="5cab2-108">Server che esegue CentOS 7 con un account utente standard con privilegi sudo</span><span class="sxs-lookup"><span data-stu-id="5cab2-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="5cab2-109">App ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5cab2-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="5cab2-110">Pubblicare l'app</span><span class="sxs-lookup"><span data-stu-id="5cab2-110">Publish the app</span></span>

<span data-ttu-id="5cab2-111">Pubblicare l'app come un [distribuzione indipendente](/dotnet/core/deploying/#self-contained-deployments-scd) nella configurazione di rilascio per il runtime CentOS 7 (`centos.7-x64`).</span><span class="sxs-lookup"><span data-stu-id="5cab2-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="5cab2-112">Copiare il contenuto del *bin/Release/netcoreapp2.0/centos.7-x64/publish* cartella al server tramite SCP, FTP o altro metodo di trasferimento di file.</span><span class="sxs-lookup"><span data-stu-id="5cab2-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="5cab2-113">In uno scenario di distribuzione di produzione, un flusso di lavoro di integrazione continua esegue le operazioni di pubblicazione dell'app e copiare le risorse nel server di.</span><span class="sxs-lookup"><span data-stu-id="5cab2-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="5cab2-114">Configurazione di un server proxy</span><span class="sxs-lookup"><span data-stu-id="5cab2-114">Configure a proxy server</span></span>

<span data-ttu-id="5cab2-115">Un proxy inverso è una configurazione comune per la gestione delle applicazioni web dinamiche.</span><span class="sxs-lookup"><span data-stu-id="5cab2-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="5cab2-116">Il proxy inverso termina la richiesta HTTP e lo inoltra all'applicazione ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5cab2-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="5cab2-117">Un server proxy inoltra le richieste del client a un altro server anziché evaderle da solo.</span><span class="sxs-lookup"><span data-stu-id="5cab2-117">A proxy server is one which forwards client requests to another server instead of fulfilling them itself.</span></span> <span data-ttu-id="5cab2-118">Un proxy inverso inoltra a una destinazione fissa, in genere per conto di client non autorizzati.</span><span class="sxs-lookup"><span data-stu-id="5cab2-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="5cab2-119">In questa guida Apache è configurato come proxy inverso in esecuzione nello stesso server che gestisce l'applicazione ASP.NET Core Kestrel.</span><span class="sxs-lookup"><span data-stu-id="5cab2-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

### <a name="install-apache"></a><span data-ttu-id="5cab2-120">Installare Apache</span><span class="sxs-lookup"><span data-stu-id="5cab2-120">Install Apache</span></span>

<span data-ttu-id="5cab2-121">CentOS i pacchetti di aggiornamento alle versioni stabili più recenti:</span><span class="sxs-lookup"><span data-stu-id="5cab2-121">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="5cab2-122">Installare il server web Apache in CentOS con un singolo `yum` comando:</span><span class="sxs-lookup"><span data-stu-id="5cab2-122">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="5cab2-123">Esempio di output dopo l'esecuzione del comando:</span><span class="sxs-lookup"><span data-stu-id="5cab2-123">Sample output after running the command:</span></span>

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
> <span data-ttu-id="5cab2-124">In questo esempio, l'output rispecchi httpd.86_64 poiché la versione CentOS 7 a 64 bit.</span><span class="sxs-lookup"><span data-stu-id="5cab2-124">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="5cab2-125">Per verificare dove è installato Apache, eseguire `whereis httpd` da un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="5cab2-125">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span> 

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="5cab2-126">Configurare Apache per il proxy inverso</span><span class="sxs-lookup"><span data-stu-id="5cab2-126">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="5cab2-127">I file di configurazione per Apache si trovano all'interno della directory `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="5cab2-127">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="5cab2-128">Qualsiasi file con il *.conf* estensione viene elaborata in ordine alfabetico oltre i file di configurazione del modulo in `/etc/httpd/conf.modules.d/`, che contiene tutte le configurazioni di file necessarie per caricare i moduli.</span><span class="sxs-lookup"><span data-stu-id="5cab2-128">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="5cab2-129">Creare un file di configurazione per l'applicazione denominata `hellomvc.conf`:</span><span class="sxs-lookup"><span data-stu-id="5cab2-129">Create a configuration file for the app named `hellomvc.conf`:</span></span>

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="5cab2-130">Il **VirtualHost** può essere visualizzato più volte in uno o più file in un server.</span><span class="sxs-lookup"><span data-stu-id="5cab2-130">The **VirtualHost** node can appear multiple times in one or more files on a server.</span></span> <span data-ttu-id="5cab2-131">**VirtualHost** è impostato per l'ascolto su qualsiasi indirizzo IP tramite la porta 80.</span><span class="sxs-lookup"><span data-stu-id="5cab2-131">**VirtualHost** is set to listen on any IP address using port 80.</span></span> <span data-ttu-id="5cab2-132">Le due righe successive vengono impostate su richieste proxy nella directory principale nel server a 127.0.0.1 sulla porta 5000.</span><span class="sxs-lookup"><span data-stu-id="5cab2-132">The next two lines are set to proxy requests at the root to the server at 127.0.0.1 on port 5000.</span></span> <span data-ttu-id="5cab2-133">Per la comunicazione bidirezionale, *ProxyPass* e *ProxyPassReverse* sono necessari.</span><span class="sxs-lookup"><span data-stu-id="5cab2-133">For bi-directional communication, *ProxyPass* and *ProxyPassReverse* are required.</span></span>

<span data-ttu-id="5cab2-134">Registrazione può essere configurata **VirtualHost** utilizzando **ErrorLog** e **CustomLog** direttive.</span><span class="sxs-lookup"><span data-stu-id="5cab2-134">Logging can be configured per **VirtualHost** using **ErrorLog** and **CustomLog** directives.</span></span> <span data-ttu-id="5cab2-135">**Log degli errori** è il percorso in cui i registri del server, errori e **CustomLog** imposta il nome e il formato del file di log.</span><span class="sxs-lookup"><span data-stu-id="5cab2-135">**ErrorLog** is the location where the server logs errors, and **CustomLog** sets the filename and format of log file.</span></span> <span data-ttu-id="5cab2-136">In questo caso, si tratta in cui vengono registrate informazioni di richiesta.</span><span class="sxs-lookup"><span data-stu-id="5cab2-136">In this case, this is where request information is logged.</span></span> <span data-ttu-id="5cab2-137">È presente una riga per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="5cab2-137">There's one line for each request.</span></span>

<span data-ttu-id="5cab2-138">Salvare il file e il test della configurazione.</span><span class="sxs-lookup"><span data-stu-id="5cab2-138">Save the file and test the configuration.</span></span> <span data-ttu-id="5cab2-139">Se tutto ciò viene superato, la risposta deve essere `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="5cab2-139">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="5cab2-140">Riavviare Apache:</span><span class="sxs-lookup"><span data-stu-id="5cab2-140">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="5cab2-141">Monitoraggio dell'app</span><span class="sxs-lookup"><span data-stu-id="5cab2-141">Monitoring the app</span></span>

<span data-ttu-id="5cab2-142">Apache è ora configurato per inoltrare le richieste effettuate a `http://localhost:80` all'app ASP.NET Core in esecuzione in Kestrel in `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="5cab2-142">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="5cab2-143">Tuttavia, Apache perché non è configurato per gestire il processo Kestrel.</span><span class="sxs-lookup"><span data-stu-id="5cab2-143">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="5cab2-144">Utilizzare *systemd* e creare un file del servizio per avviare e monitorare l'app web sottostante.</span><span class="sxs-lookup"><span data-stu-id="5cab2-144">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="5cab2-145">*systemd* è un sistema di inizializzazione che offre molte funzionalità potenti per l'avvio, l'arresto e la gestione dei processi.</span><span class="sxs-lookup"><span data-stu-id="5cab2-145">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="5cab2-146">Creare il file del servizio</span><span class="sxs-lookup"><span data-stu-id="5cab2-146">Create the service file</span></span>

<span data-ttu-id="5cab2-147">Creare il file di definizione del servizio:</span><span class="sxs-lookup"><span data-stu-id="5cab2-147">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="5cab2-148">Un file del servizio di esempio per l'app:</span><span class="sxs-lookup"><span data-stu-id="5cab2-148">An example service file for the app:</span></span>

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
> <span data-ttu-id="5cab2-149">**Utente** &mdash; se l'utente *apache* non viene usata dalla configurazione, è necessario creare prima l'utente e specificato le proprietà appropriate per i file.</span><span class="sxs-lookup"><span data-stu-id="5cab2-149">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="5cab2-150">Salvare il file e attivare il servizio:</span><span class="sxs-lookup"><span data-stu-id="5cab2-150">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="5cab2-151">Avviare il servizio e verificare che sia in esecuzione:</span><span class="sxs-lookup"><span data-stu-id="5cab2-151">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="5cab2-152">Con il proxy inverso configurato e gestite tramite Kestrel *systemd*, l'app web è completamente configurato ed è possibile accedervi da un browser del computer locale in `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="5cab2-152">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="5cab2-153">Esaminare le intestazioni di risposta, il **Server** intestazione indica che l'applicazione ASP.NET Core viene gestita da Kestrel:</span><span class="sxs-lookup"><span data-stu-id="5cab2-153">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="5cab2-154">Vista dei log</span><span class="sxs-lookup"><span data-stu-id="5cab2-154">Viewing logs</span></span>

<span data-ttu-id="5cab2-155">Poiché l'app web utilizzando Kestrel viene gestita mediante *systemd*, processi e gli eventi vengono registrati in una registrazione centralizzata.</span><span class="sxs-lookup"><span data-stu-id="5cab2-155">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="5cab2-156">Tuttavia, queste registrazioni includono voci per tutti i servizi e processi gestiti da *systemd*.</span><span class="sxs-lookup"><span data-stu-id="5cab2-156">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="5cab2-157">Per visualizzare le voci specifiche di `kestrel-hellomvc.service`, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="5cab2-157">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="5cab2-158">Per il filtro di tempo, specificare opzioni della fase con il comando.</span><span class="sxs-lookup"><span data-stu-id="5cab2-158">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="5cab2-159">Ad esempio, utilizzare `--since today` per filtrare per il giorno corrente o `--until 1 hour ago` per visualizzare le voci dell'ora precedente.</span><span class="sxs-lookup"><span data-stu-id="5cab2-159">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="5cab2-160">Per ulteriori informazioni, vedere il [pagina man per journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="5cab2-160">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="5cab2-161">Protezione dell'app</span><span class="sxs-lookup"><span data-stu-id="5cab2-161">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="5cab2-162">Configurare firewall</span><span class="sxs-lookup"><span data-stu-id="5cab2-162">Configure firewall</span></span>

<span data-ttu-id="5cab2-163">*Firewalld* è un daemon dinamico per gestire il firewall con supporto per aree di rete.</span><span class="sxs-lookup"><span data-stu-id="5cab2-163">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="5cab2-164">Porte e il filtraggio dei pacchetti può comunque essere gestiti da iptables.</span><span class="sxs-lookup"><span data-stu-id="5cab2-164">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="5cab2-165">*Firewalld* deve essere installato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="5cab2-165">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="5cab2-166">`yum`può essere utilizzato per installare il pacchetto o verificare che sia installato.</span><span class="sxs-lookup"><span data-stu-id="5cab2-166">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="5cab2-167">Utilizzare `firewalld` aprire solo le porte necessarie per l'app.</span><span class="sxs-lookup"><span data-stu-id="5cab2-167">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="5cab2-168">In questo caso vengono usate le porte 80 e 443.</span><span class="sxs-lookup"><span data-stu-id="5cab2-168">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="5cab2-169">I seguenti comandi set in modo permanente le porte 80 e 443 per aprire:</span><span class="sxs-lookup"><span data-stu-id="5cab2-169">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="5cab2-170">Ricaricare le impostazioni del firewall.</span><span class="sxs-lookup"><span data-stu-id="5cab2-170">Reload the firewall settings.</span></span> <span data-ttu-id="5cab2-171">Controllare i servizi disponibili e le porte nell'area predefinita.</span><span class="sxs-lookup"><span data-stu-id="5cab2-171">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="5cab2-172">Sono disponibili le opzioni controllando `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="5cab2-172">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="5cab2-173">Configurazione SSL</span><span class="sxs-lookup"><span data-stu-id="5cab2-173">SSL configuration</span></span>

<span data-ttu-id="5cab2-174">Per configurare Apache per SSL, il *mod_ssl* modulo è utilizzato.</span><span class="sxs-lookup"><span data-stu-id="5cab2-174">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="5cab2-175">Quando il *httpd* modulo è stato installato, il *mod_ssl* modulo è stato anche installato.</span><span class="sxs-lookup"><span data-stu-id="5cab2-175">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="5cab2-176">Se non è stato installato, utilizzare `yum` per aggiungerlo alla configurazione.</span><span class="sxs-lookup"><span data-stu-id="5cab2-176">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="5cab2-177">Per attivare SSL, installare il `mod_rewrite` per abilitare la riscrittura URL:</span><span class="sxs-lookup"><span data-stu-id="5cab2-177">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="5cab2-178">Modificare il *hellomvc.conf* file per abilitare la riscrittura degli URL e proteggere la comunicazione sulla porta 443:</span><span class="sxs-lookup"><span data-stu-id="5cab2-178">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
> <span data-ttu-id="5cab2-179">In questo esempio utilizza un certificato generato localmente.</span><span class="sxs-lookup"><span data-stu-id="5cab2-179">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="5cab2-180">**SSLCertificateFile** deve essere il file del certificato primario per il nome di dominio.</span><span class="sxs-lookup"><span data-stu-id="5cab2-180">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="5cab2-181">**SSLCertificateKeyFile** deve essere il file di chiave generato CSR viene creato.</span><span class="sxs-lookup"><span data-stu-id="5cab2-181">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="5cab2-182">**SSLCertificateChainFile** deve essere il file di certificato intermedio (se presente) che è stato fornito dall'autorità di certificazione.</span><span class="sxs-lookup"><span data-stu-id="5cab2-182">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="5cab2-183">Salvare il file e la configurazione di test:</span><span class="sxs-lookup"><span data-stu-id="5cab2-183">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="5cab2-184">Riavviare Apache:</span><span class="sxs-lookup"><span data-stu-id="5cab2-184">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="5cab2-185">Altri suggerimenti di Apache</span><span class="sxs-lookup"><span data-stu-id="5cab2-185">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="5cab2-186">Intestazioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="5cab2-186">Additional headers</span></span>

<span data-ttu-id="5cab2-187">Per proteggere contro gli attacchi, esistono alcune intestazioni che devono essere modificate o aggiunta.</span><span class="sxs-lookup"><span data-stu-id="5cab2-187">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="5cab2-188">Verificare che il `mod_headers` modulo è installato:</span><span class="sxs-lookup"><span data-stu-id="5cab2-188">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="5cab2-189">Apache protetto da attacchi clickjacking</span><span class="sxs-lookup"><span data-stu-id="5cab2-189">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="5cab2-190">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), nota anche come un *dell'interfaccia utente ricorso attacco*, costituisce un attacco dannoso, in cui un visitatore di un sito Web sia indotto a fare clic su un collegamento o un pulsante in una pagina diversa rispetto a cui sta attualmente visitando.</span><span class="sxs-lookup"><span data-stu-id="5cab2-190">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="5cab2-191">Utilizzare `X-FRAME-OPTIONS` per proteggere il sito.</span><span class="sxs-lookup"><span data-stu-id="5cab2-191">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="5cab2-192">Modificare il *httpd. conf* file:</span><span class="sxs-lookup"><span data-stu-id="5cab2-192">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="5cab2-193">Aggiungere la riga `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="5cab2-193">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="5cab2-194">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="5cab2-194">Save the file.</span></span> <span data-ttu-id="5cab2-195">Riavviare Apache.</span><span class="sxs-lookup"><span data-stu-id="5cab2-195">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="5cab2-196">Analisi del tipo MIME</span><span class="sxs-lookup"><span data-stu-id="5cab2-196">MIME-type sniffing</span></span>

<span data-ttu-id="5cab2-197">Il `X-Content-Type-Options` intestazione impedisce di Internet Explorer a *analisi MIME* (determinazione di un file `Content-Type` dal contenuto del file).</span><span class="sxs-lookup"><span data-stu-id="5cab2-197">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determing a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="5cab2-198">Se il server imposta il `Content-Type` intestazione `text/html` con il `nosniff` set di opzioni, Internet Explorer visualizza il contenuto come `text/html` indipendentemente dal contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="5cab2-198">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="5cab2-199">Modificare il *httpd. conf* file:</span><span class="sxs-lookup"><span data-stu-id="5cab2-199">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="5cab2-200">Aggiungere la riga `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="5cab2-200">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="5cab2-201">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="5cab2-201">Save the file.</span></span> <span data-ttu-id="5cab2-202">Riavviare Apache.</span><span class="sxs-lookup"><span data-stu-id="5cab2-202">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="5cab2-203">Bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="5cab2-203">Load Balancing</span></span> 

<span data-ttu-id="5cab2-204">Questo esempio illustra come impostare e configurare Apache CentOS 7 e Kestrel nello stesso computer dell'istanza.</span><span class="sxs-lookup"><span data-stu-id="5cab2-204">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="5cab2-205">Per non disporre di un singolo punto di errore. utilizzando *mod_proxy_balancer* e la modifica di **VirtualHost** consente la gestione di più istanze delle App web dietro il server proxy di Apache.</span><span class="sxs-lookup"><span data-stu-id="5cab2-205">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing mutliple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="5cab2-206">Nel file di configurazione illustrato di seguito, un'istanza aggiuntiva del `hellomvc` app è configurato per l'esecuzione sulla porta 5001.</span><span class="sxs-lookup"><span data-stu-id="5cab2-206">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="5cab2-207">Il *Proxy* sezione è impostata con una configurazione di bilanciamento del carico con due membri per il bilanciamento del carico *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="5cab2-207">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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

### <a name="rate-limits"></a><span data-ttu-id="5cab2-208">Limiti di velocità</span><span class="sxs-lookup"><span data-stu-id="5cab2-208">Rate Limits</span></span>
<span data-ttu-id="5cab2-209">Utilizzando *mod_ratelimit*, incluso nel *httpd* modulo, la larghezza di banda del client può essere limitato:</span><span class="sxs-lookup"><span data-stu-id="5cab2-209">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="5cab2-210">Il file di esempio Limita larghezza di banda come 600 KB al secondo nel percorso radice:</span><span class="sxs-lookup"><span data-stu-id="5cab2-210">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
