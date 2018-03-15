---
title: Hosting di ASP.NET Core in Linux con Nginx
author: rick-anderson
description: Viene descritto come configurare Nginx come un proxy inverso in Ubuntu 16.04 per inoltrare il traffico HTTP a un'app web ASP.NET Core in esecuzione su Kestrel.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: a1de177fcd41c925a85e5aab9a0d236249b7da0b
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a>Hosting di ASP.NET Core in Linux con Nginx

Di [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Questa guida spiega come configurare un ambiente ASP.NET Core pronto per la produzione in un server Ubuntu 16.04.

> [!NOTE]
> Per Ubuntu 14.04, *supervisord* è consigliabile come soluzione per il monitoraggio del processo Kestrel. *systemd* non è disponibile in Ubuntu 14.04. [Vedere la versione precedente di questo documento](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).

In questa guida:

* Inserisce un'app di ASP.NET Core esistente con un server proxy inverso.
* Imposta backup del server proxy inverso per inoltrare le richieste al server web Kestrel.
* Assicura che l'app web viene eseguito all'avvio come daemon.
* Consente di configurare uno strumento di gestione del processo per riavviare l'app web.

## <a name="prerequisites"></a>Prerequisiti

1. Accesso a un server Ubuntu 16.04 con un account utente standard con privilegio sudo
1. Un'app di ASP.NET Core esistente

## <a name="copy-over-the-app"></a>Copiare l'app

Eseguire [dotnet pubblicare](/dotnet/core/tools/dotnet-publish) dall'ambiente di sviluppo per creare un pacchetto di un'app in una directory indipendente che è possibile eseguire sul server.

Copia l'app ASP.NET Core per il server utilizzando qualsiasi strumento integra nel flusso di lavoro dell'organizzazione (ad esempio, SCP, FTP). Testare l'applicazione, ad esempio:

* Dalla riga di comando, eseguire `dotnet <app_assembly>.dll`.
* In un browser passare a `http://<serveraddress>:<port>` per verificare se l'applicazione funziona in Linux. 
 
## <a name="configure-a-reverse-proxy-server"></a>Configurare un server proxy inverso

Un proxy inverso è una configurazione comune per la gestione delle applicazioni web dinamiche. Un proxy inverso termina la richiesta HTTP e lo inoltra all'applicazione ASP.NET Core.

### <a name="why-use-a-reverse-proxy-server"></a>Perché usare un server proxy inverso?

Kestrel è molto utile per disporre il contenuto dinamico da ASP.NET Core. Tuttavia, le funzionalità di gestione web non sono come ricco di funzionalità come server, ad esempio IIS, Apache o Nginx. Un server proxy inverso può scaricare il lavoro, ad esempio del contenuto statico, la memorizzazione nella cache le richieste, la compressione di richieste e la terminazione SSL dal server HTTP. Un server proxy inverso può risiedere in un computer dedicato o essere distribuito insieme a un server HTTP.

Ai fini di questa guida viene usata una singola istanza di Nginx. Viene eseguito sullo stesso server, insieme al server HTTP. In base ai requisiti, può essere scelto un programma di installazione diverso.

Poiché le richieste vengono inoltrate da proxy inverso, utilizzare il Middleware di intestazioni inoltrati dal [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) pacchetto. Gli aggiornamenti di middleware di `Request.Scheme`, usando il `X-Forwarded-Proto` intestazione, in modo che gli URI di reindirizzamento e altri criteri di sicurezza funzionino correttamente.

Quando si utilizza qualsiasi tipo di middleware di autenticazione, il Middleware di intestazioni inoltrati deve eseguiti per primo. Questo ordinamento garantisce che il middleware di autenticazione può utilizzare i valori di intestazione e generare l'URI di reindirizzamento corretto.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Richiamare il [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metodo `Startup.Configure` prima di chiamare [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) o simile middleware lo schema di autenticazione:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Richiamare il [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metodo `Startup.Configure` prima di chiamare [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) e [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) o schema di autenticazione simili middleware:

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

Se non [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) specificato per il middleware, le intestazioni predefinite per l'inoltro sono `None`.

### <a name="install-nginx"></a>Installare Nginx

```bash
sudo apt-get install nginx
```

> [!NOTE]
> Se non saranno installati i moduli Nginx facoltativi, potrebbe essere necessario compilazione Nginx dall'origine.

Usare `apt-get` per installare Nginx. Il programma di installazione crea uno script di inizializzazione System V che esegue Nginx come daemon all'avvio del sistema. Poiché Nginx è stato installato per la prima volta, avviarlo in modo esplicito eseguendo:

```bash
sudo service nginx start
```

Verificare che un browser visualizzi la pagina di destinazione predefinita per Nginx.

### <a name="configure-nginx"></a>Configurare Nginx

Per configurare Nginx come un proxy inverso per inoltrare le richieste all'applicazione ASP.NET di base, modificare */etc/nginx/sites-available/default*. Aprirlo in un editor di testo e sostituire il contenuto con quanto segue:

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Se non si `server_name` corrispondenze, Nginx utilizza il server predefinito. Se non è definito alcun server predefinito, il primo server nel file di configurazione è il server predefinito. Come procedura consigliata, aggiungere un server predefinito specifico che restituisce un codice di stato di 444 nel file di configurazione. Un esempio di configurazione server predefinita è:

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

Con il precedente server di configurazione predefiniti e di file Nginx accetta traffico pubblico sulla porta 80 con intestazione host `example.com` o `*.example.com`. Le richieste non corrisponda a questi host non ottenere inoltrate a Kestrel. Nginx inoltra le richieste corrispondenti a Kestrel in `http://localhost:5000`. Vedere [come nginx elabora una richiesta](https://nginx.org/docs/http/request_processing.html) per ulteriori informazioni.

> [!WARNING]
> L'impossibilità di specificare una corretta [direttiva nome_server](https://nginx.org/docs/http/server_names.html) espone l'app a vulnerabilità di sicurezza. Associazione con caratteri jolly sottodominio (ad esempio, `*.example.com`) non comporta il rischio di sicurezza se è possibile controllare il dominio padre intero (in contrapposizione a `*.com`, che è vulnerabile). Vedere [rfc7230 sezione-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) per ulteriori informazioni.

Una volta stabilita la configurazione di Nginx, eseguire `sudo nginx -t` per verificare la sintassi dei file di configurazione. Se il test di file di configurazione ha esito positivo, forzare Nginx per rendere effettive le modifiche eseguendo `sudo nginx -s reload`.

## <a name="monitoring-the-app"></a>Monitoraggio dell'app

Il server è configurato per inoltrare le richieste effettuate a `http://<serveraddress>:80` al ASP.NET Core in esecuzione l'app in Kestrel in `http://127.0.0.1:5000`. Tuttavia, Nginx perché non è configurato per gestire il processo Kestrel. *systemd* può essere utilizzato per creare un file del servizio per avviare e monitorare l'app web sottostante. *systemd* è un sistema di inizializzazione che offre molte funzionalità potenti per l'avvio, l'arresto e la gestione dei processi. 

### <a name="create-the-service-file"></a>Creare il file del servizio

Creare il file di definizione del servizio:

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Di seguito è un esempio di file per l'applicazione del servizio:

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

**Nota:** se l'utente *www dati* non viene usata dalla configurazione, è necessario creare innanzitutto l'utente definito qui e ha le proprietà appropriate per i file.
**Nota:** Linux è un sistema di file tra maiuscole e minuscole. L'impostazione di ASPNETCORE_ENVIRONMENT su "Produzione" produce la ricerca del file di configurazione *appsettings. Production.JSON*, non *appsettings.production.json*.

Salvare il file e abilitare il servizio.

```bash
systemctl enable kestrel-hellomvc.service
```

Avviare il servizio e verificare che sia in esecuzione.

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

Con il proxy inverso configurate e gestite tramite systemd Kestrel, l'app web è completamente configurato ed è possibile accedervi da un browser del computer locale in `http://localhost`. È inoltre possibile accedere da un computer remoto, impedendo qualsiasi firewall che potrebbe essere bloccato. Esaminare le intestazioni di risposta, il `Server` intestazione Mostra l'app ASP.NET Core servito dal Kestrel.

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Vista dei log

Poiché l'app web utilizzando Kestrel viene gestita mediante `systemd`, tutti gli eventi e i processi vengono registrati in una registrazione centralizzata. Tuttavia, questo giornale include tutte le voci per tutti i servizi e i processi gestiti da `systemd`. Per visualizzare le voci specifiche di `kestrel-hellomvc.service`, usare il comando seguente:

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Per filtrare ulteriormente, opzioni come `--since today`, `--until 1 hour ago` o una combinazione delle stesse consentono di ridurre la quantità di voci restituite.

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a>Protezione dell'app

### <a name="enable-apparmor"></a>Abilitare AppArmor

I moduli di protezione Linux (LSM) è un framework che fa parte del kernel Linux dal 2.6 Linux. LSM supporta diverse implementazioni di moduli di protezione. [AppArmor](https://wiki.ubuntu.com/AppArmor) è un modulo LSM che implementa un sistema di controllo di accesso obbligatorio che consente di circoscrivere il programma a un set limitato di risorse. Verificare che AppArmor sia abilitato e configurato correttamente.

### <a name="configuring-the-firewall"></a>Configurazione del firewall

Chiudere tutte le porte esterne che non sono in uso. Il firewall UFW (Uncomplicated Firewall) offre un front-end per `iptables` attraverso un'interfaccia della riga di comando per la configurazione del firewall. Verificare che `ufw` è configurato per consentire il traffico su tutte le porte necessita.

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a>Protezione di Nginx

La distribuzione predefinita di Nginx non abilita SSL. Per abilitare ulteriori funzionalità di sicurezza, compilarle dal codice sorgente.

#### <a name="download-the-source-and-install-the-build-dependencies"></a>Scaricare il codice sorgente e installare le dipendenze di compilazione

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a>Modificare il nome di risposta Nginx

Modificare *src/http/ngx_http_header_filter_module.c*:

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a>Configurare le opzioni e compilare

La libreria PCRE è obbligatoria per le espressioni regolari. Le espressioni regolari sono usate nella direttiva ubicazione per ngx_http_rewrite_module. http_ssl_module aggiunge il supporto del protocollo HTTPS.

È consigliabile utilizzare un firewall, app web come *ModSecurity* per finalizzare l'app.

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a>Configurare SSL

* Configurare il server per ascoltare il traffico HTTPS sulla porta `443` specificando un certificato valido emesso da un'autorità di certificati attendibili (CA).

* Rafforzare la sicurezza mediante l'utilizzo di alcune delle procedure di cui è illustrate nell'esempio seguente */etc/nginx/nginx.conf* file. Gli esempi includono la scelta di una crittografia più complessa e il reindirizzamento di tutto il traffico su HTTP a HTTPS.

* L'aggiunta di un'intestazione `HTTP Strict-Transport-Security` (HSTS) garantisce che tutte le richieste successive vengano effettuate dal esclusivamente via HTTPS.

* Non aggiungere l'intestazione di sicurezza-trasporto-Strict o scelto un appropriato `max-age` se SSL verrà disabilitata in futuro.

Aggiungere il file di configurazione */etc/nginx/proxy.conf*:

[!code-nginx[](linux-nginx/proxy.conf)]

Aggiungere il file di configurazione */etc/nginx/proxy.conf*. L'esempio contiene entrambe le sezioni `http` e `server` in un unico file di configurazione.

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>Proteggere Nginx dal clickjacking
Il clickjacking è una tecnica fraudolenta con cui i clic di un utente vengono "catturati" e reindirizzati a un oggetto in un sito infetto. Utilizzare X-FRAME-OPTIONS per proteggere il sito.

Modificare il file *nginx.conf*:

```bash
sudo nano /etc/nginx/nginx.conf
```

Aggiungere la riga `add_header X-Frame-Options "SAMEORIGIN";` e salvare il file, quindi riavviare Nginx.

#### <a name="mime-type-sniffing"></a>Analisi del tipo MIME

Questa intestazione impedisce alla maggior parte dei browser di dedurre dall'analisi del tipo MIME un tipo di contenuto diverso da quello dichiarato, poiché l'intestazione indica al browser di non eseguire l'override del tipo di contenuto della risposta. Con l'opzione `nosniff`, se il server indica che il contenuto è "text/html", il browser ne esegue il rendering come "text/html".

Modificare il file *nginx.conf*:

```bash
sudo nano /etc/nginx/nginx.conf
```

Aggiungere la riga `add_header X-Content-Type-Options "nosniff";` e salvare il file, quindi riavviare Nginx.
