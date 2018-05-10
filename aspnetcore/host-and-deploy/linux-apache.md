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
ms.openlocfilehash: 473585f1be180645395c14a154c9c017ca50edab
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/10/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a>Hosting di ASP.NET Core in Linux con Apache

Di [Shayne Boyer](https://github.com/spboyer)

Utilizzando questa Guida, informazioni su come configurare [Apache](https://httpd.apache.org/) come server proxy inverso [CentOS 7](https://www.centos.org/) reindirizzare il traffico HTTP a un'app web ASP.NET Core in esecuzione in [Kestrel](xref:fundamentals/servers/kestrel). Il [mod_proxy estensione](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) e moduli correlati creare proxy inverso del server.

## <a name="prerequisites"></a>Prerequisiti

1. Server che esegue CentOS 7 con un account utente standard con privilegi sudo
2. App ASP.NET Core

## <a name="publish-the-app"></a>Pubblicare l'app

Pubblicare l'app come un [distribuzione indipendente](/dotnet/core/deploying/#self-contained-deployments-scd) nella configurazione di rilascio per il runtime CentOS 7 (`centos.7-x64`). Copiare il contenuto del *bin/Release/netcoreapp2.0/centos.7-x64/publish* cartella al server tramite SCP, FTP o altro metodo di trasferimento di file.

> [!NOTE]
> In uno scenario di distribuzione di produzione, un flusso di lavoro di integrazione continua esegue le operazioni di pubblicazione dell'app e copiare le risorse nel server di. 

## <a name="configure-a-proxy-server"></a>Configurazione di un server proxy

Un proxy inverso è una configurazione comune per la gestione delle applicazioni web dinamiche. Il proxy inverso termina la richiesta HTTP e lo inoltra all'applicazione ASP.NET.

È un server proxy che inoltra le richieste client a un altro server invece che soddisfano le richieste di se stesso. Un proxy inverso inoltra a una destinazione fissa, in genere per conto di client non autorizzati. In questa guida Apache è configurato come proxy inverso in esecuzione nello stesso server che gestisce l'applicazione ASP.NET Core Kestrel.

Poiché le richieste vengono inoltrate da proxy inverso, utilizzare il Middleware di intestazioni inoltrati dal [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) pacchetto. Gli aggiornamenti di middleware di `Request.Scheme`, usando il `X-Forwarded-Proto` intestazione, in modo che gli URI di reindirizzamento e altri criteri di sicurezza funzionino correttamente.

Quando si utilizza qualsiasi tipo di middleware di autenticazione, il Middleware di intestazioni inoltrati deve eseguiti per primo. Questo ordinamento garantisce che il middleware di autenticazione può utilizzare i valori di intestazione e generare l'URI di reindirizzamento corretto.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Richiamare il [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metodo `Startup.Configure` prima di chiamare [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) o simile middleware di schema di autenticazione. Configurare il middleware di inoltrare il `X-Forwarded-For` e `X-Forwarded-Proto` intestazioni:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Richiamare il [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metodo `Startup.Configure` prima di chiamare [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) e [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) o schema di autenticazione simili middleware. Configurare il middleware di inoltrare il `X-Forwarded-For` e `X-Forwarded-Proto` intestazioni:

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

Potrebbero essere necessari interventi di configurazione aggiuntivi per le app ospitate dietro a server proxy e a servizi di bilanciamento del carico. Per altre informazioni, vedere [Configurare ASP.NET Core per l'utilizzo di server proxy e servizi di bilanciamento del carico](xref:host-and-deploy/proxy-load-balancer).

### <a name="install-apache"></a>Installare Apache

CentOS i pacchetti di aggiornamento alle versioni stabili più recenti:

```bash
sudo yum update -y
```

Installare il server web Apache in CentOS con un singolo `yum` comando:

```bash
sudo yum -y install httpd mod_ssl
```

Esempio di output dopo l'esecuzione del comando:

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
> In questo esempio, l'output rispecchi httpd.86_64 poiché la versione CentOS 7 a 64 bit. Per verificare dove è installato Apache, eseguire `whereis httpd` da un prompt dei comandi.

### <a name="configure-apache-for-reverse-proxy"></a>Configurare Apache per il proxy inverso

I file di configurazione per Apache si trovano all'interno della directory `/etc/httpd/conf.d/`. Qualsiasi file con il *.conf* estensione viene elaborata in ordine alfabetico oltre i file di configurazione del modulo in `/etc/httpd/conf.modules.d/`, che contiene tutte le configurazioni di file necessarie per caricare i moduli.

Creare un file di configurazione denominato *hellomvc.conf*, per l'app:

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

Il `VirtualHost` blocco può comparire più volte, in uno o più file in un server. Nel file di configurazione precedente Apache accetta traffico pubblico sulla porta 80. Il dominio `www.example.com` viene gestita e `*.example.com` alias risolve allo stesso sito. Vedere [supporto basato sul nome host virtuale](https://httpd.apache.org/docs/current/vhosts/name-based.html) per ulteriori informazioni. Le richieste vengono elaborate dal proxy alla radice alla porta 5000 del server 127.0.0.1. Per la comunicazione bidirezionale, `ProxyPass` e `ProxyPassReverse` sono necessari.

> [!WARNING]
> L'impossibilità di specificare una corretta [ServerName direttiva](https://httpd.apache.org/docs/current/mod/core.html#servername) nel **VirtualHost** blocco espone l'app a vulnerabilità di sicurezza. Associazione con caratteri jolly sottodominio (ad esempio, `*.example.com`) non comporta il rischio di sicurezza se è possibile controllare il dominio padre intero (in contrapposizione a `*.com`, che è vulnerabile). Vedere la [sezione 5.4 di RFC7230](https://tools.ietf.org/html/rfc7230#section-5.4) per altre informazioni.

Registrazione può essere configurata `VirtualHost` utilizzando `ErrorLog` e `CustomLog` direttive. `ErrorLog` è il percorso in cui i registri del server, errori e `CustomLog` imposta il nome e il formato del file di log. In questo caso, si tratta in cui vengono registrate informazioni di richiesta. È presente una riga per ogni richiesta.

Salvare il file e il test della configurazione. Se tutto ciò viene superato, la risposta deve essere `Syntax [OK]`.

```bash
sudo service httpd configtest
```

Riavviare Apache:

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a>Monitoraggio dell'app

Apache è ora configurato per inoltrare le richieste effettuate a `http://localhost:80` all'app ASP.NET Core in esecuzione in Kestrel in `http://127.0.0.1:5000`.  Tuttavia, Apache perché non è configurato per gestire il processo Kestrel. Utilizzare *systemd* e creare un file del servizio per avviare e monitorare l'app web sottostante. *systemd* è un sistema di inizializzazione che offre molte funzionalità potenti per l'avvio, l'arresto e la gestione dei processi. 


### <a name="create-the-service-file"></a>Creare il file del servizio

Creare il file di definizione del servizio:

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Un file del servizio di esempio per l'app:

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
> **Utente** &mdash; se l'utente *apache* non viene usata dalla configurazione, è necessario creare prima l'utente e specificato le proprietà appropriate per i file.

> [!NOTE]
> Alcuni valori (ad esempio, stringhe di connessione SQL) devono utilizzare caratteri di escape per i provider di configurazione leggere le variabili di ambiente. Utilizzare il comando seguente per generare un valore correttamente con caratteri di escape da utilizzare nel file di configurazione:
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

Salvare il file e attivare il servizio:

```bash
systemctl enable kestrel-hellomvc.service
```

Avviare il servizio e verificare che sia in esecuzione:

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

Con il proxy inverso configurato e gestite tramite Kestrel *systemd*, l'app web è completamente configurato ed è possibile accedervi da un browser del computer locale in `http://localhost`. Esaminare le intestazioni di risposta, il **Server** intestazione indica che l'applicazione ASP.NET Core viene gestita da Kestrel:

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Vista dei log

Poiché l'app web utilizzando Kestrel viene gestita mediante *systemd*, processi e gli eventi vengono registrati in una registrazione centralizzata. Tuttavia, queste registrazioni includono voci per tutti i servizi e processi gestiti da *systemd*. Per visualizzare le voci specifiche di `kestrel-hellomvc.service`, usare il comando seguente:

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Per il filtro di tempo, specificare opzioni della fase con il comando. Ad esempio, utilizzare `--since today` per filtrare per il giorno corrente o `--until 1 hour ago` per visualizzare le voci dell'ora precedente. Per ulteriori informazioni, vedere il [pagina man per journalctl](https://www.unix.com/man-page/centos/1/journalctl/).

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a>Protezione dell'app

### <a name="configure-firewall"></a>Configurare firewall

*Firewalld* è un daemon dinamico per gestire il firewall con supporto per aree di rete. Porte e il filtraggio dei pacchetti può comunque essere gestiti da iptables. *Firewalld* deve essere installato per impostazione predefinita. `yum` può essere utilizzato per installare il pacchetto o verificare che sia installato.

```bash
sudo yum install firewalld -y
```

Utilizzare `firewalld` aprire solo le porte necessarie per l'app. In questo caso vengono usate le porte 80 e 443. I seguenti comandi set in modo permanente le porte 80 e 443 per aprire:

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

Ricaricare le impostazioni del firewall. Controllare i servizi disponibili e le porte nell'area predefinita. Sono disponibili le opzioni controllando `firewall-cmd -h`.

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

### <a name="ssl-configuration"></a>Configurazione SSL

Per configurare Apache per SSL, il *mod_ssl* modulo è utilizzato. Quando il *httpd* modulo è stato installato, il *mod_ssl* modulo è stato anche installato. Se non è stato installato, utilizzare `yum` per aggiungerlo alla configurazione.

```bash
sudo yum install mod_ssl
```
Per attivare SSL, installare il `mod_rewrite` per abilitare la riscrittura URL:

```bash
sudo yum install mod_rewrite
```

Modificare il *hellomvc.conf* file per abilitare la riscrittura degli URL e proteggere la comunicazione sulla porta 443:

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
> In questo esempio utilizza un certificato generato localmente. **SSLCertificateFile** deve essere il file del certificato primario per il nome di dominio. **SSLCertificateKeyFile** deve essere il file di chiave generato CSR viene creato. **SSLCertificateChainFile** deve essere il file di certificato intermedio (se presente) che è stato fornito dall'autorità di certificazione.

Salvare il file e la configurazione di test:

```bash
sudo service httpd configtest
```

Riavviare Apache:

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a>Altri suggerimenti di Apache

### <a name="additional-headers"></a>Intestazioni aggiuntive

Per proteggere contro gli attacchi, esistono alcune intestazioni che devono essere modificate o aggiunta. Verificare che il `mod_headers` modulo è installato:

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a>Apache protetto da attacchi clickjacking

[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), nota anche come un *dell'interfaccia utente ricorso attacco*, costituisce un attacco dannoso, in cui un visitatore di un sito Web sia indotto a fare clic su un collegamento o un pulsante in una pagina diversa rispetto a cui sta attualmente visitando. Utilizzare `X-FRAME-OPTIONS` per proteggere il sito.

Modificare il *httpd. conf* file:

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

Aggiungere la riga `Header append X-FRAME-OPTIONS "SAMEORIGIN"`. Salvare il file. Riavviare Apache.

#### <a name="mime-type-sniffing"></a>Analisi del tipo MIME

Il `X-Content-Type-Options` intestazione impedisce di Internet Explorer a *analisi MIME* (determinazione di un file `Content-Type` dal contenuto del file). Se il server imposta il `Content-Type` intestazione `text/html` con il `nosniff` set di opzioni, Internet Explorer visualizza il contenuto come `text/html` indipendentemente dal contenuto del file.

Modificare il *httpd. conf* file:

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

Aggiungere la riga `Header set X-Content-Type-Options "nosniff"`. Salvare il file. Riavviare Apache.

### <a name="load-balancing"></a>Bilanciamento del carico 

Questo esempio illustra come impostare e configurare Apache CentOS 7 e Kestrel nello stesso computer dell'istanza. Per non disporre di un singolo punto di errore. utilizzando *mod_proxy_balancer* e la modifica di **VirtualHost** consentirebbe la gestione di più istanze delle App web dietro il server proxy di Apache.

```bash
sudo yum install mod_proxy_balancer
```

Nel file di configurazione illustrato di seguito, un'istanza aggiuntiva del `hellomvc` app è configurato per l'esecuzione sulla porta 5001. Il *Proxy* sezione è impostata con una configurazione di bilanciamento del carico con due membri per il bilanciamento del carico *byrequests*.

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

### <a name="rate-limits"></a>Limiti di velocità
Utilizzando *mod_ratelimit*, incluso nel *httpd* modulo, la larghezza di banda del client può essere limitato:

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
Il file di esempio Limita larghezza di banda come 600 KB al secondo nel percorso radice:

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
