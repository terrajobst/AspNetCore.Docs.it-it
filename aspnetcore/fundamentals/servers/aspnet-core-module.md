---
title: Modulo ASP.NET Core
author: rick-anderson
description: Informazioni su come il modulo ASP.NET Core consente al server Web Kestrel di usare IIS o IIS Express come server proxy inverso.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 789b0b2e74405256bb0e247ae5ea9697e3cb28e5
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153705"
---
# <a name="aspnet-core-module"></a>Modulo ASP.NET Core

Di [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) e [Chris Ross](https://github.com/Tratcher) 

Il modulo ASP.NET Core consente l'esecuzione delle app ASP.NET Core dietro a IIS in una configurazione di proxy inverso. IIS offre funzionalità di sicurezza avanzate per le app Web e ne garantisce la facilità di gestione.

Versioni di Windows supportate:

* Windows 7 o versione successiva
* Windows Server 2008 R2 o versioni successive

Il modulo ASP.NET Core Module funziona solo con Kestrel. Il modulo non è compatibile con [HTTP.sys](xref:fundamentals/servers/httpsys) (in precedenza detto [WebListener](xref:fundamentals/servers/weblistener)).

## <a name="aspnet-core-module-description"></a>Descrizione del modulo ASP.NET Core

Il modulo ASP.NET Core è un modulo IIS nativo collegato alla pipeline di IIS che reindirizza le richieste Web alle app back-end ASP.NET Core. Molti moduli nativi, ad esempio l'autenticazione di Windows, rimangono attivi. Per altre informazioni sui moduli IIS attivi con il modulo, vedere [Moduli IIS](xref:host-and-deploy/iis/modules).

Poiché le app ASP.NET Core vengono eseguite in un processo distinto dal processo di lavoro IIS, il modulo esegue anche la gestione dei processi. Il modulo avvia il processo per l'app ASP.NET Core quando arriva la prima richiesta e riavvia l'app se si verifica un arresto anomalo di questa. Si tratta essenzialmente dello stesso comportamento delle app ASP.NET 4.x eseguite In-Process in IIS e gestite dal [servizio Attivazione processo Windows (WAS, Windows Activation Service)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Il diagramma seguente illustra la relazione tra IIS, il modulo ASP.NET Core e le app ASP.NET Core:

![Modulo ASP.NET Core](aspnet-core-module/_static/ancm.png)

Le richieste arrivano dal Web al driver HTTP.sys in modalità kernel. Il driver instrada le richieste a IIS sulla porta configurata per il sito Web, in genere 80 (HTTP) o 443 (HTTPS). Il modulo inoltra le richieste a Kestrel su una porta casuale per l'app non corrispondente alla porta 80 o 443.

Il modulo specifica la porta tramite una variabile di ambiente all'avvio e il middleware di integrazione di IIS configura il server per l'ascolto su `http://localhost:{port}`. Vengono eseguiti controlli aggiuntivi e le richieste che non provengono dal modulo vengono rifiutate. Il modulo non supporta l'inoltro HTTPS, pertanto le richieste vengono inoltrate tramite HTTP anche se sono state ricevute da IIS tramite HTTPS.

Dopo che Kestrel ha prelevato una richiesta dal modulo, viene eseguito il push della richiesta nella pipeline middleware ASP.NET Core. La pipeline middleware gestisce la richiesta e la passa come istanza di `HttpContext` alla logica dell'app. La risposta dell'app viene quindi passata a IIS, che ne esegue di nuovo il push al client HTTP che ha avviato la richiesta.

Il modulo ASP.NET Core ha anche qualche altra funzione. Il modulo è in grado di:

* Impostare variabili di ambiente per il processo di lavoro.
* Registrare output stdout in una risorsa di archiviazione di file per la risoluzione di problemi di avvio.
* Inoltrare token di autenticazione di Windows.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>Come installare e usare il modulo ASP.NET Core

Per istruzioni dettagliate su come installare e usare il modulo ASP.NET Core, vedere [Host in Windows con IIS](xref:host-and-deploy/iis/index). Per informazioni sulla configurazione del modulo, vedere le [Informazioni di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

## <a name="additional-resources"></a>Risorse aggiuntive

* [Host in Windows con IIS](xref:host-and-deploy/iis/index)
* [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Repository GitHub del modulo ASP.NET Core (codice sorgente)](https://github.com/aspnet/AspNetCoreModule)
