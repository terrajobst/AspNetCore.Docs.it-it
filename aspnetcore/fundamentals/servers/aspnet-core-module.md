---
title: Modulo ASP.NET Core
author: rick-anderson
description: Informazioni su come il modulo ASP.NET Core consente al server Web Kestrel di usare IIS o IIS Express come server proxy inverso.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 2f73a34b7d311c9e98ad2ecba11894d27bb2aa4d
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910889"
---
# <a name="aspnet-core-module"></a>Modulo ASP.NET Core

Di [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) e [Chris Ross](https://github.com/Tratcher)

::: moniker range=">= aspnetcore-2.2"

Il modulo ASP.NET Core consente l'esecuzione delle app ASP.NET Core in un processo di lavoro IIS (*in-process*) o dietro a IIS in una configurazione di proxy inverso (*out-of-process*). IIS offre funzionalità di sicurezza avanzate per le app Web e ne garantisce la facilità di gestione.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Il modulo ASP.NET Core consente l'esecuzione delle app ASP.NET Core dietro a IIS in una configurazione di proxy inverso. IIS offre funzionalità di sicurezza avanzate per le app Web e ne garantisce la facilità di gestione.

::: moniker-end

Versioni di Windows supportate:

* Windows 7 o versione successiva
* Windows Server 2008 R2 o versioni successive

::: moniker range=">= aspnetcore-2.2"

In caso di hosting in-process, il modulo segue un'implementazione del server specifica, `IISHttpServer`.

In caso di hosting out-of-process, il modulo funziona solo con Kestrel. Il modulo non è compatibile con [HTTP.sys](xref:fundamentals/servers/httpsys) (in precedenza detto [WebListener](xref:fundamentals/servers/weblistener)).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Il modulo funziona solo con Kestrel. Il modulo non è compatibile con [HTTP.sys](xref:fundamentals/servers/httpsys) (in precedenza detto [WebListener](xref:fundamentals/servers/weblistener)).

::: moniker-end

## <a name="aspnet-core-module-description"></a>Descrizione del modulo ASP.NET Core

::: moniker range=">= aspnetcore-2.2"

Il modulo ASP.NET Core è un modulo IIS nativo collegato alla pipeline di IIS per eseguire le operazioni seguenti:

* Ospitare un app ASP.NET Core all'interno del processo di lavoro IIS (`w3wp.exe`), denominato [modello di hosting in-process](#in-process-hosting-model).

* Inoltrare le richieste Web all'app back-end ASP.NET Core che esegue il [server Kestrel](xref:fundamentals/servers/kestrel), denominato [modello di hosting out-of-process](#out-of-process-hosting-model).

### <a name="in-process-hosting-model"></a>Modello di hosting in-process

Se si usa l'hosting in-process, un'app ASP.NET Core esegue lo stesso processo del processo di lavoro IIS. In questo modo le prestazioni non vengono ridotte per inoltrare i dati delle richieste sulla scheda Loopback quando si usa il modello di hosting out-of-process. Per gestire il processo, IIS usa il [servizio Attivazione processo Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Il modulo ASP.NET Core:

* Esegue l'inizializzazione dell'app.
  * Carica [CoreCLR](/dotnet/standard/glossary#coreclr).
  * Chiama `Program.Main`.
* Gestisce la durata della richiesta nativa di IIS.

Il diagramma seguente illustra la relazione tra IIS, il modulo ASP.NET Core e un'app ospitata in-process:

![Modulo ASP.NET Core](aspnet-core-module/_static/ancm-inprocess.png)

Una richiesta arriva dal Web al driver HTTP.sys in modalità kernel. Il driver instrada la richiesta nativa IIS sulla porta configurata per il sito Web, in genere 80 (HTTP) o 443 (HTTPS). Il modulo riceve la richiesta nativa e passa il controllo a `IISHttpServer`, vale a dire ciò che converte la richiesta da nativa a gestita.

Dopo che `IISHttpServer` ha prelevato la richiesta dal modulo, viene eseguito il push della richiesta nella pipeline middleware ASP.NET Core. La pipeline middleware gestisce la richiesta e la passa come istanza di `HttpContext` alla logica dell'app. La risposta dell'app viene quindi passata a IIS, che ne esegue di nuovo il push al client HTTP che ha avviato la richiesta.

### <a name="out-of-process-hosting-model"></a>Modello di hosting out-of-process

Poiché le app ASP.NET Core vengono eseguite in un processo distinto dal processo di lavoro IIS, il modulo esegue la gestione dei processi. Il modulo avvia il processo per l'app ASP.NET Core quando arriva la prima richiesta e riavvia l'app se viene arrestata o si arresta in modo anomalo. Si tratta essenzialmente dello stesso comportamento delle app eseguite in-process e gestite dal [servizio Attivazione processo Windows](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Il diagramma seguente illustra la relazione tra IIS, il modulo ASP.NET Core e un'app ospitata out-of-process:

![Modulo ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

Le richieste arrivano dal Web al driver HTTP.sys in modalità kernel. Il driver instrada le richieste a IIS sulla porta configurata per il sito Web, in genere 80 (HTTP) o 443 (HTTPS). Il modulo inoltra le richieste a Kestrel su una porta casuale per l'app non corrispondente alla porta 80 o 443.

Il modulo specifica la porta tramite una variabile di ambiente all'avvio e il middleware di integrazione di IIS configura il server per l'ascolto su `http://localhost:{port}`. Vengono eseguiti controlli aggiuntivi e le richieste che non provengono dal modulo vengono rifiutate. Il modulo non supporta l'inoltro HTTPS, pertanto le richieste vengono inoltrate tramite HTTP anche se sono state ricevute da IIS tramite HTTPS.

Dopo che Kestrel ha prelevato la richiesta dal modulo, viene eseguito il push della richiesta nella pipeline middleware ASP.NET Core. La pipeline middleware gestisce la richiesta e la passa come istanza di `HttpContext` alla logica dell'app. Il middleware aggiunto dall'integrazione di IIS aggiorna lo schema, l'IP remoto e il percorso di base all'account per l'inoltro della richiesta a Kestrel. La risposta dell'app viene quindi passata a IIS, che ne esegue di nuovo il push al client HTTP che ha avviato la richiesta.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Il modulo ASP.NET Core è un modulo IIS nativo collegato alla pipeline di IIS che inoltra le richieste Web alle app back-end ASP.NET Core.

Poiché le app ASP.NET Core vengono eseguite in un processo distinto dal processo di lavoro IIS, il modulo esegue anche la gestione dei processi. Il modulo avvia il processo per l'app ASP.NET Core quando arriva la prima richiesta e riavvia l'app se si verifica un arresto anomalo di questa. Si tratta essenzialmente dello stesso comportamento delle app ASP.NET 4.x eseguite In-Process in IIS e gestite dal [servizio Attivazione processo Windows (WAS, Windows Activation Service)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Il diagramma seguente illustra la relazione tra IIS, il modulo ASP.NET Core e un'app:

![Modulo ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

Le richieste arrivano dal Web al driver HTTP.sys in modalità kernel. Il driver instrada le richieste a IIS sulla porta configurata per il sito Web, in genere 80 (HTTP) o 443 (HTTPS). Il modulo inoltra le richieste a Kestrel su una porta casuale per l'app non corrispondente alla porta 80 o 443.

Il modulo specifica la porta tramite una variabile di ambiente all'avvio e il middleware di integrazione di IIS configura il server per l'ascolto su `http://localhost:{port}`. Vengono eseguiti controlli aggiuntivi e le richieste che non provengono dal modulo vengono rifiutate. Il modulo non supporta l'inoltro HTTPS, pertanto le richieste vengono inoltrate tramite HTTP anche se sono state ricevute da IIS tramite HTTPS.

Dopo che Kestrel ha prelevato la richiesta dal modulo, viene eseguito il push della richiesta nella pipeline middleware ASP.NET Core. La pipeline middleware gestisce la richiesta e la passa come istanza di `HttpContext` alla logica dell'app. Il middleware aggiunto dall'integrazione di IIS aggiorna lo schema, l'IP remoto e il percorso di base all'account per l'inoltro della richiesta a Kestrel. La risposta dell'app viene quindi passata a IIS, che ne esegue di nuovo il push al client HTTP che ha avviato la richiesta.

::: moniker-end

Molti moduli nativi, ad esempio l'autenticazione di Windows, rimangono attivi. Per altre informazioni sui moduli IIS attivi con il modulo, vedere <xref:host-and-deploy/iis/modules>.

Il modulo ASP.NET Core ha anche qualche altra funzione. Il modulo è in grado di:

* Impostare variabili di ambiente per il processo di lavoro.
* Registrare output stdout in una risorsa di archiviazione di file per la risoluzione di problemi di avvio.
* Inoltrare token di autenticazione di Windows.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>Come installare e usare il modulo ASP.NET Core

Per istruzioni dettagliate su come installare e usare il modulo ASP.NET Core, vedere <xref:host-and-deploy/iis/index>. Per informazioni sulla configurazione del modulo, vedere <xref:host-and-deploy/aspnet-core-module>.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* [Repository GitHub del modulo ASP.NET Core (codice sorgente)](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
