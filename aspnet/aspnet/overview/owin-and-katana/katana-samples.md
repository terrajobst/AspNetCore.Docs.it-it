---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Esempi di Katana | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: e81da1e650d8dfd24a3e0fda6aa42b7f360ce12d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393319"
---
<a name="katana-samples"></a>Esempi di Katana
====================
da [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Esempi di Katana

**ASP.NET esegue il routing campione** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)  
In alcune applicazioni si dovrà associare i componenti OWIN nella tabella di route Asp.Net affiancato con componenti non OWIN. In questo esempio viene illustrato come utilizzare i metodi di estensione RouteCollection MapOwinPath e MapOwinRoute fornita da systemweb.

**Creazione di rami pipeline di esempio** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)  
Le pipeline di elaborazione della richiesta OWIN non deve necessariamente essere lineare, può essere sottoposto a branching per elaborare le richieste in modi diversi. Questo esempio viene illustrato come creare una pipeline con rami basata su percorsi di richiesta o altri dati della richiesta, ad esempio le intestazioni. Questi componenti sono disponibili nel pacchetto nuget Microsoft.Owin.Mapping.

**Esempio di Server personalizzati** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs)   
Viene illustrato come utilizzare un server personalizzato OWIN self-hosting OWIN.

**Esempio incorporato** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)  
Alcuni server OWIN può essere eseguito all'interno di un processo personalizzato (&quot;self-hosted&quot;). In questo esempio viene illustrato come avviare un'applicazione OWIN usando gli strumenti forniti dal pacchetto nuget di owin.

**Esempio HelloWorld** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)  
OWIN è un server HTTP astrazione di API che consente la portabilità delle applicazioni tra server diversi. In questo esempio viene illustrato come scrivere un'applicazione Hello World usando alcuni **semplici wrapper** tutto l'astrazione di OWIN e l'esecuzione non elaborato in un server web, ad esempio ASP.NET.

**Esempio Hello World non elaborati OWIN** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)  
In questo esempio viene illustrato come scrivere un'applicazione Hello World tramite il **raw** astrazione OWIN ed eseguirlo in un server web come Asp.Net.

**Esempio di SignalR** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)  
Viene illustrato come self-hosting OWIN tramite SignalR / Katana. Per altre informazioni su SignalR self-hosting, vedi [esercitazione: hosting indipendente di SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Esempio di file statici** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs)   
Viene illustrato come supportare le richieste HTTP per i file statici Usa OWIN / Katana.

**API Web** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt)   
Questo esempio viene illustrato come ospitare OWIN in IIS e aggiungere l'API Web alla pipeline OWIN.

**Web Socket campione** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs)   
Viene illustrato come supportare WebSocket in OWIN usando la [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) classe.
