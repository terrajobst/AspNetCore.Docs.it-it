---
title: ASP.NET Core Module
author: tdykstra
description: Introduce ASP.NET Core modulo (ANCM), un modulo IIS che consente al server web Kestrel utilizzare IIS o IIS Express come server proxy inverso.
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/aspnet-core-module
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9dc2183ebbdf8b74106fe57a1dd191a57ba5d1bc
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-aspnet-core-module"></a>Introduzione al modulo ASP.NET Core

Da [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), e [Chris Ross](https://github.com/Tratcher) 

Modulo Core ASP.NET (ANCM) consente di eseguire le applicazioni protetti da IIS, ASP.NET Core utilizzando IIS è bene (sicurezza, gestibilità e altro ancora) e [Kestrel](kestrel.md) è bene (da velocemente) e il recupero di vantaggi di entrambe le tecnologie in una sola volta. **ANCM funziona solo con Kestrel; non è compatibile con WebListener (in ASP.NET Core 1. x) o HTTP.sys (in 2. x).** 

Versioni supportate di Windows:

* Windows 7 e Windows Server 2008 R2 e versioni successive

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-aspnet-core-module-does"></a>Funzionamento di ASP.NET Core modulo

ANCM è un modulo nativo di IIS che si associa alla pipeline di IIS e reindirizza il traffico nel back-end dell'applicazione ASP.NET Core. La maggior parte degli altri moduli, ad esempio l'autenticazione di windows, è comunque essere eseguito. ANCM assume il controllo solo quando viene selezionato un gestore per la richiesta e mapping del gestore è definito nell'applicazione *Web. config* file.

Poiché le applicazioni ASP.NET Core eseguite in un processo diversi dal processo di lavoro IIS, ANCM anche dei processi. Quando la prima richiesta in riavviarlo quando si blocca, ANCM avvia il processo per l'applicazione ASP.NET Core. Si tratta essenzialmente lo stesso comportamento di applicazioni ASP.NET classiche che eseguono e in-process in IIS sono gestite da WAS (Windows Activation Service).

Di seguito è riportato un diagramma che illustra la relazione tra le applicazioni di IIS, ANCM e ASP.NET Core.

![ASP.NET Core Module](aspnet-core-module/_static/ancm.png)

Le richieste di entrare dal Web e raggiunge il driver Http.Sys in modalità kernel che li indirizza in IIS in cui la porta principale (80) o una porta SSL (443). ANCM inoltra le richieste all'applicazione ASP.NET di base sulla porta HTTP configurata per l'applicazione, che non è la porta 80/443.

Kestrel in ascolto del traffico proveniente da ANCM.  ANCM specifica la porta tramite la variabile di ambiente all'avvio e [UseIISIntegration](#call-useiisintegration) metodo consente di configurare il server per l'ascolto su `http://localhost:{port}`. Sono disponibili ulteriori controlli per rifiutare la richiesta non ANCM. (ANCM non supporta l'inoltro HTTPS, pertanto le richieste vengono inoltrate tramite HTTP, anche se ricevuto da IIS tramite HTTPS.)

Kestrel preleva le richieste dal ANCM e li inserisce nella pipeline di middleware di ASP.NET Core, che quindi vengono gestiti e li invia come `HttpContext` istanze per la logica dell'applicazione. Le risposte dell'applicazione vengono quindi passate a IIS, quali inserimenti li tornare indietro al client che ha avviato le richieste HTTP.

ANCM presenta alcune altre funzioni:

* Imposta le variabili di ambiente.
* Registri `stdout` all'archiviazione file di output.
* Inoltra il token di autenticazione di Windows.

## <a name="how-to-use-ancm-in-aspnet-core-apps"></a>Come usare ANCM nelle applicazioni ASP.NET Core

In questa sezione viene fornita una panoramica del processo per la configurazione di un server IIS e l'applicazione ASP.NET Core. Per istruzioni dettagliate, vedere [Host su Windows con IIS](xref:host-and-deploy/iis/index).

### <a name="install-ancm"></a>Installare ANCM


Il modulo di base di ASP.NET deve essere installato in IIS nei server e in IIS Express nel computer di sviluppo. Per i server, ANCM è incluso nel [.NET Core Windows Server che ospita bundle](https://aka.ms/dotnetcore-2-windowshosting). Per i computer di sviluppo, Visual Studio installa automaticamente ANCM in IIS Express in IIS se è già installato nel computer.

### <a name="install-the-iisintegration-nuget-package"></a>Installare il pacchetto IISIntegration NuGet

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Il [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) pacchetto è incluso in metapackages di ASP.NET Core ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/) e [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ). Se non si utilizza uno del metapackages, installare `Microsoft.AspNetCore.Server.IISIntegration` separatamente. Il `IISIntegration` pacchetto è un pacchetto di interoperabilità che vengono lette le variabili di ambiente da ANCM per impostare l'app. Le variabili di ambiente è fornire informazioni di configurazione, ad esempio la porta per l'ascolto. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Nell'applicazione, installare [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/). Il `IISIntegration` pacchetto è un pacchetto di interoperabilità che vengono lette le variabili di ambiente da ANCM per impostare l'app. Le variabili di ambiente è fornire informazioni di configurazione, ad esempio la porta per l'ascolto. 

---

### <a name="call-useiisintegration"></a>Chiamare UseIISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Il `UseIISIntegration` metodo di estensione su [ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) viene chiamato automaticamente quando si esegue con IIS.

Se si non utilizzando uno dei metapackages di ASP.NET Core e non sono stati installati il `Microsoft.AspNetCore.Server.IISIntegration` pacchetto, si ottiene un errore di runtime. Se si chiama `UseIISIntegration` in modo esplicito, ottenere un errore in fase di compilazione se non è installato il pacchetto.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

In un'applicazione `Main` metodo, chiamare il `UseIISIntegration` metodo di estensione su [ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder). 

[!code-csharp[](aspnet-core-module/sample/Program.cs?name=snippet_Main&highlight=12)]

---

Il `UseIISIntegration` metodo esegue la ricerca per le variabili di ambiente che imposta ANCM e no-ops se essi non vengono trovate. Questo comportamento semplifica gli scenari quali lo sviluppo e test in macOS o Linux e la distribuzione in un server che esegue IIS. Durante l'esecuzione in macOS o Linux, Kestrel funge da server web. Tuttavia, quando l'applicazione viene distribuita nell'ambiente di IIS, viene automaticamente ANCM e IIS.

### <a name="ancm-port-binding-overrides-other-port-bindings"></a>Binding porta ANCM esegue l'override di altri binding delle porte

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ANCM genera una porta dinamica da assegnare al processo di back-end. Il `UseIISIntegration` metodo preleva la porta dinamica e configura Kestrel per l'ascolto su `http://locahost:{dynamicPort}/`. Si esegue l'override di altre configurazioni di URL, quali chiamate a `UseUrls` o [API del Kestrel di ascolto](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration). Pertanto, non è necessario chiamare `UseUrls` o del Kestrel `Listen` API quando si utilizza ANCM. Se si chiama `UseUrls` o `Listen`, Kestrel è in ascolto sulla porta specificati quando si esegue l'app senza IIS.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ANCM genera una porta dinamica da assegnare al processo di back-end. Il `UseIISIntegration` metodo preleva la porta dinamica e configura Kestrel per l'ascolto su `http://locahost:{dynamicPort}/`. Si esegue l'override di altre configurazioni di URL, quali chiamate a `UseUrls`. Pertanto, non è necessario chiamare `UseUrls` quando si utilizza ANCM. Se si chiama `UseUrls`, Kestrel è in ascolto sulla porta specificati quando si esegue l'app senza IIS.

In ASP.NET Core 1.0, se si chiama `UseUrls`, chiamarlo **prima** si chiama `UseIISIntegration` in modo che la porta configurata ANCM non viene sovrascritto. Questo ordine di chiamata non è obbligatoria in ASP.NET 1.1 Core, perché esegue l'override dell'impostazione ANCM `UseUrls`.

---

### <a name="configure-ancm-options-in-webconfig"></a>Configurare le opzioni di ANCM in Web. config

Configurazione per il modulo di base di ASP.NET viene archiviata nel *Web. config* file che si trova nella cartella radice dell'applicazione. Impostazioni di questo file scegliere il comando di avvio e gli argomenti che avvia l'app ASP.NET Core. Per esempio *Web. config* codice e informazioni aggiuntive sulle opzioni di configurazione, vedere [riferimento di configurazione di ASP.NET Core modulo](xref:host-and-deploy/aspnet-core-module).

### <a name="run-with-iis-express-in-development"></a>Esecuzione di IIS Express in fase di sviluppo

IIS Express può essere avviata da Visual Studio usando il profilo predefinito definito tramite i modelli ASP.NET Core.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>Configurazione del proxy utilizza il protocollo HTTP e un token di associazione

Il proxy creato tra il ANCM e Kestrel utilizza il protocollo HTTP. Utilizzo di HTTP è un'ottimizzazione delle prestazioni in cui il traffico tra il ANCM e Kestrel effettuata in un indirizzo di loopback dall'interfaccia di rete. Non c'è alcun rischio di intercettazione il traffico tra il ANCM e Kestrel da una posizione dal server.

Un token di associazione viene utilizzato per garantire che le richieste ricevute da Kestrel sono state elaborate da IIS e che non provengono da un'altra origine. Il token di associazione viene creato e impostato in una variabile di ambiente (`ASPNETCORE_TOKEN`) per il ANCM. Il token di associazione viene impostato anche in un'intestazione (`MSAspNetCoreToken`) in tutte le richieste elaborate. Controlla il Middleware di IIS richiesta che riceve per confermare che il valore di intestazione dei token di associazione corrisponda al valore di variabile di ambiente. Se i valori del token non corrispondono, la richiesta viene registrata e rifiutata. La variabile di ambiente token di associazione e il traffico tra il ANCM e Kestrel non sono accessibili da una posizione dal server. Senza conoscere il valore del token di associazione, un utente malintenzionato non è possibile inviare le richieste che ignora il controllo nel Middleware di IIS.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere le seguenti risorse:

* [Applicazione di esempio per questo articolo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)
* [Codice sorgente ASP.NET modulo Core](https://github.com/aspnet/AspNetCoreModule)
* [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Host in Windows con IIS](xref:host-and-deploy/iis/index)
