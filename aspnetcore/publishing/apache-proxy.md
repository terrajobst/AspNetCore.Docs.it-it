---
title: Hosting di ASP.NET Core in Linux con Apache
description: Informazioni su come configurare Apache come server di proxy inverso in CentOS per reindirizzare il traffico HTTP a un'applicazione Web ASP.NET Core in esecuzione su Kestrel.
keywords: ASP.NET Core, Apache, CentOS, proxy inverso, Linux, mod_proxy, httpd, hosting
author: spboyer
ms.author: spboyer
manager: wpickett
ms.date: 10/19/2016
ms.topic: article
ms.assetid: fa9b0cb7-afb3-4361-9e7e-33afffeaca0c
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/apache-proxy
ms.openlocfilehash: 34ede2fdebe0e9516f62e39618e7adba3c8c89db
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-apache-and-deploy-to-it"></a><span data-ttu-id="ef2b1-104">Configurare un ambiente di hosting per ASP.NET Core in Linux con Apache e distribuirlo</span><span class="sxs-lookup"><span data-stu-id="ef2b1-104">Set up a hosting environment for ASP.NET Core on Linux with Apache, and deploy to it</span></span>

<span data-ttu-id="ef2b1-105">Di [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="ef2b1-105">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="ef2b1-106">Apache è un server HTTP molto diffuso e può essere configurato come un proxy per reindirizzare il traffico HTTP simile a nginx.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-106">Apache is a very popular HTTP server and can be configured as a proxy to redirect HTTP traffic similar to nginx.</span></span> <span data-ttu-id="ef2b1-107">In questa guida si apprenderà come configurare Apache in CentOS 7 e come usarlo in qualità di proxy inverso per accogliere le connessioni in ingresso e reindirizzarle verso l'applicazione ASP.NET Core in esecuzione su Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-107">In this guide, we will learn how to set up Apache on CentOS 7 and use it as a reverse proxy to welcome incoming connections and redirect them to the ASP.NET Core application running on Kestrel.</span></span> <span data-ttu-id="ef2b1-108">A tale scopo, si userà l'estensione *mod_proxy* e altri moduli Apache correlati.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-108">For this purpose, we will use the *mod_proxy* extension and other related Apache modules.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ef2b1-109">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="ef2b1-109">Prerequisites</span></span>

1. <span data-ttu-id="ef2b1-110">Un server in esecuzione su CentOS 7, con un account utente standard con privilegio sudo.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-110">A server running CentOS 7, with a standard user account with sudo privilege.</span></span>
2. <span data-ttu-id="ef2b1-111">Un'applicazione ASP.NET Core esistente.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-111">An existing ASP.NET Core application.</span></span> 

## <a name="publish-your-application"></a><span data-ttu-id="ef2b1-112">Pubblicare un'applicazione</span><span class="sxs-lookup"><span data-stu-id="ef2b1-112">Publish your application</span></span>

<span data-ttu-id="ef2b1-113">Eseguire `dotnet publish -c Release` nell'ambiente di sviluppo per impacchettare l'applicazione in una directory autonoma che è possibile eseguire sul server.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-113">Run `dotnet publish -c Release` from your development environment to package your application into a self-contained directory that can run on your server.</span></span> <span data-ttu-id="ef2b1-114">L'applicazione pubblicata deve quindi essere copiata sul server tramite SCP, FTP o un altro metodo di trasferimento di file.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-114">The published application must then be copied to the server using SCP, FTP or other file transfer method.</span></span> 

> [!NOTE]
> <span data-ttu-id="ef2b1-115">In uno scenario di distribuzione di produzione un flusso di lavoro di integrazione continua esegue le operazioni di pubblicazione dell'applicazione e di copia delle risorse nel server.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-115">Under a production deployment scenario, a continuous integration workflow does the work of publishing the application and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="ef2b1-116">Configurazione di un server proxy</span><span class="sxs-lookup"><span data-stu-id="ef2b1-116">Configure a proxy server</span></span>

<span data-ttu-id="ef2b1-117">Un proxy inverso è una configurazione comune per la gestione delle applicazioni Web dinamiche.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-117">A reverse proxy is a common setup for serving dynamic web applications.</span></span> <span data-ttu-id="ef2b1-118">Il proxy inverso termina la richiesta HTTP e la inoltra all'applicazione ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-118">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET application.</span></span>

<span data-ttu-id="ef2b1-119">Un server proxy inoltra le richieste del client a un altro server anziché evaderle da solo.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-119">A proxy server is one which forwards client requests to another server instead of fulfilling them itself.</span></span> <span data-ttu-id="ef2b1-120">Un proxy inverso inoltra a una destinazione fissa, in genere per conto di client non autorizzati.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-120">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="ef2b1-121">In questa guida Apache viene configurato come proxy inverso in esecuzione nello stesso server usato da Kestrel per gestire l'applicazione ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-121">In this guide, Apache is being configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core application.</span></span> 

<span data-ttu-id="ef2b1-122">Ogni parte dell'applicazione può essere presente su computer fisici separati, su contenitori Docker, o su una combinazione di configurazioni, a seconda delle esigenze di architettura o delle restrizioni.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-122">Each piece of the application can exist on separate physical machines, Docker containers, or a combination of configurations depending on your architectural needs or restrictions.</span></span>

### <a name="install-apache"></a><span data-ttu-id="ef2b1-123">Installare Apache</span><span class="sxs-lookup"><span data-stu-id="ef2b1-123">Install Apache</span></span>

<span data-ttu-id="ef2b1-124">L'installazione del server web Apache in CentOS è un comando singolo, ma aggiornare prima i pacchetti.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-124">Installing the Apache web server on CentOS is a single command, but first let's update our packages.</span></span>

```bash
    sudo yum update -y
```

<span data-ttu-id="ef2b1-125">In questo modo si garantisce che tutti i pacchetti installati vengono aggiornati a una versione più recente.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-125">This ensures that all of the installed packages are updated to their latest version.</span></span> <span data-ttu-id="ef2b1-126">Installare Apache tramite `yum`</span><span class="sxs-lookup"><span data-stu-id="ef2b1-126">Install Apache using `yum`</span></span>

```bash
    sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="ef2b1-127">L'output deve riflettere qualcosa di simile a quanto segue.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-127">The output should reflect something similar to the following.</span></span>

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
> <span data-ttu-id="ef2b1-128">In questo esempio l'output rispecchia httpd.86_64 poiché la versione CentOS 7 è a 64 bit.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-128">In this example the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="ef2b1-129">L'output potrebbe essere diverso per il server.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-129">The output may be different for your server.</span></span> <span data-ttu-id="ef2b1-130">Per verificare dove è installato Apache, eseguire `whereis httpd` da un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-130">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span> 

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="ef2b1-131">Configurare Apache per il proxy inverso</span><span class="sxs-lookup"><span data-stu-id="ef2b1-131">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="ef2b1-132">I file di configurazione per Apache si trovano all'interno della directory `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-132">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="ef2b1-133">Qualsiasi file con l'estensione **.conf** verrà elaborato in ordine alfabetico oltre ai file di configurazione del modulo in `/etc/httpd/conf.modules.d/`, che contiene tutti i file di configurazione necessari per caricare i moduli.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-133">Any file with the **.conf** extension will be processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="ef2b1-134">Creare un file di configurazione per l'app, per questo esempio verrà chiamato `hellomvc.conf`</span><span class="sxs-lookup"><span data-stu-id="ef2b1-134">Create a configuration file for your app, for this example we'll call it `hellomvc.conf`</span></span>

```text
    <VirtualHost *:80>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/
        ErrorLog /var/log/httpd/hellomvc-error.log
        CustomLog /var/log/httpd/hellomvc-access.log common
    </VirtualHost>
```

<span data-ttu-id="ef2b1-135">Il nodo *VirtualHost*, di cui è possibile definirne molti in un file o in un server in molti file, è impostato per l'ascolto su qualsiasi indirizzo IP tramite la porta 80.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-135">The *VirtualHost* node, of which there can be multiple in a file or on a server in many files, is set to listen on any IP address using port 80.</span></span> <span data-ttu-id="ef2b1-136">Le due righe successive vengono impostate per passare tutte le richieste ricevute nella directory radice per la porta della macchina 127.0.0.1 5000 e in ordine inverso.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-136">The next two lines are set to pass all requests received at the root to the machine 127.0.0.1 port 5000 and in reverse.</span></span> <span data-ttu-id="ef2b1-137">Per essere una comunicazione bidirezionale, sono necessarie entrambe le impostazioni *ProxyPass* e *ProxyPassReverse*.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-137">For there to be bi-directional communication, both settings *ProxyPass* and *ProxyPassReverse* are required.</span></span>

<span data-ttu-id="ef2b1-138">La registrazione può essere configurata per VirtualHost tramite le direttive *ErrorLog* e *CustomLog*.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-138">Logging can be configured per VirtualHost using *ErrorLog* and *CustomLog* directives.</span></span> <span data-ttu-id="ef2b1-139">*ErrorLog* è la posizione in cui il server registrerà gli errori e *CustomLog* imposta il nome di file e il formato del file di log.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-139">*ErrorLog* is the location where the server will log errors and *CustomLog* sets the filename and format of log file.</span></span> <span data-ttu-id="ef2b1-140">In questo caso si tratta della posizione in cui verranno registrate le informazioni sulla richiesta.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-140">In our case this is where request information will be logged.</span></span> <span data-ttu-id="ef2b1-141">Sarà presente una riga per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-141">There will be one line for each request.</span></span>

<span data-ttu-id="ef2b1-142">Salvare il file e testare la configurazione.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-142">Save the file, and test the configuration.</span></span> <span data-ttu-id="ef2b1-143">Se tutto ciò viene superato, la risposta deve essere `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-143">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
    sudo service httpd configtest
```

<span data-ttu-id="ef2b1-144">Riavviare Apache.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-144">Restart Apache.</span></span>

```text
    sudo systemctl restart httpd
    sudo systemctl enable httpd
```

## <a name="monitoring-our-application"></a><span data-ttu-id="ef2b1-145">Monitoraggio dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="ef2b1-145">Monitoring our application</span></span>

<span data-ttu-id="ef2b1-146">Apache è ora configurato in modo da inoltrare le richieste eseguite a `http://localhost:80` all'applicazione ASP.NET Core eseguita in Kestrel all'indirizzo `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-146">Apache is now setup to forward requests made to `http://localhost:80` on to the ASP.NET Core application running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="ef2b1-147">Tuttavia, Apache non è configurato per la gestione del processo Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-147">However, Apache is not set up to manage the Kestrel process.</span></span> <span data-ttu-id="ef2b1-148">Si userà *systemd* e si creerà un file del servizio per avviare e monitorare l'applicazione Web sottostante.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-148">We will use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="ef2b1-149">*systemd* è un sistema di inizializzazione che offre molte funzionalità potenti per l'avvio, l'arresto e la gestione dei processi.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-149">*systemd* is an init system that provides many powerful features for starting, stopping and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="ef2b1-150">Creare il file del servizio</span><span class="sxs-lookup"><span data-stu-id="ef2b1-150">Create the service file</span></span>

<span data-ttu-id="ef2b1-151">Creare il file di definizione del servizio</span><span class="sxs-lookup"><span data-stu-id="ef2b1-151">Create the service definition file</span></span> 

```bash
    sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="ef2b1-152">Un esempio di file del servizio per questa applicazione.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-152">An example service file for our application.</span></span>

```text
[Unit]
    Description=Example .NET Web API Application running on CentOS 7

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
> <span data-ttu-id="ef2b1-153">**Utente:**-- se l'utente *apache* non viene usato dalla configurazione, è necessario creare prima l'utente definito qui e assegnargli correttamente la proprietà dei file</span><span class="sxs-lookup"><span data-stu-id="ef2b1-153">**User** -- If the user *apache* is not used by your configuration, the user defined here must be created first and given proper ownership for files</span></span>

<span data-ttu-id="ef2b1-154">Salvare il file e abilitare il servizio.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-154">Save the file and enable the service.</span></span>

```bash
    systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="ef2b1-155">Avviare il servizio e verificare che sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-155">Start the service and verify that it is running.</span></span>

```
    systemctl start kestrel-hellomvc.service
    systemctl status kestrel-hellomvc.service

    ● kestrel-hellomvc.service - Example .NET Web API Application running on CentOS 7
        Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
        Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
    Main PID: 9021 (dotnet)
        CGroup: /system.slice/kestrel-hellomvc.service
                └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="ef2b1-156">Con il proxy inverso configurate e Kestrel gestito con systemd, l'applicazione Web è completamente configurata ed è possibile accedervi da un browser nel computer locale in `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-156">With the reverse proxy configured and Kestrel managed through systemd, the web application is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="ef2b1-157">Se si esaminano le intestazioni di risposta, il **Server** indica comunque che l'applicazione ASP.NET Core è gestita da Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-157">Inspecting the response headers, the **Server** still shows the ASP.NET Core application being served by Kestrel.</span></span>

```text
    HTTP/1.1 200 OK
    Date: Tue, 11 Oct 2016 16:22:23 GMT
    Server: Kestrel
    Keep-Alive: timeout=5, max=98
    Connection: Keep-Alive
    Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="ef2b1-158">Vista dei log</span><span class="sxs-lookup"><span data-stu-id="ef2b1-158">Viewing logs</span></span>

<span data-ttu-id="ef2b1-159">Poiché l'applicazione Web che usa Kestrel viene gestita tramite systemd, tutti gli eventi e i processi vengono registrati in un giornale di registrazione centralizzato.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-159">Since the web application using Kestrel is managed using systemd, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="ef2b1-160">Tuttavia, questo giornale include tutte le voci per tutti i servizi e i processi gestiti da systemd.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-160">However, this journal includes all entries for all services and processes managed by systemd.</span></span> <span data-ttu-id="ef2b1-161">Per visualizzare le voci specifiche di `kestrel-hellomvc.service`, usare il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-161">To view the `kestrel-hellomvc.service` specific items, use the following command.</span></span>

```bash
    sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="ef2b1-162">Per filtrare ulteriormente, opzioni come `--since today`, `--until 1 hour ago` o una combinazione delle stesse consentono di ridurre la quantità di voci restituite.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-162">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
    sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a><span data-ttu-id="ef2b1-163">Protezione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="ef2b1-163">Securing our application</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="ef2b1-164">Configurare firewall</span><span class="sxs-lookup"><span data-stu-id="ef2b1-164">Configure firewall</span></span>

<span data-ttu-id="ef2b1-165">*Firewalld* è un daemon dinamico per gestire dei firewall con un supporto per le aree di rete, sebbene sia comunque possibile usare degli iptables per gestire le porte e il filtraggio dei pacchetti.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-165">*Firewalld* is a dynamic daemon to manage firewall with support for network zones, although you can still use iptables to manage ports and packet filtering.</span></span> <span data-ttu-id="ef2b1-166">Firewalld deve essere installato per impostazione predefinita, `yum` può essere usato per installare il pacchetto o verificarlo.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-166">Firewalld should be installed by default, `yum` can be used to install the package or verify.</span></span>

```bash
    sudo yum install firewalld -y
```

<span data-ttu-id="ef2b1-167">Tramite `firewalld` è possibile aprire solo le porte necessarie per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-167">Using `firewalld` you can open only the ports needed for the application.</span></span> <span data-ttu-id="ef2b1-168">In questo caso vengono usate le porte 80 e 443.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-168">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="ef2b1-169">I comandi seguenti le impostano in modo permanente per l'apertura.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-169">The following commands permanently sets these to open.</span></span>

```bash
    sudo firewall-cmd --add-port=80/tcp --permanent
    sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="ef2b1-170">Ricaricare le impostazioni del firewall e verificare i servizi e le porte disponibili nell'area predefinita.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-170">Reload the firewall settings, and check the available services and ports in the default zone.</span></span> <span data-ttu-id="ef2b1-171">Le opzioni sono disponibili controllando `firewall-cmd -h`</span><span class="sxs-lookup"><span data-stu-id="ef2b1-171">Options are available by inspecting `firewall-cmd -h`</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="ef2b1-172">Configurazione SSL</span><span class="sxs-lookup"><span data-stu-id="ef2b1-172">SSL configuration</span></span>

<span data-ttu-id="ef2b1-173">Per configurare Apache per SSL, viene usato il modulo mod_ssl.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-173">To configure Apache for SSL, the mod_ssl module is used.</span></span>  <span data-ttu-id="ef2b1-174">Questo componente è stato installato inizialmente quando è stato installato il modulo `httpd`.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-174">This was installed initially when we installed the `httpd` module.</span></span> <span data-ttu-id="ef2b1-175">Se è stato perso o non è stato installato, usare yum per aggiungerlo alla propria configurazione.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-175">If it was missed or not installed, use yum to add it to your configuration.</span></span>

```bash
    sudo yum install mod_ssl
```
<span data-ttu-id="ef2b1-176">Per attivare SSL, installare `mod_rewrite`</span><span class="sxs-lookup"><span data-stu-id="ef2b1-176">To enforce SSL, install `mod_rewrite`</span></span>

```bash
    sudo yum install mod_rewrite
```

<span data-ttu-id="ef2b1-177">Il file `hellomvc.conf` che è stato creato per questo esempio deve essere modificato per consentire la riscrittura e per permettere di aggiungere la nuova sezione **VirtualHost** per HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-177">The `hellomvc.conf` file that was created for this example needs to be modified to enable the rewrite as well as adding the new **VirtualHost** section for HTTPS.</span></span>

```text
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
> <span data-ttu-id="ef2b1-178">Questo esempio usa un certificato generato localmente.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-178">This example is using a locally generated certificate.</span></span> <span data-ttu-id="ef2b1-179">**SSLCertificateFile** deve essere il file del certificato primario per il nome di dominio.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-179">**SSLCertificateFile** should be your primary certificate file for your domain name.</span></span> <span data-ttu-id="ef2b1-180">**SSLCertificateKeyFile** deve essere il file di chiave generato quando è stato creato il CSR.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-180">**SSLCertificateKeyFile** should be the key file generated when you created the CSR.</span></span> <span data-ttu-id="ef2b1-181">**SSLCertificateChainFile** deve essere il file di certificato intermedio (se presente) che è stato fornito dall'autorità di certificazione</span><span class="sxs-lookup"><span data-stu-id="ef2b1-181">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by your certificate authority</span></span>

<span data-ttu-id="ef2b1-182">Salvare il file e testare la configurazione.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-182">Save the file, and test the configuration.</span></span>

```bash
    sudo service httpd configtest
```

<span data-ttu-id="ef2b1-183">Riavviare Apache.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-183">Restart Apache.</span></span>

```bash
    sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="ef2b1-184">Altri suggerimenti di Apache</span><span class="sxs-lookup"><span data-stu-id="ef2b1-184">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="ef2b1-185">Intestazioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="ef2b1-185">Additional Headers</span></span> 
<span data-ttu-id="ef2b1-186">Al fine di proteggere da attacchi dannosi vi sono alcune intestazioni che devono essere modificate o aggiunte.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-186">In order to secure against malicious attacks there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="ef2b1-187">Verificare che il modulo `mod_headers` sia installato.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-187">Ensure that the `mod_headers` module is installed.</span></span>

```bash
    sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking"></a><span data-ttu-id="ef2b1-188">Proteggere Apache dal clickjacking</span><span class="sxs-lookup"><span data-stu-id="ef2b1-188">Secure Apache from clickjacking</span></span>
<span data-ttu-id="ef2b1-189">Il clickjacking è una tecnica fraudolenta con cui i clic di un utente vengono "catturati"</span><span class="sxs-lookup"><span data-stu-id="ef2b1-189">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="ef2b1-190">e reindirizzati a un oggetto in un sito infetto.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-190">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="ef2b1-191">Usare X-FRAME-OPTIONS per proteggere il sito.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-191">Use X-FRAME-OPTIONS to secure your site.</span></span>

<span data-ttu-id="ef2b1-192">Modificare il file httpd.conf.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-192">Edit the httpd.conf file.</span></span>

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="ef2b1-193">Aggiungere la riga `Header append X-FRAME-OPTIONS "SAMEORIGIN"` e salvare il file, quindi riavviare Apache.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-193">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"` and save the file, then restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="ef2b1-194">Analisi del tipo MIME</span><span class="sxs-lookup"><span data-stu-id="ef2b1-194">MIME-type sniffing</span></span>

<span data-ttu-id="ef2b1-195">Questa intestazione impedisce a Internet Explorer di dedurre dall'analisi del tipo MIME un tipo di contenuto diverso da quello dichiarato, poiché l'intestazione indica al browser di non eseguire l'override del tipo di contenuto della risposta.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-195">This header prevents Internet Explorer from MIME-sniffing a response away from the declared content-type as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="ef2b1-196">Con l'opzione nosniff, se il server indica che il contenuto è "text/html", il browser ne esegue il rendering come "text/html".</span><span class="sxs-lookup"><span data-stu-id="ef2b1-196">With the nosniff option, if the server says the content is text/html, the browser will render it as text/html.</span></span>

<span data-ttu-id="ef2b1-197">Modificare il file httpd.conf.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-197">Edit the httpd.conf file.</span></span>

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="ef2b1-198">Aggiungere la riga `Header set X-Content-Type-Options "nosniff"` e salvare il file, quindi riavviare Apache.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-198">Add the line `Header set X-Content-Type-Options "nosniff"` and save the file, then restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="ef2b1-199">Bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="ef2b1-199">Load Balancing</span></span> 

<span data-ttu-id="ef2b1-200">Questo esempio illustra come impostare e configurare Apache CentOS 7 e Kestrel nello stesso computer dell'istanza.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-200">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span>  <span data-ttu-id="ef2b1-201">Tuttavia, per non avere un singolo punto di errore; l'uso di *mod_proxy_balancer* e la modifica di VirtualHost consentono la gestione di più istanze delle applicazioni web dietro il server proxy di Apache.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-201">However, in order to not have a single point of failure; using *mod_proxy_balancer* and modifying the VirtualHost would allow for managing mutliple instances of the web applications behind the Apache proxy server.</span></span>

```bash
    sudo yum install mod_proxy_balancer
```

<span data-ttu-id="ef2b1-202">Nel file di configurazione un'istanza aggiuntiva dell'app `hellomvc` è stata configurata per l'esecuzione sulla porta 5001 e la sezione *Proxy* è stata impostata con una configurazione di bilanciamento del carico con due membri per il bilanciamento del carico *byrequests* .</span><span class="sxs-lookup"><span data-stu-id="ef2b1-202">In the configuration file, an additional instance of the `hellomvc` app has been setup to run on port 5001 and the *Proxy* section has been set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```text
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

### <a name="rate-limits"></a><span data-ttu-id="ef2b1-203">Limiti di velocità</span><span class="sxs-lookup"><span data-stu-id="ef2b1-203">Rate Limits</span></span>
<span data-ttu-id="ef2b1-204">Usando `mod_ratelimit`, incluso nel modulo `htttpd` è possibile limitare la quantità di larghezza di banda dei client.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-204">Using `mod_ratelimit`, which is included in the `htttpd` module you can limit the amount of bandwidth of clients.</span></span> 

```bash
    sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="ef2b1-205">Il file di esempio limita la larghezza di banda a 600 KB al secondo nel percorso della radice.</span><span class="sxs-lookup"><span data-stu-id="ef2b1-205">The example file limits bandwidth as 600 KB/sec under the root location.</span></span>

```text
    <IfModule mod_ratelimit.c>
        <Location />
            SetOutputFilter RATE_LIMIT
            SetEnv rate-limit 600
        </Location>
    </IfModule>
```
