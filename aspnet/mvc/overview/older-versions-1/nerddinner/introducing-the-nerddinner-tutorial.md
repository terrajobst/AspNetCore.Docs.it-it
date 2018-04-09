---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Introduzione dell'esercitazione NerdDinner | Documenti Microsoft
author: shanselman
description: Il modo migliore per imparare un nuovo framework consiste nel compilare un elemento con esso. In questa esercitazione illustra in dettaglio come compilare un'applicazione di piccola ma completa, utilizzando ASP.NE...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 3d925a7dc89fc0c742468653c5c138a0f1d71231
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="introducing-the-nerddinner-tutorial"></a>Introduzione dell'esercitazione NerdDinner
====================
da [Scott Hanselman](https://github.com/shanselman)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il modo migliore per imparare un nuovo framework consiste nel compilare un elemento con esso. In questa esercitazione illustra in dettaglio come compilare una piccola ma completa, l'applicazione mediante ASP.NET MVC 1 e vengono presentati alcuni dei concetti di base.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire il [recupero avviato con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [negozio MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-tutorial"></a>NerdDinner esercitazione

Il modo migliore per imparare un nuovo framework consiste nel compilare un elemento con esso. In questa esercitazione illustra in dettaglio come compilare una piccola ma completa, l'applicazione mediante ASP.NET MVC e vengono presentati alcuni dei concetti di base.

L'applicazione che verrà compilazione viene chiamato "NerdDinner". NerdDinner fornisce un modo semplice per gli utenti individuare e organizzare dinners online:

![](introducing-the-nerddinner-tutorial/_static/image1.png)

NerdDinner consente agli utenti registrati creare, modificare ed eliminare dinners. Applica un set coerente di convalida e regole business all'interno dell'applicazione:

![](introducing-the-nerddinner-tutorial/_static/image2.png)

I visitatori possono utilizzare una mappa basata su AJAX per la ricerca dinners future mantenuti vicini:

![](introducing-the-nerddinner-tutorial/_static/image3.png)

Facendo clic su una cena giungeranno a una pagina di dettagli in cui possono apprendere altre informazioni:

![](introducing-the-nerddinner-tutorial/_static/image4.png)

Se sono interessati che frequentano dinner potranno accedere o registrare nel sito:

![](introducing-the-nerddinner-tutorial/_static/image5.png)

È quindi possibile fare clic su un collegamento RSVP basate su AJAX per partecipare all'evento:

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>Implementazione NerdDinner

Per avviare l'applicazione NerdDinner tramite File - verrà&gt;comando nuovo progetto in Visual Studio per creare un nuovo progetto ASP.NET MVC. Verranno quindi aggiunti in modo incrementale e delle funzionalità. Lungo il percorso ci occuperemo:

1. [Come creare un nuovo progetto ASP.NET MVC](# "crea un nuovo progetto MVC ASP.NET")
2. [Come creare un database](# "creare un Database")
3. [Come creare un modello con le convalide di regola business](# "compilare un modello con le convalide di regola Business")
4. [Come usare controller e visualizzazioni per implementare un listato/dettagli UI](# "utilizzano controller e visualizzazioni per implementare un'interfaccia utente/dettagli")
5. [Come fornire CRUD (create, leggere, aggiornare ed eliminare) dati formano supporto voce](# "forniscono CRUD (Create, Read, Update, Delete) dati Form voce supporta")
6. [Come utilizzare ViewData e implementare le classi ViewModel](# "ViewData usare e implementare le classi ViewModel")
7. [Come per il riutilizzo dell'interfaccia utente utilizzando le pagine master e parziali](# "riutilizzo di interfaccia utente utilizzando pagine Master e parziali")
8. [Come implementare il paging dei dati efficiente](# "implementare Paging dei dati efficiente")
9. [Come proteggere le applicazioni tramite l'autenticazione e autorizzazione](# "sicuro delle applicazioni usando autenticazione e autorizzazione")
10. [Come utilizzare AJAX per distribuire gli aggiornamenti dinamici](# "utilizzare AJAX per inviare gli aggiornamenti dinamici")
11. [Come utilizzare AJAX per implementare scenari di mapping](# "utilizzare AJAX per implementare scenari di Mapping")
12. [Come abilitare unit test automatici](# "attiva Testing unità automatizzati")

È possibile compilare la propria copia di NerdDinner da zero, completare ogni passaggio è procedura dettagliata in questo capitolo. In alternativa, è possibile scaricare una versione completa del codice sorgente qui: [NerdDinner su GitHub](https://github.com/AspNetMVPSamples/NerdDinner). Facoltativamente è possibile inoltre anche [scaricare una versione gratuita PDF di questa esercitazione](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) se si desidera leggere l'esercitazione non in linea.

Per compilare l'applicazione, è possibile utilizzare Visual Studio 2008 o il libero Visual Web Developer 2008 Express. È possibile utilizzare SQL Server o il disponibile SQL Server Express per il database.

È possibile installare ASP.NET MVC, Visual Web Developer 2008 Express e SQL Server Express (gratuitamente) tramite V2 del [installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>Ora è possibile iniziare subito...

Ora che abbiamo trattato novità NerdDinner, si riporta il nostro manicotti e scrivere codice.

Inizieremo con File -&gt;nuovo progetto di Visual Studio per creare l'applicazione NerdDinner.

> [!div class="step-by-step"]
> [avanti](create-a-new-aspnet-mvc-project.md)
