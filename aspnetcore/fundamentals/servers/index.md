---
title: Implementazioni di server Web in ASP.NET Core
author: tdykstra
description: Introduce i server Web Kestrel e WebListener per ASP.NET Core. Offre indicazioni su come scegliere uno dei server e quando usarlo con un server proxy inverso.
manager: wpickett
ms.author: tdykstra
ms.date: 08/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/index
ms.openlocfilehash: 9e2bea396e50615bd02affad93f0ee55255d299f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="web-server-implementations-in-aspnet-core"></a>Implementazioni di server Web in ASP.NET Core

Di [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) e [Chris Ross](https://github.com/Tratcher)

Un'applicazione ASP.NET Core viene eseguita con un'implementazione del server HTTP in-process. L'implementazione del server attende le richieste HTTP e le rende visibili all'applicazione come set di [funzionalità di richiesta](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) combinate in un oggetto `HttpContext`.

Ad ASP.NET Core sono accluse due implementazioni di server:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [Kestrel](kestrel.md) è un server HTTP multipiattaforma basato su [libuv](https://github.com/libuv/libuv), una libreria di I/O asincrona multipiattaforma.

* [HTTP.sys](httpsys.md) è un server HTTP solo per Windows basato sul [driver del kernel Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [Kestrel](kestrel.md) è un server HTTP multipiattaforma basato su [libuv](https://github.com/libuv/libuv), una libreria di I/O asincrona multipiattaforma.

* [WebListener](weblistener.md) è un server HTTP solo per Windows basato sul [driver del kernel Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

---

## <a name="kestrel"></a>Kestrel

Kestrel è il server Web incluso per impostazione predefinita nei modelli di nuovo progetto di ASP.NET Core. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

È possibile usare Kestrel da solo o con un *server proxy inverso*, ad esempio IIS, Nginx o Apache. Un server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel dopo alcune operazioni di gestione preliminari.

![Kestrel comunica direttamente con Internet senza un server proxy inverso](kestrel/_static/kestrel-to-internet2.png)

![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

È possibile usare una delle due configurazioni &mdash; con o senza un server proxy inverso &mdash; anche se Kestrel viene esposto solo a una rete interna.

Per informazioni su quando usare Kestrel con un proxy inverso, vedere l'[introduzione a Kestrel](kestrel.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Se l'applicazione accetta le richieste solo da una rete interna, è possibile usare Kestrel da solo.

![Kestrel comunica direttamente con la rete interna](kestrel/_static/kestrel-to-internal.png)

Se si espone l'applicazione a Internet, è necessario usare IIS, Nginx o Apache come *server proxy inverso*. Un server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel dopo alcune operazioni di gestione preliminari, come illustrato nel diagramma che segue.

![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

Il motivo principale per cui si usa un proxy inverso per le distribuzioni perimetrali, ovvero esposte al traffico da Internet, è la sicurezza. Nelle versioni 1. x di Kestrel non è disponibile una serie completa di difese contro gli attacchi. Questo include, senza limitazioni, timeout appropriati, limiti delle dimensioni e limiti delle connessioni simultanee.

Per informazioni su quando usare Kestrel con un proxy inverso, vedere l'[introduzione a Kestrel](kestrel.md).

---

Non è possibile usare IIS, Nginx o Apache senza Kestrel o un'[implementazione del server personalizzata](#custom-servers). ASP.NET Core è stato progettato per essere eseguito nel proprio processo in modo che il comportamento sia coerente tra le piattaforme. IIS, Nginx e Apache impongono il proprio processo di avvio e il proprio ambiente: per usarli direttamente, ASP.NET Core dovrebbe adattarsi alle esigenze di ognuno di essi. L'uso di un'implementazione del server Web come Kestrel consente ad ASP.NET Core di controllare il processo di avvio e l'ambiente. Quindi, anziché tentare di adattare ASP.NET Core a IIS, Nginx o Apache, è sufficiente impostare i server Web in modo che inoltrino le richieste a Kestrel. In questo modo le classi `Program.Main` e `Startup` possono essere essenzialmente le stesse indipendentemente da dove si esegue la distribuzione.

### <a name="iis-with-kestrel"></a>IIS con Kestrel

Quando si usa IIS o IIS Express come proxy inverso per ASP.NET Core, l'applicazione ASP.NET Core viene eseguita in un processo separato dal processo di lavoro di IIS. Nel processo di IIS viene eseguito un modulo IIS speciale per coordinare la relazione di proxy inverso.  Si tratta del *modulo ASP.NET Core*. Le funzioni principali del modulo ASP.NET Core sono l'avvio dell'applicazione ASP.NET Core, il riavvio dell'applicazione quando si blocca e l'inoltro del traffico HTTP. Per altre informazioni, vedere l'[introduzione al modulo ASP.NET Core](aspnet-core-module.md). 

### <a name="nginx-with-kestrel"></a>Nginx con Kestrel

Per informazioni sull'uso di Nginx in Linux come server proxy inverso per Kestrel, vedere [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx) (Hosting in Linux con Nginx).

### <a name="apache-with-kestrel"></a>Apache con Kestrel

Per informazioni sull'uso di Apache in Linux come server proxy inverso per Kestrel, vedere [Host on Linux with Apache](xref:host-and-deploy/linux-apache) (Hosting in Linux con Apache).

## <a name="httpsys"></a>HTTP.sys

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Se si esegue l'app ASP.NET Core in Windows, HTTP.sys è un'alternativa a Kestrel. È possibile usare HTTP.sys per scenari in cui si espone l'app a Internet e sono necessarie funzionalità di HTTP.sys che Kestrel non supporta. 

![HTTP.sys comunica direttamente con Internet](httpsys/_static/httpsys-to-internet.png)

HTTP.sys può essere usato anche per le applicazioni che vengono esposte solo a una rete interna. 

![HTTP.sys comunica direttamente con la rete interna](httpsys/_static/httpsys-to-internal.png)

Negli scenari con rete interna, in genere si consiglia Kestrel perché offre prestazioni ottimali, ma in alcuni scenari può essere preferibile una funzionalità offerta solo da HTTP.sys. Per informazioni sulle funzionalità di HTTP.sys, vedere [HTTP.sys](httpsys.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

HTTP.sys è denominato WebListener in ASP.NET Core 1.x. Se si esegue l'app ASP.NET Core in Windows, WebListener è un'alternativa che è possibile usare in scenari in cui si vuole esporre l'app a Internet ma non si può usare IIS.

![WebListener comunica direttamente con Internet](weblistener/_static/weblistener-to-internet.png)

WebListener può essere usato anche al posto di Kestrel per le applicazioni che vengono esposte solo a una rete interna, se sono necessarie funzionalità di WebListener non supportate da Kestrel. 

![Weblistener comunica direttamente con la rete interna](weblistener/_static/weblistener-to-internal.png)

Negli scenari con rete interna, in genere si consiglia Kestrel perché offre prestazioni ottimali, ma in alcuni scenari può essere preferibile una funzionalità offerta solo da WebListener. Per informazioni sulle funzionalità di WebListener, vedere [WebListener](weblistener.md).

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a>Note sull'infrastruttura del server ASP.NET Core

L'oggetto [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder), disponibile nella classe `Startup` all'interno del metodo `Configure`, espone la proprietà `ServerFeatures` del tipo [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection). Kestrel e WebListener espongono entrambi solo una singola funzionalità, [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), ma le diverse implementazioni del server possono esporre funzionalità aggiuntive.

L'oggetto `IServerAddressesFeature` può essere usato per individuare la porta a cui è associata l'implementazione del server in fase di esecuzione.

## <a name="custom-servers"></a>Server personalizzati

Se i server predefiniti non soddisfano le proprie esigenze, è possibile creare un'implementazione del server personalizzata. La [guida all'interfaccia OWIN (Open Web Interface for .NET)](../owin.md) spiega come scrivere un'implementazione [Nowin](https://github.com/Bobris/Nowin) di [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver). Si ha la possibilità di implementare solo le interfacce di funzionalità richieste dall'applicazione, anche se è necessario il supporto almeno per [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) e [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere le seguenti risorse:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

- [Kestrel](kestrel.md)
- [Kestrel con IIS](aspnet-core-module.md)
- [Host in Linux con Nginx](xref:host-and-deploy/linux-nginx)
- [Host in Linux con Apache](xref:host-and-deploy/linux-apache)
- [HTTP.sys](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

- [Kestrel](kestrel.md)
- [Kestrel con IIS](aspnet-core-module.md)
- [Host in Linux con Nginx](xref:host-and-deploy/linux-nginx)
- [Host in Linux con Apache](xref:host-and-deploy/linux-apache)
- [WebListener](weblistener.md)

---
