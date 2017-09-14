---
title: Hosting di ASP.NET Core in Linux con Nginx
description: Viene descritto come configurare Nginx come proxy inverso in Ubuntu 16.04 per inoltrare il traffico HTTP a un'applicazione Web ASP.NET Core in esecuzione su Kestrel.
keywords: ASP.NET Core, Linux, nginx, Ubuntu, proxy inverso
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 08/21/2017
ms.topic: article
ms.assetid: 1c33e576-33de-481a-8ad3-896b94fde0e3
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/linuxproduction
ms.openlocfilehash: 1f2b5fc6d769c63110f832a31cd0d0aa8c3298e9
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/25/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-nginx-and-deploy-to-it"></a>Configurare un ambiente di hosting per ASP.NET Core in Linux con Nginx e distribuirlo

Di [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Questa guida spiega come configurare un ambiente ASP.NET Core pronto per la produzione in un server Ubuntu 16.04.

**Nota:** per Ubuntu 14.04, è consigliabile supervisord come soluzione per il monitoraggio del processo Kestrel. systemd non è disponibile in Ubuntu 14.04. [Vedere la versione precedente di questo documento](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

In questa guida:

* Posizionare un'applicazione ASP.NET Core esistente dietro un server proxy inverso
* Configurare il server proxy inverso per l'inoltro delle richieste al server Web Kestrel
* Verificare che l'applicazione Web venga eseguita all'avvio come daemon
* Configurare uno strumento di gestione del processo per consentire il riavvio dell'applicazione Web

## <a name="prerequisites"></a>Prerequisiti

1. Accesso a un server Ubuntu 16.04 con un account utente standard con privilegio sudo
2. Un'applicazione ASP.NET Core esistente

## <a name="copy-over-your-app"></a>Copiare l'applicazione

Eseguire `dotnet publish` dall'ambiente di sviluppo per creare un pacchetto per un'applicazione in una directory indipendente che può essere eseguita sul server.

Copiare l'applicazione ASP.NET Core sul server usando qualsiasi strumento (SCP, FTP e così via) che si possa integrare nel flusso di lavoro. Testare l'applicazione, ad esempio:
 - Eseguire `dotnet yourapp.dll` dalla riga di comando
 - In un browser passare a `http://<serveraddress>:<port>` per verificare se l'applicazione funziona in Linux. 
 
**Nota:** usare [Yeoman](xref:client-side/yeoman) per creare una nuova applicazione ASP.NET Core per un nuovo progetto.

## <a name="configure-a-reverse-proxy-server"></a>Configurare un server proxy inverso

Un proxy inverso è una configurazione comune per la gestione delle applicazioni Web dinamiche. Un proxy inverso termina la richiesta HTTP e la inoltra all'applicazione ASP.NET Core.

### <a name="why-use-a-reverse-proxy-server"></a>Perché usare un server proxy inverso?

Kestrel funziona perfettamente per la gestione del contenuto dinamico da ASP.NET Core, tuttavia le parti relative al Web non sono così ricche di funzionalità come i server IIS, Apache o Nginx. Un server proxy inverso è in grado di eseguire l'offload di attività come la gestione del contenuto statico, le richieste di memorizzazione nella cache, la compressione delle richieste e la terminazione SSL dal server HTTP. Un server proxy inverso può risiedere in un computer dedicato o essere distribuito insieme a un server HTTP.

Ai fini di questa guida viene usata una singola istanza di Nginx. Viene eseguito sullo stesso server, insieme al server HTTP. In base alle proprie esigenze, è possibile scegliere un'impostazione diversa.

Poiché le richieste vengono inoltrate dal proxy inverso, usare il middleware `ForwardedHeaders` dal pacchetto `Microsoft.AspNetCore.HttpOverrides`. Questo middleware aggiorna `Request.Scheme` usando l'intestazione `X-Forwarded-Proto`, in modo che gli URI di reindirizzamento e altri criteri di sicurezza funzionino correttamente.

Quando si configura un server proxy inverso, il middleware di autenticazione richiede `UseForwardedHeaders` per essere eseguito per primo. Questo ordine garantisce che il middleware di autenticazione sia in grado di usare i valori interessati e di generare gli URI di reindirizzamento corretti.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Richiamare il metodo `UseForwardedHeaders` (nel metodo `Configure` di *Startup.cs*) prima di chiamare `UseAuthentication` o un middleware con schema di autenticazione simile:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Richiamare il metodo `UseForwardedHeaders` (nel metodo `Configure` di *Startup.cs*) prima di chiamare `UseIdentity` e `UseFacebookAuthentication` o un middleware con schema di autenticazione simile:

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

### <a name="install-nginx"></a>Installare Nginx

```bash
sudo apt-get install nginx
```

> [!NOTE]
> Se si prevede di installare i moduli Nginx facoltativi, può essere necessario compilare Nginx dall'origine.

Usare `apt-get` per installare Nginx. Il programma di installazione crea uno script di inizializzazione System V che esegue Nginx come daemon all'avvio del sistema. Poiché Nginx è stato installato per la prima volta, avviarlo in modo esplicito eseguendo:

```bash
sudo service nginx start
```

Verificare che un browser visualizzi la pagina di destinazione predefinita per Nginx.

### <a name="configure-nginx"></a>Configurare Nginx

Per configurare Nginx come proxy inverso per inoltrare le richieste all'applicazione ASP.NET Core, modificare `/etc/nginx/sites-available/default`. Aprirlo in un editor di testo e sostituire il contenuto con quanto segue:

```nginx
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Questo file di configurazione Nginx inoltra il traffico pubblico in ingresso dalla porta `80` alla porta `5000`.

Dopo aver apportato le modifiche alla configurazione Nginx, è possibile eseguire `sudo nginx -t` per verificare la sintassi dei file di configurazione. Se il test dei file di configurazione ha esito positivo, è possibile chiedere a Nginx di applicare le modifiche eseguendo `sudo nginx -s reload`.

## <a name="monitoring-our-application"></a>Monitoraggio dell'applicazione

Nginx è ora configurato in modo da inoltrare le richieste effettuate a `http://yourhost:80` all'applicazione ASP.NET Core eseguita in Kestrel all'indirizzo `http://127.0.0.1:5000`. Tuttavia, Nginx non è configurato per la gestione del processo Kestrel. È possibile usare *systemd* e creare un file del servizio per avviare e monitorare l'applicazione Web sottostante. *systemd* è un sistema di inizializzazione che offre molte funzionalità potenti per l'avvio, l'arresto e la gestione dei processi. 

### <a name="create-the-service-file"></a>Creare il file del servizio

Creare il file di definizione del servizio:

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Di seguito è riportato un esempio di file del servizio per questa applicazione:

```ini
[Unit]
Description=Example .NET Web API Application running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
RestartSec=10  # Restart service after 10 seconds if dotnet service crashes
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

**Nota:** se l'utente *www-data* non viene usato dalla configurazione, è necessario creare prima l'utente definito qui e assegnargli correttamente la proprietà dei file.

Salvare il file e abilitare il servizio.

```bash
systemctl enable kestrel-hellomvc.service
```

Avviare il servizio e verificare che sia in esecuzione.

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API Application running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

Con il proxy inverso configurate e Kestrel gestito con systemd, l'applicazione Web è completamente configurata ed è possibile accedervi da un browser nel computer locale in `http://localhost`. È anche possibile accedervi da un computer remoto, escludendo eventuali firewall che potrebbero impedirlo. Se si esaminano le intestazioni di risposta, l'intestazione `Server` indica che l'applicazione ASP.NET Core è gestita da Kestrel.

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Visualizzazione dei log

Poiché l'applicazione Web che usa Kestrel viene gestita con `systemd`, tutti gli eventi e i processi vengono registrati in un giornale di registrazione centralizzato. Tuttavia, questo giornale include tutte le voci per tutti i servizi e i processi gestiti da `systemd`. Per visualizzare le voci specifiche di `kestrel-hellomvc.service`, usare il comando seguente:

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Per filtrare ulteriormente, opzioni come `--since today`, `--until 1 hour ago` o una combinazione delle stesse consentono di ridurre la quantità di voci restituite.

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a>Protezione dell'applicazione

### <a name="enable-apparmor"></a>Abilitare AppArmor

Linux Security Modules (LSM) è un framework che fa parte del kernel Linux a partire da Linux 2.6. LSM supporta diverse implementazioni di moduli di protezione. [AppArmor](https://wiki.ubuntu.com/AppArmor) è un modulo LSM che implementa un sistema di controllo di accesso obbligatorio che consente di circoscrivere il programma a un set limitato di risorse. Verificare che AppArmor sia abilitato e configurato correttamente.

### <a name="configuring-our-firewall"></a>Configurazione del firewall

Chiudere tutte le porte esterne che non sono in uso. Il firewall UFW (Uncomplicated Firewall) offre un front-end per `iptables` attraverso un'interfaccia della riga di comando per la configurazione del firewall. Verificare che `ufw` sia configurato per consentire il traffico su tutte le porte necessarie.

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

# Download nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a>Modificare il nome di risposta Nginx

Modificare *src/http/ngx_http_header_filter_module.c*:

```c
static char ngx_http_server_string[] = "Server: Your Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Your Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a>Configurare le opzioni e compilare

La libreria PCRE è obbligatoria per le espressioni regolari. Le espressioni regolari sono usate nella direttiva ubicazione per ngx_http_rewrite_module. http_ssl_module aggiunge il supporto del protocollo HTTPS.

È consigliabile usare un firewall per applicazioni Web, ad esempio *ModSecurity*, per la protezione avanzata dell'applicazione.

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a>Configurare SSL

* Configurare il server in modo che sia in ascolto del traffico HTTPS sulla porta `443` specificando un certificato valido emesso da un'autorità di certificazione attendibile.

* Rafforzare la protezione adottando alcune delle procedure descritte nel file */etc/nginx/nginx.conf* che segue. Gli esempi includono la scelta di una crittografia più complessa e il reindirizzamento di tutto il traffico su HTTP a HTTPS.

* L'aggiunta di un'intestazione `HTTP Strict-Transport-Security` (HSTS) garantisce che tutte le richieste successive vengano effettuate dal esclusivamente via HTTPS.

* Non aggiungere l'intestazione Strict-Transport-Security o scegliere un elemento `max-age` appropriato se si prevede di disabilitare SSL in futuro.

Aggiungere il file di configurazione */etc/nginx/proxy.conf*:

[!code-nginx[Principale](linuxproduction/proxy.conf)]

Aggiungere il file di configurazione */etc/nginx/proxy.conf*. L'esempio contiene entrambe le sezioni `http` e `server` in un unico file di configurazione.

[!code-nginx[Principale](../publishing/linuxproduction/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>Proteggere Nginx dal clickjacking
Il clickjacking è una tecnica fraudolenta con cui i clic di un utente vengono "catturati" e reindirizzati a un oggetto in un sito infetto. Usare X-FRAME-OPTIONS per proteggere il sito.

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
