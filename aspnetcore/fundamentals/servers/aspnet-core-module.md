---
title: Modulo ASP.NET Core
author: tdykstra
description: In questo articolo viene descritto il modulo ASP.NET Core, un modulo IIS che consente al server Web Kestrel di usare IIS o IIS Express come server proxy inverso.
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 4337bc42c5454d6a9634a396d9c89f3518af148b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-aspnet-core-module"></a>Introduzione al modulo ASP.NET Core

Di [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) e [Chris Ross](https://github.com/Tratcher) 

Il modulo ASP.NET Core consente di eseguire le applicazioni ASP.NET Core in IIS, sfruttando i punti di forza di IIS (sicurezza, gestibilità e altro ancora) e quelli di [Kestrel](kestrel.md) (velocità) e offrendo i vantaggi di entrambe le tecnologie in un'unica soluzione. **Il modulo ASP.NET Core funziona solo con Kestrel; non è compatibile con WebListener (in ASP.NET Core 1. x) o HTTP.sys (in 2. x).** 

Versioni di Windows supportate:

* Windows 7 e Windows Server 2008 R2 e versioni successive

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-aspnet-core-module-does"></a>Funzionamento del modulo ASP.NET Core

Il modulo ASP.NET Core è un modulo nativo di IIS che si associa alla pipeline di IIS e reindirizza il traffico nell'applicazione ASP.NET Core di back-end. La maggior parte degli altri moduli, ad esempio l'autenticazione di Windows, continua comunque a essere eseguita. Il modulo ASP.NET Core assume il controllo solo quando viene selezionato un gestore per la richiesta e il mapping del gestore è definito nel file *web.config* dell'applicazione.

Poiché le applicazioni ASP.NET Core vengono eseguite in un processo distinto dal processo di lavoro IIS, il modulo ASP.NET Core esegue anche l'elaborazione dei processi. Il modulo ASP.NET Core avvia il processo per l'applicazione ASP.NET Core quando si presenta la prima richiesta e lo riavvia quando si verifica un arresto anomalo del sistema. Si tratta essenzialmente dello stesso comportamento delle applicazioni ASP.NET classiche che vengono eseguite in-process in IIS e sono gestite da WAS (Windows Activation Service).

Di seguito è riportato un diagramma che illustra la relazione tra IIS, il modulo ASP.NET Core e le applicazioni di ASP.NET Core.

![Modulo ASP.NET Core](aspnet-core-module/_static/ancm.png)

Le richieste arrivano dal Web e raggiungono il driver Http.Sys in modalità kernel che le indirizza in IIS sulla porta primaria (80) o su una porta SSL (443). Il modulo ASP.NET Core inoltra le richieste all'applicazione ASP.NET Core sulla porta HTTP configurata per l'applicazione, che non è la porta 80/443.

Kestrel è in ascolto del traffico proveniente dal modulo ASP.NET Core.  Il modulo ASP.NET Core specifica la porta tramite la variabile di ambiente all'avvio e il metodo [UseIISIntegration](#call-useiisintegration) configura il server per l'ascolto su `http://localhost:{port}`. Sono disponibili controlli aggiuntivi per rifiutare le richieste che non provengono dal modulo ASP.NET Core. (Il modulo ASP.NET Core non supporta l'inoltro HTTPS, pertanto le richieste vengono inoltrate tramite HTTP anche se ricevute da IIS tramite HTTPS.)

Kestrel preleva le richieste dal modulo ASP.NET Core e le inserisce nella pipeline del middleware di ASP.NET Core, che le gestisce e le invia come istanze `HttpContext` alla logica dell'applicazione. Le risposte dell'applicazione vengono quindi passate a IIS, che le invia nuovamente al client HTTP che ha avviato le richieste.

Il modulo ASP.NET Core presenta anche alcune altre funzioni:

* Consente di impostare le variabili di ambiente.
* Consente di registrare l'output `stdout` nell'archiviazione file.
* Consente di inoltrare i token di autenticazione di Windows.

## <a name="how-to-use-ancm-in-aspnet-core-apps"></a>Come usare il modulo ASP.NET Core nelle applicazioni ASP.NET Core

In questa sezione viene descritta una panoramica del processo di configurazione di un server IIS e dell'applicazione ASP.NET Core. Per istruzioni dettagliate, vedere [Host in Windows con IIS](xref:host-and-deploy/iis/index).

### <a name="install-ancm"></a>Installare il modulo ASP.NET Core


Il modulo ASP.NET Core deve essere installato in IIS nei server e in IIS Express nei computer di sviluppo. Per i server, il modulo ASP.NET Core è incluso nell'[aggregazione di Hosting di .NET Core Windows Server ](https://aka.ms/dotnetcore-2-windowshosting). Per i computer di sviluppo, Visual Studio installa automaticamente il modulo ASP.NET Core in IIS Express e in IIS se è già installato nel computer.

### <a name="install-the-iisintegration-nuget-package"></a>Installare il pacchetto NuGet IISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Il pacchetto [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) è incluso nei metapacchetti di ASP.NET Core ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/) e [Microsoft.AspNetCore.All](xref:fundamentals/metapackage)). Se non viene usato uno dei metapacchetti, installare `Microsoft.AspNetCore.Server.IISIntegration` separatamente. Il pacchetto `IISIntegration` è un pacchetto di interoperabilità che legge le variabili di ambiente trasmesse dal modulo ASP.NET Core per impostare l'app. Le variabili di ambiente contengono informazioni di configurazione, ad esempio la porta per l'ascolto. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Installare [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) nell'applicazione. Il pacchetto `IISIntegration` è un pacchetto di interoperabilità che legge le variabili di ambiente trasmesse dal modulo ASP.NET Core per impostare l'app. Le variabili di ambiente contengono informazioni di configurazione, ad esempio la porta per l'ascolto. 

---

### <a name="call-useiisintegration"></a>Chiamare UseIISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Il metodo di estensione `UseIISIntegration` su [`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) viene chiamato automaticamente durante l'esecuzione con IIS.

Se non viene usato uno dei metapacchetti di ASP.NET Core e non è stato installato il pacchetto `Microsoft.AspNetCore.Server.IISIntegration`, si riceve un errore di runtime. Se si chiama `UseIISIntegration` in modo esplicito, si riceve un errore in fase di compilazione se il pacchetto non è installato.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Nel metodo `Main` dell'applicazione chiamare il metodo di estensione `UseIISIntegration` su [`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder). 

[!code-csharp[](aspnet-core-module/sample/Program.cs?name=snippet_Main&highlight=12)]

---

Il metodo `UseIISIntegration` esegue la ricerca delle variabili di ambiente impostate dal modulo ASP.NET Core e non esegue alcuna operazione se non vengono trovate. Questo comportamento semplifica gli scenari come lo sviluppo e il test in macOS o Linux e la distribuzione a un server che esegue IIS. Nell'esecuzione in macOS o Linux, Kestrel funge da server Web. L'applicazione usa automaticamente il modulo ASP.NET Core e IIS quando viene distribuita nell'ambiente IIS.

### <a name="ancm-port-binding-overrides-other-port-bindings"></a>Il binding porta del modulo ASP.NET Core sostituisce gli altri binding delle porte

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Il modulo ASP.NET Core genera una porta dinamica da assegnare al processo di back-end. Il metodo `UseIISIntegration` preleva tale porta dinamica e configura Kestrel per l'ascolto su `http://locahost:{dynamicPort}/`. Questa operazione sostituisce le altre configurazioni URL, come chiamate a `UseUrls` o [API Listen di Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration). Non è pertanto necessario chiamare `UseUrls` o l'API `Listen` di Kestrel quando si usa il modulo ASP.NET Core. Se si chiama `UseUrls` o `Listen`, Kestrel è in ascolto sulla porta specificata quando si esegue l'app senza IIS.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Il modulo ASP.NET Core genera una porta dinamica da assegnare al processo di back-end. Il metodo `UseIISIntegration` preleva tale porta dinamica e configura Kestrel per l'ascolto su `http://locahost:{dynamicPort}/`. Questa operazione sostituisce le altre configurazioni URL, come chiamate a `UseUrls`. Non è pertanto necessario chiamare `UseUrls` quando si usa il modulo ASP.NET Core. Se si chiama `UseUrls`, Kestrel è in ascolto sulla porta specificata quando si esegue l'app senza IIS.

In ASP.NET Core 1.0, se si chiama `UseUrls`, chiamarlo **prima** di chiamare `UseIISIntegration` in modo che la porta configurata dal modulo ASP.NET Core non venga sovrascritta. Questo ordine di chiamata non è obbligatorio in ASP.NET 1.1 Core perché l'impostazione del modulo ASP.NET Core sostituisce `UseUrls`.

---

### <a name="configure-ancm-options-in-webconfig"></a>Configurare le opzioni del modulo ASP.NET Core in Web.config

La configurazione per il modulo ASP.NET Core viene archiviata nel file *web.config* che si trova nella cartella radice dell'applicazione. Le impostazioni di questo file puntano al comando di avvio e agli argomenti che avviano l'app ASP.NET Core. Per un esempio di codice *web.config* e il materiale sussidiario sulle opzioni di configurazione, vedere [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

### <a name="run-with-iis-express-in-development"></a>Esecuzione con IIS Express in fase di sviluppo

IIS Express può essere avviato da Visual Studio usando il profilo predefinito specificato tramite i modelli ASP.NET Core.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>La configurazione del proxy usa il protocollo HTTP e un token di associazione

Il proxy creato tra il modulo ASP.NET Core e Kestrel usa il protocollo HTTP. L'uso di HTTP è un'ottimizzazione delle prestazioni in cui il traffico tra il modulo ASP.Net Core e Kestrel avviene in un indirizzo di loopback dell'interfaccia di rete. Non vi è alcun rischio di intercettazione del traffico tra il modulo ASP.Net Core e Kestrel da una posizione esterna al server.

Viene usato un token di associazione per garantire che le richieste ricevute da Kestrel siano state trasmesse tramite proxy da IIS e non provengano da un'altra origine. Il token di associazione viene creato e impostato in una variabile di ambiente (`ASPNETCORE_TOKEN`) dal modulo ASP.Net Core. Il token di associazione viene impostato anche in un'intestazione (`MSAspNetCoreToken`) in tutte le richieste trasmesse tramite proxy. Il middleware di IIS controlla ogni richiesta che riceve in modo da confermare che il valore dell'intestazione del token di associazione corrisponda al valore della variabile di ambiente. Se i valori del token non corrispondono, la richiesta viene registrata e rifiutata. La variabile di ambiente del token di associazione e il traffico tra il modulo ASP.Net Core e Kestrel non sono accessibili da una posizione esterna al server. Senza conoscere il valore del token di associazione, un utente malintenzionato non può inviare richieste che ignorano il controllo nel middleware di IIS.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere le seguenti risorse:

* [App di esempio per questo articolo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)
* [Codice sorgente del modulo ASP.NET Core](https://github.com/aspnet/AspNetCoreModule)
* [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Host in Windows con IIS](xref:host-and-deploy/iis/index)
