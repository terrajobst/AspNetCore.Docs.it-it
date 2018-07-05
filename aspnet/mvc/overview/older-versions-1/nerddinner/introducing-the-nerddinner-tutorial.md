---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Introduzione all'esercitazione NerdDinner | Microsoft Docs
author: shanselman
description: Il modo migliore per imparare un nuovo framework consiste nella compilazione di un elemento con esso. Questa esercitazione illustra in dettaglio come compilare un'applicazione di piccole dimensioni, ma completa, tramite ASP. ne...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 9188df1eca7f488502640bc17d5034f93f4ac82c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805110"
---
<a name="introducing-the-nerddinner-tutorial"></a>Introduzione all'esercitazione NerdDinner
====================
da [Scott Hanselman](https://github.com/shanselman)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il modo migliore per imparare un nuovo framework consiste nella compilazione di un elemento con esso. Questa esercitazione illustra come compilare una piccola, ma completa, applicazione che usa ASP.NET MVC 1 e introduce alcuni concetti di base sottostante.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le [Guida introduttiva con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oppure [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-tutorial"></a>Esercitazione NerdDinner

Il modo migliore per imparare un nuovo framework consiste nella compilazione di un elemento con esso. Questa esercitazione illustra come compilare una piccola, ma completa, dell'applicazione tramite ASP.NET MVC e introduce alcuni concetti di base sottostante.

L'applicazione che si intende compilare è chiamato "NerdDinner". NerdDinner offre un modo semplice per gli utenti individuare e organizzare dinners online:

![](introducing-the-nerddinner-tutorial/_static/image1.png)

NerdDinner consente agli utenti registrati di creare, modificare ed eliminare dinners. Viene utilizzato per applicare un set coerente di convalida e le regole business all'interno dell'applicazione:

![](introducing-the-nerddinner-tutorial/_static/image2.png)

I visitatori possono utilizzare una mappa basata su AJAX per la ricerca dinners imminenti mantenuto quasi li:

![](introducing-the-nerddinner-tutorial/_static/image3.png)

Facendo clic su un dinner li indirizzerà a una pagina in cui è possibile imparare informazioni su di essa:

![](introducing-the-nerddinner-tutorial/_static/image4.png)

Se si è interessati a frequentate dinner potranno accedere o registrarsi nel sito:

![](introducing-the-nerddinner-tutorial/_static/image5.png)

È quindi possibile fare clic su un collegamento RSVP basate su AJAX per partecipare all'evento:

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>Implementazione di NerdDinner

Si userà per avviare l'applicazione di NerdDinner usando il File -&gt;comandi nuovo progetto di Visual Studio per creare un nuovo progetto ASP.NET MVC. Verrà quindi aggiunto in modo incrementale le funzionalità. Corso della trattazione trattati:

1. [Come creare un nuovo progetto ASP.NET MVC](# "crea un nuovo progetto MVC ASP.NET")
2. [Come creare un database](# "creare un Database")
3. [Come compilare un modello con convalide delle regole business](# "compilare un modello con convalide delle regole Business")
4. [Come usare controller e visualizzazioni per implementare un'interfaccia utente elenco/dettagli](# "usare controller e visualizzazioni per implementare un'interfaccia utente elenco/dettagli")
5. [Come fornire CRUD (create, leggere, aggiornare ed eliminare) dati formano il supporto di ingresso](# "forniscono CRUD (Create, Read, Update, Delete) dati Form voce supporta")
6. [Come usare ViewData e implementare classi ViewModel](# "usare ViewData e implementare classi ViewModel")
7. [Come usare nuovamente l'interfaccia utente usando le pagine master e visualizzazioni parziali](# "riutilizzo di interfaccia utente tramite pagine Master e visualizzazioni parziali")
8. [Come implementare il paging dei dati efficiente](# "implementare Paging dei dati efficiente")
9. [Come proteggere le applicazioni usando l'autenticazione e autorizzazione](# "sicure le applicazioni usando autenticazione e autorizzazione")
10. [Come usare AJAX per distribuire gli aggiornamenti dinamici](# "usare AJAX per inviare gli aggiornamenti dinamici")
11. [Come usare AJAX per implementare scenari di mapping](# "usare AJAX per implementare scenari di Mapping")
12. [Come abilitare il testing unità automatizzato](# "Abilita Testing unità automatizzato")

È possibile compilare la propria copia di NerdDinner da zero, completare ogni passaggio sono procedura dettagliata in questo capitolo. In alternativa, è possibile scaricare una versione completa del codice sorgente di seguito: [NerdDinner su GitHub](https://github.com/AspNetMVPSamples/NerdDinner). È anche possibile anche possibile [scaricare una versione PDF gratuita di questa esercitazione](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) se si desidera leggere l'esercitazione offline.

Per compilare l'applicazione, è possibile usare Visual Studio 2008 o il gratuito Visual Web Developer 2008 Express. È possibile usare SQL Server o gratuita SQL Server Express per il database.

È possibile installare ASP.NET MVC, Visual Web Developer 2008 Express e SQL Server Express (tutto gratuito) con V2 del [installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>A questo punto possiamo iniziare...

Ora che abbiamo trattato NerdDinner What ' s, verrà ora nostro maniche e scrivere il codice.

Inizieremo con File -&gt;nuovo progetto in Visual Studio per creare l'applicazione di NerdDinner.

> [!div class="step-by-step"]
> [avanti](create-a-new-aspnet-mvc-project.md)
