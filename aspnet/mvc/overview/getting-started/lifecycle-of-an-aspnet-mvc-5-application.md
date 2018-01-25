---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Ciclo di vita di un'applicazione ASP.NET MVC 5 | Documenti Microsoft
author: cephalin
description: Scaricare un documento PDF che illustra il ciclo di vita di un'applicazione ASP.NET MVC 5. Questo ciclo di vita documento viene fornita una panoramica del ciclo di vita MVC un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 50d58d10c11677fa72ede6a03e686cbde4cbae1d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a>Ciclo di vita di un'applicazione MVC ASP.NET 5
====================
da [Cephas Lin](https://github.com/cephalin)

[Scaricare il documento PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

Qui è possibile scaricare un documento PDF che grafici il ciclo di vita di ogni applicazione ASP.NET MVC 5, di ricezione HTTP richiesta per l'invio della risposta HTTP al client. È progettato come un strumento di apprendimento per gli utenti che hanno familiarità con MVC ASP.NET e anche come riferimento per gli utenti che necessitano di approfondire gli aspetti specifici dell'applicazione. Il documento PDF ha le caratteristiche seguenti:

- Rilevanti [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) fasi per comprendere in MVC integra il [ciclo di vita dell'applicazione ASP.NET](https://msdn.microsoft.com/library/bb470252.aspx).
- Panoramica del processo di applicazione MVC, in cui è possibile comprendere le fasi principali che ogni applicazione MVC di passa attraverso la pipeline di elaborazione della richiesta.  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- Una visualizzazione dettagliata che illustra drill nei dettagli della pipeline di elaborazione della richiesta. È possibile confrontare la visualizzazione di alto livello e la visualizzazione di dettaglio per vedere come vengono raccolti i dettagli di cicli di vita in varie fasi. [Scarica il PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) per ottenere una visualizzazione più grande.
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- La selezione e lo scopo di tutti i metodi sottoponibili a override sul [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) oggetto nella pipeline di elaborazione della richiesta. È possibile o non abbia la necessità di eseguire l'override di qualsiasi metodo di uno, ma è importante comprendere il proprio ruolo nel ciclo di vita dell'applicazione in modo che sia possibile scrivere codice fase del ciclo di vita appropriato per ottenere l'effetto desiderato.
- Diagrammi di essere sostituito-up che mostra come ognuno dei tipi di filtro (autenticazione, autorizzazione, azione e risultato) viene richiamata.
- Per collegarsi a un articolo utile o un blog da ogni punto di interesse nella vista di dettaglio.


## <a name="next-steps"></a>Passaggi successivi

Questo documento soddisfa l'esigenza? Si si apprezza l'invio di commenti e suggerimenti. Se hai domande sul ciclo di vita di ASP.NET MVC nell'applicazione, [Stackoverflow](http://stackoverflow.com/help) e [forum di ASP.NET MVC](https://forums.asp.net/1146.aspx) sono i modi per chiedere. Seguire [me](https://twitter.com/Cephas_MSFT) su twitter, pertanto è possibile ottenere gli aggiornamenti in my esercitazioni più recente.
