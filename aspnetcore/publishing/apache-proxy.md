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
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-apache-and-deploy-to-it"></a>Configurare un ambiente di hosting per ASP.NET Core in Linux con Apache e distribuirlo

Di [Shayne Boyer](https://github.com/spboyer)

Apache è un server HTTP molto diffuso e può essere configurato come un proxy per reindirizzare il traffico HTTP simile a nginx. In questa guida si apprenderà come configurare Apache in CentOS 7 e come usarlo in qualità di proxy inverso per accogliere le connessioni in ingresso e reindirizzarle verso l'applicazione ASP.NET Core in esecuzione su Kestrel. A tale scopo, si userà l'estensione *mod_proxy* e altri moduli Apache correlati.

## <a name="prerequisites"></a>Prerequisiti

1. Un server in esecuzione su CentOS 7, con un account utente standard con privilegio sudo.
2. Un'applicazione ASP.NET Core esistente. 

## <a name="publish-your-application"></a>Pubblicare un'applicazione

Eseguire `dotnet publish -c Release` nell'ambiente di sviluppo per impacchettare l'applicazione in una directory autonoma che è possibile eseguire sul server. L'applicazione pubblicata deve quindi essere copiata sul server tramite SCP, FTP o un altro metodo di trasferimento di file. 

> [!NOTE]
> In uno scenario di distribuzione di produzione un flusso di lavoro di integrazione continua esegue le operazioni di pubblicazione dell'applicazione e di copia delle risorse nel server. 

## <a name="configure-a-proxy-server"></a>Configurazione di un server proxy

Un proxy inverso è una configurazione comune per la gestione delle applicazioni Web dinamiche. Il proxy inverso termina la richiesta HTTP e la inoltra all'applicazione ASP.NET.

Un server proxy inoltra le richieste del client a un altro server anziché evaderle da solo. Un proxy inverso inoltra a una destinazione fissa, in genere per conto di client non autorizzati. In questa guida Apache viene configurato come proxy inverso in esecuzione nello stesso server usato da Kestrel per gestire l'applicazione ASP.NET Core. 

Ogni parte dell'applicazione può essere presente su computer fisici separati, su contenitori Docker, o su una combinazione di configurazioni, a seconda delle esigenze di architettura o delle restrizioni.

### <a name="install-apache"></a>Installare Apache

L'installazione del server web Apache in CentOS è un comando singolo, ma aggiornare prima i pacchetti.

```bash
    sudo yum update -y
```

In questo modo si garantisce che tutti i pacchetti installati vengono aggiornati a una versione più recente. Installare Apache tramite `yum`

```bash
    sudo yum -y install httpd mod_ssl
```

L'output deve riflettere qualcosa di simile a quanto segue.

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
> In questo esempio l'output rispecchia httpd.86_64 poiché la versione CentOS 7 è a 64 bit. L'output potrebbe essere diverso per il server. Per verificare dove è installato Apache, eseguire `whereis httpd` da un prompt dei comandi. 

### <a name="configure-apache-for-reverse-proxy"></a>Configurare Apache per il proxy inverso

I file di configurazione per Apache si trovano all'interno della directory `/etc/httpd/conf.d/`. Qualsiasi file con l'estensione **.conf** verrà elaborato in ordine alfabetico oltre ai file di configurazione del modulo in `/etc/httpd/conf.modules.d/`, che contiene tutti i file di configurazione necessari per caricare i moduli.

Creare un file di configurazione per l'app, per questo esempio verrà chiamato `hellomvc.conf`

```text
    <VirtualHost *:80>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/
        ErrorLog /var/log/httpd/hellomvc-error.log
        CustomLog /var/log/httpd/hellomvc-access.log common
    </VirtualHost>
```

Il nodo *VirtualHost*, di cui è possibile definirne molti in un file o in un server in molti file, è impostato per l'ascolto su qualsiasi indirizzo IP tramite la porta 80. Le due righe successive vengono impostate per passare tutte le richieste ricevute nella directory radice per la porta della macchina 127.0.0.1 5000 e in ordine inverso. Per essere una comunicazione bidirezionale, sono necessarie entrambe le impostazioni *ProxyPass* e *ProxyPassReverse*.

La registrazione può essere configurata per VirtualHost tramite le direttive *ErrorLog* e *CustomLog*. *ErrorLog* è la posizione in cui il server registrerà gli errori e *CustomLog* imposta il nome di file e il formato del file di log. In questo caso si tratta della posizione in cui verranno registrate le informazioni sulla richiesta. Sarà presente una riga per ogni richiesta.

Salvare il file e testare la configurazione. Se tutto ciò viene superato, la risposta deve essere `Syntax [OK]`.

```bash
    sudo service httpd configtest
```

Riavviare Apache.

```text
    sudo systemctl restart httpd
    sudo systemctl enable httpd
```

## <a name="monitoring-our-application"></a>Monitoraggio dell'applicazione

Apache è ora configurato in modo da inoltrare le richieste eseguite a `http://localhost:80` all'applicazione ASP.NET Core eseguita in Kestrel all'indirizzo `http://127.0.0.1:5000`.  Tuttavia, Apache non è configurato per la gestione del processo Kestrel. Si userà *systemd* e si creerà un file del servizio per avviare e monitorare l'applicazione Web sottostante. *systemd* è un sistema di inizializzazione che offre molte funzionalità potenti per l'avvio, l'arresto e la gestione dei processi. 


### <a name="create-the-service-file"></a>Creare il file del servizio

Creare il file di definizione del servizio 

```bash
    sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Un esempio di file del servizio per questa applicazione.

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
> **Utente:**-- se l'utente *apache* non viene usato dalla configurazione, è necessario creare prima l'utente definito qui e assegnargli correttamente la proprietà dei file

Salvare il file e abilitare il servizio.

```bash
    systemctl enable kestrel-hellomvc.service
```

Avviare il servizio e verificare che sia in esecuzione.

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

Con il proxy inverso configurate e Kestrel gestito con systemd, l'applicazione Web è completamente configurata ed è possibile accedervi da un browser nel computer locale in `http://localhost`. Se si esaminano le intestazioni di risposta, il **Server** indica comunque che l'applicazione ASP.NET Core è gestita da Kestrel.

```text
    HTTP/1.1 200 OK
    Date: Tue, 11 Oct 2016 16:22:23 GMT
    Server: Kestrel
    Keep-Alive: timeout=5, max=98
    Connection: Keep-Alive
    Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Vista dei log

Poiché l'applicazione Web che usa Kestrel viene gestita tramite systemd, tutti gli eventi e i processi vengono registrati in un giornale di registrazione centralizzato. Tuttavia, questo giornale include tutte le voci per tutti i servizi e i processi gestiti da systemd. Per visualizzare le voci specifiche di `kestrel-hellomvc.service`, usare il comando seguente.

```bash
    sudo journalctl -fu kestrel-hellomvc.service
```

Per filtrare ulteriormente, opzioni come `--since today`, `--until 1 hour ago` o una combinazione delle stesse consentono di ridurre la quantità di voci restituite.

```bash
    sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a>Protezione dell'applicazione

### <a name="configure-firewall"></a>Configurare firewall

*Firewalld* è un daemon dinamico per gestire dei firewall con un supporto per le aree di rete, sebbene sia comunque possibile usare degli iptables per gestire le porte e il filtraggio dei pacchetti. Firewalld deve essere installato per impostazione predefinita, `yum` può essere usato per installare il pacchetto o verificarlo.

```bash
    sudo yum install firewalld -y
```

Tramite `firewalld` è possibile aprire solo le porte necessarie per l'applicazione. In questo caso vengono usate le porte 80 e 443. I comandi seguenti le impostano in modo permanente per l'apertura.

```bash
    sudo firewall-cmd --add-port=80/tcp --permanent
    sudo firewall-cmd --add-port=443/tcp --permanent
```

Ricaricare le impostazioni del firewall e verificare i servizi e le porte disponibili nell'area predefinita. Le opzioni sono disponibili controllando `firewall-cmd -h`

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

Per configurare Apache per SSL, viene usato il modulo mod_ssl.  Questo componente è stato installato inizialmente quando è stato installato il modulo `httpd`. Se è stato perso o non è stato installato, usare yum per aggiungerlo alla propria configurazione.

```bash
    sudo yum install mod_ssl
```
Per attivare SSL, installare `mod_rewrite`

```bash
    sudo yum install mod_rewrite
```

Il file `hellomvc.conf` che è stato creato per questo esempio deve essere modificato per consentire la riscrittura e per permettere di aggiungere la nuova sezione **VirtualHost** per HTTPS.

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
> Questo esempio usa un certificato generato localmente. **SSLCertificateFile** deve essere il file del certificato primario per il nome di dominio. **SSLCertificateKeyFile** deve essere il file di chiave generato quando è stato creato il CSR. **SSLCertificateChainFile** deve essere il file di certificato intermedio (se presente) che è stato fornito dall'autorità di certificazione

Salvare il file e testare la configurazione.

```bash
    sudo service httpd configtest
```

Riavviare Apache.

```bash
    sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a>Altri suggerimenti di Apache

### <a name="additional-headers"></a>Intestazioni aggiuntive 
Al fine di proteggere da attacchi dannosi vi sono alcune intestazioni che devono essere modificate o aggiunte. Verificare che il modulo `mod_headers` sia installato.

```bash
    sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking"></a>Proteggere Apache dal clickjacking
Il clickjacking è una tecnica fraudolenta con cui i clic di un utente vengono "catturati" e reindirizzati a un oggetto in un sito infetto. Usare X-FRAME-OPTIONS per proteggere il sito.

Modificare il file httpd.conf.

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

Aggiungere la riga `Header append X-FRAME-OPTIONS "SAMEORIGIN"` e salvare il file, quindi riavviare Apache.

#### <a name="mime-type-sniffing"></a>Analisi del tipo MIME

Questa intestazione impedisce a Internet Explorer di dedurre dall'analisi del tipo MIME un tipo di contenuto diverso da quello dichiarato, poiché l'intestazione indica al browser di non eseguire l'override del tipo di contenuto della risposta. Con l'opzione nosniff, se il server indica che il contenuto è "text/html", il browser ne esegue il rendering come "text/html".

Modificare il file httpd.conf.

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

Aggiungere la riga `Header set X-Content-Type-Options "nosniff"` e salvare il file, quindi riavviare Apache.

### <a name="load-balancing"></a>Bilanciamento del carico 

Questo esempio illustra come impostare e configurare Apache CentOS 7 e Kestrel nello stesso computer dell'istanza.  Tuttavia, per non avere un singolo punto di errore; l'uso di *mod_proxy_balancer* e la modifica di VirtualHost consentono la gestione di più istanze delle applicazioni web dietro il server proxy di Apache.

```bash
    sudo yum install mod_proxy_balancer
```

Nel file di configurazione un'istanza aggiuntiva dell'app `hellomvc` è stata configurata per l'esecuzione sulla porta 5001 e la sezione *Proxy* è stata impostata con una configurazione di bilanciamento del carico con due membri per il bilanciamento del carico *byrequests* .

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

### <a name="rate-limits"></a>Limiti di velocità
Usando `mod_ratelimit`, incluso nel modulo `htttpd` è possibile limitare la quantità di larghezza di banda dei client. 

```bash
    sudo nano /etc/httpd/conf.d/ratelimit.conf
```
Il file di esempio limita la larghezza di banda a 600 KB al secondo nel percorso della radice.

```text
    <IfModule mod_ratelimit.c>
        <Location />
            SetOutputFilter RATE_LIMIT
            SetEnv rate-limit 600
        </Location>
    </IfModule>
```
