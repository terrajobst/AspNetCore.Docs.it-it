---
title: Implementazioni di server Web in ASP.NET Core
author: guardrex
description: Individuare i server Web Kestrel e HTTP.sys per ASP.NET Core. Informazioni su come scegliere un server e quando usare un server proxy inverso.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 06d4bf09b07fc70a10b3e260e78c29fe189486c5
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505726"
---
# <a name="web-server-implementations-in-aspnet-core"></a>Implementazioni di server Web in ASP.NET Core

Di [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) e [Chris Ross](https://github.com/Tratcher)

Un'app ASP.NET Core viene eseguita con un'implementazione del server HTTP in-process. L'implementazione del server attende le richieste HTTP e le rende visibili all'app come set di [funzionalità di richiesta](xref:fundamentals/request-features) combinate in un <xref:Microsoft.AspNetCore.Http.HttpContext>.

ASP.NET Core viene fornito con le implementazioni server seguenti:

::: moniker range=">= aspnetcore-2.2"

* [Kestrel](xref:fundamentals/servers/kestrel) è il server HTTP multipiattaforma predefinito per ASP.NET Core.
* `IISHttpServer` viene usato con il [modello di hosting in-process](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) e il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) in Windows.
* [HTTP.sys](xref:fundamentals/servers/httpsys) è un server HTTP solo per Windows basato sul [driver del kernel HTTP.Sys e l'API HTTP Server](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). (HTTP.sys è denominato [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [Kestrel](xref:fundamentals/servers/kestrel) è il server HTTP multipiattaforma predefinito per ASP.NET Core.
* [HTTP.sys](xref:fundamentals/servers/httpsys) è un server HTTP solo per Windows basato sul [driver del kernel HTTP.Sys e l'API HTTP Server](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). (HTTP.sys è denominato [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)

::: moniker-end

## <a name="kestrel"></a>Kestrel

Kestrel è il server Web predefinito incluso nei modelli di progetto di ASP.NET Core.

::: moniker range=">= aspnetcore-2.0"

È possibile usare Kestrel da solo o con un *server proxy inverso*, ad esempio IIS, Nginx o Apache. Un server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel dopo alcune operazioni di gestione preliminari.

![Kestrel comunica direttamente con Internet senza un server proxy inverso](kestrel/_static/kestrel-to-internet2.png)

![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

Entrambe le configurazioni (con o senza un server proxy inverso) sono configurazioni di hosting valide e supportate per le app ASP.NET Core 2.0 o versioni successive. Per altre informazioni, vedere [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Quando usare Kestrel con un proxy inverso).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Se l'app accetta solo le richieste provenienti da una rete interna, Kestrel è utilizzabile in modo autonomo.

![Kestrel comunica direttamente con la rete interna](kestrel/_static/kestrel-to-internal.png)

Se si espone l'app a Internet, Kestrel deve usare IIS, Nginx o Apache come *server proxy inverso*. Un server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel dopo alcune operazioni di gestione preliminari, come illustrato nel diagramma che segue:

![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

Il motivo principale per cui si usa un proxy inverso per le distribuzioni server perimetrali pubbliche, esposte direttamente a Internet, è la sicurezza. Le versioni 1.x di Kestrel non includono funzionalità di sicurezza importanti per difendersi da attacchi da Internet, ad esempio timeout appropriati, limiti delle dimensioni delle richieste e limiti delle connessioni simultanee.

Per altre informazioni, vedere [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Quando usare Kestrel con un proxy inverso).

::: moniker-end

Non è possibile usare IIS, Nginx e Apache senza Kestrel o un'[implementazione del server personalizzata](#custom-servers). ASP.NET Core è stato progettato per essere eseguito nel proprio processo in modo che il comportamento sia coerente tra le piattaforme. IIS, Nginx e Apache stabiliscono procedure di avvio e ambiente propri. Per usare queste tecnologie server direttamente, ASP.NET Core dovrà adattarsi ai requisiti di ogni server. L'uso di un'implementazione del server Web come Kestrel consente ad ASP.NET Core di controllare il processo di avvio e l'ambiente quando viene ospitato in tecnologie server diverse.

### <a name="iis-with-kestrel"></a>IIS con Kestrel

::: moniker range=">= aspnetcore-2.2"

Se si usa [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), l'app ASP.NET Core può essere eseguita nello stesso processo del processo di lavoro IIS (modello di hosting *in-process*) o in un processo separato dal processo di lavoro IIS (modello di hosting *out-of-process*).

Il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) è un modulo IIS nativo che gestisce le richieste IIS native con il server HTTP dell'app IIS in-process o il server Kestrel out-of-process. Per ulteriori informazioni, vedere <xref:fundamentals/servers/aspnet-core-module>.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Quando si usa [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) come proxy inverso per ASP.NET Core, l'app ASP.NET Core viene eseguita in un processo separato dal processo di lavoro di IIS. Nel processo di IIS, il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) coordina la relazione di proxy inverso. Le funzioni principali del modulo ASP.NET Core sono l'avvio dell'app ASP.NET Core, il riavvio dell'app quando si blocca e l'inoltro del traffico HTTP all'app. Per ulteriori informazioni, vedere <xref:fundamentals/servers/aspnet-core-module>.

::: moniker-end

### <a name="nginx-with-kestrel"></a>Nginx con Kestrel

Per informazioni su come usare Nginx in Linux come server proxy inverso per Kestrel, vedere [Host in Linux con Nginx](xref:host-and-deploy/linux-nginx).

### <a name="apache-with-kestrel"></a>Apache con Kestrel

Per informazioni su come usare Apache in Linux come server proxy inverso per Kestrel, vedere [Host in Linux con Apache](xref:host-and-deploy/linux-apache).

## <a name="httpsys"></a>HTTP.sys

::: moniker range=">= aspnetcore-2.0"

Se si eseguono app ASP.NET Core in Windows, HTTP.sys è un'alternativa a Kestrel. Kestrel è in genere consigliato per ottenere prestazioni ottimali. HTTP.sys è utilizzabile negli scenari in cui l'app viene esposta a Internet e le funzionalità necessarie sono supportate da HTTP.sys, ma non da Kestrel. Per informazioni su HTTP.sys, vedere [HTTP.sys](xref:fundamentals/servers/httpsys).

![HTTP.sys comunica direttamente con Internet](httpsys/_static/httpsys-to-internet.png)

HTTP.sys può essere usato anche per le app che vengono esposte solo a una rete interna.

![HTTP.sys comunica direttamente con la rete interna](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

HTTP.sys è denominato [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x. Se le app ASP.NET Core vengono eseguite in Windows, WebListener rappresenta un'alternativa per gli scenari in cui IIS non è disponibile per l'hosting delle app.

![WebListener comunica direttamente con Internet](weblistener/_static/weblistener-to-internet.png)

WebListener può essere usato anche al posto di Kestrel per le app che vengono esposte solo a una rete interna, se le funzionalità necessarie sono supportate da WebListener ma non da Kestrel. Per informazioni su WebListener, vedere [WebListener](xref:fundamentals/servers/weblistener).

![Weblistener comunica direttamente con la rete interna](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a>Infrastruttura del server ASP.NET Core

[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder), disponibile nel metodo `Startup.Configure`, espone la proprietà [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) di tipo [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection). Kestrel e HTTP. sys (WebListener in ASP.NET Core 1.x) espongono solo una singola funzionalità ognuno, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), ma implementazioni server diverse potrebbero esporre funzionalità aggiuntive.

È possibile usare `IServerAddressesFeature` per individuare la porta a cui è associata l'implementazione server in fase di esecuzione.

## <a name="custom-servers"></a>Server personalizzati

Se i server predefiniti non soddisfano i requisiti dell'app, è possibile creare un'implementazione server personalizzata. La [guida all'interfaccia OWIN (Open Web Interface for .NET)](xref:fundamentals/owin) spiega come scrivere un'implementazione [Nowin](https://github.com/Bobris/Nowin) di [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver). Solo le interfacce delle funzionalità usate dall'app devono essere implementate, anche se è richiesto come minimo il supporto di [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) e [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).

## <a name="server-startup"></a>Avvio del server

Quando si usa [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio per Mac](https://www.visualstudio.com/vs/mac/) o [Visual Studio Code](https://code.visualstudio.com/), il server viene avviato all'avvio dell'app dall'ambiente di sviluppo integrato (IDE). In Visual Studio in Windows, è possibile usare i profili di avvio per avviare l'app e il server con [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[Modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) o la console. In Visual Studio Code, l'app e il server vengono avviati da [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), che attiva il debugger CoreCLR. In Visual Studio per Mac, l'app e il server vengono avviati mediante il [debugger in modalità Mono Soft](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).

All'avvio di un'app dal prompt dei comandi nella cartella del progetto, [dotnet run](/dotnet/core/tools/dotnet-run) avvia l'app e il server (solo Kestrel e HTTP.sys). La configurazione viene specificata dall'opzione `-c|--configuration`, impostata su `Debug` (valore predefinito) o su `Release`. Se sono presenti profili di avvio in un file *launchSettings.json*, usare l'opzione `--launch-profile <NAME>` per impostare il profilo di avvio (ad esempio, `Development` o `Production`). Per altre informazioni, vedere gli argomenti [dotnet run](/dotnet/core/tools/dotnet-run) e [Creazione di pacchetti di distribuzione di .NET Core](/dotnet/core/build/distribution-packaging).

## <a name="http2-support"></a>Supporto per HTTP/2

[HTTP/2](https://httpwg.org/specs/rfc7540.html) è supportato con ASP.NET Core negli scenari di distribuzione seguenti:

::: moniker range=">= aspnetcore-2.2"

* [Kestrel](xref:fundamentals/servers/kestrel#http2-support)
  * Sistema operativo
    * Windows Server 2016/Windows 10 o versioni successive&dagger;
    * Linux con OpenSSL 1.0.2 o versioni successive (ad esempio, Ubuntu 16.04 o versioni successive)
    * HTTP/2 verrà supportato in macOS in una versione futura.
  * Framework di destinazione: .NET Core 2.2 o versioni successive
* [HTTP.sys](xref:fundamentals/servers/httpsys#http2-support)
  * Windows Server 2016/Windows 10 o versioni successive
  * Framework di destinazione: non applicabile alle distribuzioni HTTP.sys.
* [IIS (in-process)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 o versioni successive; IIS 10 o versioni successive
  * Framework di destinazione: .NET Core 2.2 o versioni successive
* [IIS (out-of-process)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 o versioni successive; IIS 10 o versioni successive
  * Le connessioni server perimetrali pubbliche usano HTTP/2, mentre la connessione con proxy inverso a Kestrel usa HTTP/1.1.
  * Framework di destinazione: non applicabile alle distribuzioni IIS out-of-process.

&dagger;Kestrel ha un supporto limitato per HTTP/2 in Windows Server 2012 R2 e Windows 8.1. Il supporto è limitato perché l'elenco di suite di crittografia TLS supportate disponibili in questi sistemi operativi è limitato. Un certificato generato con un algoritmo ECDSA potrebbe essere necessario per proteggere le connessioni TLS.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [HTTP.sys](xref:fundamentals/servers/httpsys#http2-support)
  * Windows Server 2016/Windows 10 o versioni successive
  * Framework di destinazione: non applicabile alle distribuzioni HTTP.sys.
* [IIS (out-of-process)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 o versioni successive; IIS 10 o versioni successive
  * Le connessioni server perimetrali pubbliche usano HTTP/2, mentre la connessione con proxy inverso a Kestrel usa HTTP/1.1.
  * Framework di destinazione: non applicabile alle distribuzioni IIS out-of-process.

::: moniker-end

Una connessione HTTP/2 deve usare [ALPN (Application-Layer Protocol Negotiation)](https://tools.ietf.org/html/rfc7301#section-3) e TLS 1.2 o versioni successive. Per altre informazioni, vedere gli argomenti relativi agli scenari di distribuzione server.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:fundamentals/servers/httpsys> (per ASP.NET Core 1.x, vedere <xref:fundamentals/servers/weblistener>)
