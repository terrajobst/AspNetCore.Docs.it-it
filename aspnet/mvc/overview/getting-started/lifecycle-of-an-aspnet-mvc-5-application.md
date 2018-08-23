---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Ciclo di vita di un'applicazione ASP.NET MVC 5 | Microsoft Docs
author: cephalin
description: Scaricare un documento PDF che rappresenta graficamente i ciclo di vita di un'applicazione ASP.NET MVC 5. Questo ciclo di vita documento viene fornita una panoramica del ciclo di vita MVC un...
ms.author: riande
ms.date: 02/28/2014
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 5523217ae5674ac4e5898a150f9e5f0ae3a70d26
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834362"
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a>Ciclo di vita di un'applicazione ASP.NET MVC 5
====================
da [Cephas Lin](https://github.com/cephalin)

[Scaricare il documento PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

Qui è possibile scaricare un documento PDF grafici il ciclo di vita di ogni applicazione di ASP.NET MVC 5, dalla ricezione HTTP richiesta per l'invio della risposta HTTP al client. È progettato come un strumento di apprendimento per gli utenti che hanno familiarità con ASP.NET MVC e anche come riferimento per coloro che richiedono approfondire alcuni aspetti specifici dell'applicazione. Il documento PDF presenta le funzionalità seguenti:

- Rilevanti [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) fasi che aiutano a comprendere dove MVC integra le [ciclo di vita dell'applicazione ASP.NET](https://msdn.microsoft.com/library/bb470252.aspx).
- Una panoramica generale del ciclo di vita dell'applicazione MVC, dove è possibile comprendere le fasi principali che ogni applicazione MVC di passa attraverso la pipeline di elaborazione della richiesta.  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- Visualizzazione dettagli che mostra drill nei dettagli della pipeline di elaborazione della richiesta. È possibile confrontare la visualizzazione di alto livello e la visualizzazione dettagli per vedere come vengono raccolti i dettagli di cicli di vita in varie fasi. [Scarica il PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) per ottenere una visualizzazione più grande.
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- Selezione host e lo scopo di tutti i metodi sottoponibili a override nel [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) oggetto nella pipeline di elaborazione della richiesta. È possibile o non abbia la necessità di eseguire l'override di qualsiasi metodo di uno, ma è importante comprendere il loro ruolo nel ciclo di vita dell'applicazione in modo che sia possibile scrivere codice nella fase del ciclo di vita appropriato per ottenere l'effetto desiderato.
- Diagrammi-up entusiasmante che mostra come ognuno dei tipi di filtro (autenticazione, autorizzazione, azioni e risultati) viene richiamata.
- Collega a un articolo utile o blog da ogni punto di interesse nella visualizzazione dettagli.


## <a name="next-steps"></a>Passaggi successivi

Questo documento soddisfa l'esigenza? Apprezziamo i tuoi commenti. Se hai domande sul ciclo di vita di ASP.NET MVC nell'applicazione [Stackoverflow](http://stackoverflow.com/help) e il [forum su ASP.NET MVC](https://forums.asp.net/1146.aspx) sono uno spazio eccezionale per chiedere. Seguire [me](https://twitter.com/Cephas_MSFT) su twitter in modo che è possibile ottenere gli aggiornamenti nella mio esercitazioni più recenti.
