---
title: Ospitare e distribuire Blazor sul lato client
author: guardrex
description: Informazioni su come ospitare e distribuire un'app Blazor con ASP.NET Core, reti per la distribuzione di contenuti (CDN), file server e pagine di GitHub.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/13/2019
uid: host-and-deploy/blazor/client-side
ms.openlocfilehash: ea8ece266809913e32ac212bc55cb3c2499c234f
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/19/2019
ms.locfileid: "65874970"
---
# <a name="host-and-deploy-blazor-client-side"></a>Ospitare e distribuire Blazor sul lato client

Di [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) e [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Valori di configurazione dell'host

Le app Blazor che usano il [modello di hosting sul lato client](xref:blazor/hosting-models#client-side) possono accettare i valori di configurazione dell'host seguenti come argomenti della riga di comando in fase di esecuzione nell'ambiente di sviluppo.

### <a name="content-root"></a>Radice del contenuto

L'argomento `--contentroot` imposta il percorso assoluto sulla directory che contiene i file di contenuto dell'app. Negli esempi seguenti `/content-root-path` è il percorso radice del contenuto dell'app.

* Passare l'argomento quando si esegue localmente l'app a un prompt dei comandi. Dalla directory dell'app, eseguire:

  ```console
  dotnet run --contentroot=/content-root-path
  ```

* Aggiungere una voce al file *launchSettings.json* dell'app nel profilo **IIS Express**. Questa impostazione viene usata quando l'app viene eseguita con il debugger di Visual Studio e da un prompt dei comandi con `dotnet run`.

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* In Visual Studio specificare l'argomento in **Proprietà** > **Debug** > **Argomenti applicazione**. Impostando l'argomento nella pagina delle proprietà di Visual Studio, si aggiunge l'argomento al file *launchSettings.json*.

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a>Base del percorso

L'argomento `--pathbase` imposta il percorso di base dell'app per un'app eseguita in locale con un percorso virtuale non radice (il tag `<base>` `href` è impostato su un percorso diverso da `/` per staging e produzione). Negli esempi seguenti `/virtual-path` è la base del percorso dell'app. Per altre informazioni, vedere la sezione [Percorso di base dell'app](#app-base-path).

> [!IMPORTANT]
> A differenza del percorso specificato per `href` del tag `<base>`, non includere una barra finale (`/`) quando si passa il valore dell'argomento `--pathbase`. Se il percorso di base dell'app non viene specificato nel tag `<base>` come `<base href="/CoolApp/">` (include una barra finale), passare il valore dell'argomento della riga di comando come `--pathbase=/CoolApp` (senza barra finale).

* Passare l'argomento quando si esegue localmente l'app a un prompt dei comandi. Dalla directory dell'app, eseguire:

  ```console
  dotnet run --pathbase=/virtual-path
  ```

* Aggiungere una voce al file *launchSettings.json* dell'app nel profilo **IIS Express**. Questa impostazione viene usata quando l'app viene eseguita con il debugger di Visual Studio e da un prompt dei comandi con `dotnet run`.

  ```json
  "commandLineArgs": "--pathbase=/virtual-path"
  ```

* In Visual Studio specificare l'argomento in **Proprietà** > **Debug** > **Argomenti applicazione**. Impostando l'argomento nella pagina delle proprietà di Visual Studio, si aggiunge l'argomento al file *launchSettings.json*.

  ```console
  --pathbase=/virtual-path
  ```

### <a name="urls"></a>URL

L'argomento `--urls` imposta gli indirizzi IP o gli indirizzi host con le porte e i protocolli su cui eseguire l'ascolto per le richieste.

* Passare l'argomento quando si esegue localmente l'app a un prompt dei comandi. Dalla directory dell'app, eseguire:

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* Aggiungere una voce al file *launchSettings.json* dell'app nel profilo **IIS Express**. Questa impostazione viene usata quando l'app viene eseguita con il debugger di Visual Studio e da un prompt dei comandi con `dotnet run`.

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* In Visual Studio specificare l'argomento in **Proprietà** > **Debug** > **Argomenti applicazione**. Impostando l'argomento nella pagina delle proprietà di Visual Studio, si aggiunge l'argomento al file *launchSettings.json*.

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deployment"></a>Distribuzione

Con il [modello di hosting sul lato client](xref:blazor/hosting-models#client-side):

* L'app Blazor, le relative dipendenze e il runtime .NET vengono scaricati nel browser.
* L'app viene eseguita direttamente nel thread dell'interfaccia utente del browser. È supportata una delle strategie seguenti:
  * L'app Blazor viene fornita da un'app ASP.NET Core. Questa strategia viene trattata nella sezione [Distribuzione ospitata con ASP.NET Core](#hosted-deployment-with-aspnet-core).
  * L'app Blazor viene posizionata in un server o un servizio Web di hosting statico, in cui non viene usato .NET per fornire l'app Blazor. Questa strategia viene trattata nella sezione [Distribuzione autonoma](#standalone-deployment).

## <a name="configure-the-linker"></a>Configurare il linker

Blazor esegue il collegamento di linguaggio intermedio (IL) in ogni build per rimuovere IL non necessario dagli assembly di output. Il collegamento degli assembly può essere controllato in fase di compilazione. Per ulteriori informazioni, vedere <xref:host-and-deploy/blazor/configure-linker>.

## <a name="rewrite-urls-for-correct-routing"></a>Riscrivere gli URL per il routing corretto

Il routing delle richieste per i componenti di pagina in un'app sul lato client non è semplice come il routing delle richieste a un'app ospitata sul lato server. Si consideri un'app sul lato client con due pagine:

* **_Main.razor**&ndash; Viene caricata nella radice dell'app e contiene un collegamento alla pagina About (`href="About"`).
* **_About.Razor** &ndash; Pagina About.

Quando viene richiesto il documento predefinito dell'app usando la barra degli indirizzi del browser (ad esempio, `https://www.contoso.com/`):

1. Il browser invia una richiesta.
1. Viene restituita la pagina predefinita, di solito *index.html*.
1. *index.html* esegue il bootstrap dell'app.
1. Il router di Blazor viene caricato e viene visualizzata la pagina principale di Razor (*Main.razor*).

Nella pagina principale, se si seleziona il collegamento alla pagina About viene caricata tale pagina. La selezione del collegamento alla pagina About funziona nel client perché il router Blazor impedisce al browser di inviare una richiesta su Internet a `www.contoso.com` per `About` e fornisce la pagina About autonomamente. Tutte le richieste di pagine interne *all'interno dell'app sul lato client* funzionano allo stesso modo: Le richieste non attivano richieste basate su browser a risorse ospitate nel server su Internet. Le richieste vengono gestite internamente dal router.

Se una richiesta viene effettuata usando la barra degli indirizzi del browser per `www.contoso.com/About`, la richiesta ha esito negativo. La risorsa non esiste nell'host Internet dell'app, quindi viene restituita la risposta *404 non trovato*.

Dato che i browser inviano le richieste agli host basati su Internet per le pagine sul lato client, i server Web e i servizi di hosting devono riscrivere tutte le richieste per le risorse che non si trovano fisicamente nel server per la pagina *index.html*. Quando viene restituita la pagina *index.html*, il router sul lato client dell'app subentra e risponde con la risorsa corretta.

## <a name="app-base-path"></a>Percorso di base dell'app

Il *percorso di base dell'app* è il percorso radice dell'app virtuale nel server. Ad esempio, un'app che si trova nel server di Contoso in una cartella virtuale in `/CoolApp/` è raggiungibile all'indirizzo `https://www.contoso.com/CoolApp` e ha un percorso virtuale di base `/CoolApp/`. Impostando il percorso di base dell'app sul percorso virtuale (`<base href="/CoolApp/">`), l'app viene informata della posizione in cui risiede virtualmente nel server. L'app può usare il percorso di base dell'app per costruire URL relativi rispetto alla radice dell'app da un componente che non si trova nella directory radice. In questo modo i componenti presenti a diversi livelli della struttura di directory possono creare collegamenti ad altre risorse in varie posizioni in tutta l'app. Il percorso di base dell'app viene anche usato per intercettare i clic su collegamenti ipertestuali in cui la destinazione del collegamento `href` si trova all'interno dello spazio URI del percorso di base dell'app. Il router Blazor gestisce la navigazione interna.

In molti scenari di hosting, il percorso virtuale del server per l'app è la radice dell'app. In questi casi, il percorso di base dell'app è una barra (`<base href="/" />`), ovvero la configurazione predefinita per un'app. In altri scenari di hosting, ad esempio le pagine di GitHub, le directory virtuali di IIS e o le sottoapplicazioni, è necessario impostare il percorso di base dell'app sul percorso virtuale del server per l'app. Per impostare il percorso di base dell'app, aggiornare il tag `<base>` all'interno degli elementi del tag `<head>` del file*wwwroot/index.html*. Impostare il valore dell'attributo `href` su `/virtual-path/` (la barra finale è obbligatoria), dove `/virtual-path/` è il percorso radice dell'app virtuale completo nel server per l'app. Nell'esempio precedente, il percorso virtuale è impostato su `/CoolApp/`: `<base href="/CoolApp/">`.

Per un'app con un percorso virtuale non radice configurato (ad esempio, `<base href="/CoolApp/">`), l'app non riesce a trovare le relative risorse *quando viene eseguita in locale*. Per risolvere questo problema durante sviluppo e test locali, è possibile specificare un argomento *base del percorso* corrispondente al valore `href` del tag `<base>` in fase di esecuzione.

Per passare l'argomento di base del percorso con il percorso radice (`/`) quando si esegue l'app in locale, eseguire il comando `dotnet run` dalla directory dell'app usando l'opzione `--pathbase`:

```console
dotnet run --pathbase=/{Virtual Path (no trailing slash)}
```

Per un'app con un percorso virtuale di base di `/CoolApp/` (`<base href="/CoolApp/">`), il comando è:

```console
dotnet run --pathbase=/CoolApp
```

L'app risponde in locale all'indirizzo `http://localhost:port/CoolApp`.

Per altre informazioni, vedere la sezione sul [valore di configurazione dell'host della base del percorso](#path-base).

Se un'app usa il [modello di hosting sul lato client](xref:blazor/hosting-models#client-side) (basato sul modello di progetto **Blazor**, il modello `blazor` quando si usa il comando [dotnet new](/dotnet/core/tools/dotnet-new)) ed è ospitata come sottoapplicazione IIS in un'app ASP.NET Core, è importante disabilitare il gestore del modulo ASP.NET Core ereditato o assicurarsi che la sezione `<handlers>` dell'app radice (padre) nel file *web.config* non venga ereditata dalla sottoapplicazione.

Rimuovere il gestore nel file *web.config* pubblicato dell'app aggiungendo una sezione `<handlers>` al file:

```xml
<handlers>
  <remove name="aspNetCore" />
</handlers>
```

In alternativa, disabilitare l'ereditarietà della sezione `<system.webServer>` dell'app radice (padre) usando un elemento `<location>` con `inheritInChildApplications` impostato su `false`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" ... />
      </handlers>
      <aspNetCore ... />
    </system.webServer>
  </location>
</configuration>
```

La rimozione del gestore o la disabilitazione dell'ereditarietà viene eseguita in aggiunta alla configurazione del percorso di base dell'app come descritto in questa sezione. Impostare il percorso di base dell'app nel file *index.html* dell'app sull'alias IIS usato durante la configurazione della sottoapp in IIS.

## <a name="hosted-deployment-with-aspnet-core"></a>Distribuzione ospitata con ASP.NET Core

Una *distribuzione ospitata* fornisce l'app Blazor sul lato client ai browser da un'[app ASP.NET Core](xref:index) eseguita in un server.

L'app Blazor è inclusa con l'app ASP.NET Core nell'output pubblicato in modo che le due app vengano distribuite insieme. È necessario un server Web in grado di ospitare un'app ASP.NET Core. Per una distribuzione ospitata, Visual Studio include il modello di progetto **Blazor (ospitato in ASP.NET Core)** (modello `blazorhosted` quando si usa il comando [dotnet new](/dotnet/core/tools/dotnet-new)).

Per altre informazioni su hosting e distribuzione di app ASP.NET Core, vedere <xref:host-and-deploy/index>.

Per informazioni sulla distribuzione in Servizio app di Azure, vedere <xref:tutorials/publish-to-azure-webapp-using-vs>.

## <a name="standalone-deployment"></a>Distribuzione autonoma

Una *distribuzione autonoma* serve l'app Blazor sul lato client come un set di file statici richiesti direttamente dai client. Non viene usato un server Web per fornire l'app Blazor.

### <a name="iis"></a>IIS

IIS è un file server statico per app Blazor. Per configurare IIS per ospitare Blazor, vedere [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis) (Creare un sito Web statico in IIS).

Gli asset pubblicati vengono creati nella cartella */bin/Release/{FRAMEWORK DESTINAZIONE}/publish*. Ospitare il contenuto della cartella *publish* nel server Web o nel servizio di hosting.

#### <a name="webconfig"></a>web.config

Quando viene pubblicato un progetto Blazor, viene creato un file *web.config* con la configurazione di IIS seguente:

* Vengono impostati tipi MIME per le estensioni di file seguenti:
  * *.dll* &ndash; `application/octet-stream`
  * *.json* &ndash; `application/json`
  * *.wasm* &ndash; `application/wasm`
  * *.woff* &ndash; `application/font-woff`
  * *.woff2* &ndash; `application/font-woff`
* Viene abilitata la compressione HTTP per i tipi MIME seguenti:
  * `application/octet-stream`
  * `application/wasm`
* Vengono stabilite le regole URL Rewrite Module:
  * Specificare la sottodirectory in cui risiedono gli asset statici dell'app (*{NOME ASSEMBLY}/dist/{PERCORSO RICHIESTO}*).
  * Creare routing di fallback SPA in modo che le richieste per gli asset non file vengano reindirizzate al documento predefinito dell'app nella relativa cartella degli asset statici (*{NOME ASSEMBLY}/dist/index.html*).

#### <a name="install-the-url-rewrite-module"></a>Installare URL Rewrite Module

[URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) è necessario per riscrivere gli URL. Il modulo non viene installato per impostazione predefinita e non è disponibile per l'installazione come funzionalità del servizio ruolo Server Web (IIS). Il modulo deve essere scaricato dal sito Web IIS. Usare Installazione guidata piattaforma Web per installare il modulo:

1. In locale, passare alla [pagina di download di URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads). Per la versione in lingua inglese selezionare **WebPI** per scaricare il programma di installazione di Installazione guidata piattaforma Web. Per altre lingue, selezionare l'architettura appropriata per il server (x86 o x64) per scaricare il programma di installazione.
1. Copiare il programma di installazione nel server. Eseguire il programma di installazione. Selezionare il pulsante **Installa** e accettare le condizioni di licenza. Al termine dell'installazione non è necessario un riavvio del server.

#### <a name="configure-the-website"></a>Configurare il sito Web

Impostare il **percorso fisico** del sito Web sulla cartella dell'app. La cartella contiene:

* Il file *web.config* usato da IIS per configurare il sito Web, inclusi le regole di reindirizzamento richieste e i tipi di contenuto di file.
* Cartella degli asset statici dell'app.

#### <a name="troubleshooting"></a>Risoluzione dei problemi

Se si riceve un *errore interno del server 500* e Gestione IIS genera errori durante il tentativo di accedere alla configurazione del sito Web, verificare che sia installato URL Rewrite Module. Quando il modulo non è installato, il file *web.config* non può essere analizzato da IIS. Ciò impedisce a Gestione IIS di caricare la configurazione del sito Web e al sito Web di fornire i file statici di Blazor.

Per altre informazioni sulla risoluzione dei problemi relativi alle distribuzioni in IIS, vedere <xref:host-and-deploy/iis/troubleshoot>.

### <a name="nginx"></a>Nginx

Il file *nginx.conf* seguente è semplificato per mostrare come configurare Nginx per l'invio del file *index.html* ogni volta che non riesce a trovare un file su disco corrispondente.

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /index.html =404;
        }
    }
}
```

Per altre informazioni sulla configurazione del server Web Nginx in ambiente di produzione, vedere [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/) (Creazione di file di configurazione NGINX Plus e NGINX).

### <a name="nginx-in-docker"></a>Nginx in Docker

Per ospitare Blazor in Docker usando Nginx, configurare il Dockerfile per l'uso dell'immagine di Nginx basata su Alpine. Aggiornare il Dockerfile per copiare il file *nginx.config* nel contenitore.

Aggiungere una riga al Dockerfile, come illustrato nell'esempio seguente:

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="github-pages"></a>Pagine di GitHub

Per gestire le riscritture degli URL, aggiungere un file *404.html* con uno script che gestisce il reindirizzamento della richiesta alla pagina *index.html*. Per un esempio di implementazione fornito dalla community, vedere [Single Page Apps for GitHub Pages](http://spa-github-pages.rafrex.com/) (App a pagina singola per pagine GitHub) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)). È possibile visualizzare un esempio d'uso dell'approccio della community in [blazor-demo/blazor-demo.github.io su GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([sito live](https://blazor-demo.github.io/)).

Quando si usa un sito di progetto anziché un sito dell'organizzazione, aggiungere o aggiornare il tag `<base>` in *index.html*. Impostare il valore dell'attributo `href` sul nome del repository GitHub con una barra finale (ad esempio, `my-repository/`.
