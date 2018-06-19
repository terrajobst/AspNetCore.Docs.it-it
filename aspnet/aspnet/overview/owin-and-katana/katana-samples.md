---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Esempi di Katana | Documenti Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 815355c00c9c15cfefa5f98dc89da676743b0390
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
ms.locfileid: "28034490"
---
<a name="katana-samples"></a>Esempi di Katana
====================
by [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Esempi di Katana

**Esegue il routing ASP.NET esempio** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)  
In alcune applicazioni è possibile collegare i componenti OWIN nella tabella di routing di Asp.Net in modo affiancato con componenti non OWIN. In questo esempio viene illustrato come utilizzare i metodi di estensione RouteCollection MapOwinPath e MapOwinRoute forniti da systemweb.

**Creazione di rami pipeline esempio** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)  
Pipeline di elaborazione della richiesta OWIN non deve necessariamente essere lineare, è possibile creare per elaborare le richieste in modi diversi. Questo esempio viene illustrato come creare una pipeline di diramazione in base a percorsi di richiesta o altri dati della richiesta, ad esempio le intestazioni. Questi componenti sono disponibili nel pacchetto nuget Microsoft.Owin.Mapping.

**Esempio di Server personalizzati** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs)   
Viene illustrato come utilizzare un server OWIN personalizzato quando il Self-hosting OWIN.

**Esempio incorporato** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)  
Alcuni server OWIN può essere eseguito all'interno di un processo personalizzato (&quot;self-hosted&quot;). In questo esempio viene illustrato come avviare un'applicazione OWIN usando gli strumenti forniti dal pacchetto nuget di owin.

**Esempio HelloWorld** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)  
OWIN è un server HTTP astrazione di API che consentono la portabilità dell'applicazione tra più server. In questo esempio viene illustrato come scrivere un'applicazione Hello World utilizzando alcune **semplici wrapper** intorno l'astrazione di OWIN non elaborati e l'esecuzione su un server web come ASP.NET.

**Esempio Hello World Raw OWIN** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)  
In questo esempio viene illustrato come scrivere un'applicazione Hello World utilizzando il **raw** astrazione OWIN ed eseguirlo in un server web ad Asp.Net.

**Esempio di SignalR** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)  
Viene illustrato come self-hosting SignalR utilizzando OWIN / Katana. Per ulteriori informazioni su SignalR self-hosting, vedere [esercitazione: SignalR indipendente](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Esempio di file statici** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs)   
Viene illustrato come supportare le richieste HTTP per i file statici con OWIN / Katana.

**API Web** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt)   
In questo esempio viene illustrato come ospitare OWIN in IIS e aggiungere l'API Web per la pipeline OWIN.

**Esempio di Socket Web** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs)   
Viene illustrato come supportare WebSocket in OWIN tramite il [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) classe.
