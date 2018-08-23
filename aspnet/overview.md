---
uid: overview
title: Panoramica di ASP.NET | Microsoft Docs
author: rick-anderson
description: Introduzione ad ASP.NET, un framework gratuito per la creazione di siti Web, applicazioni web e API web.
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.author: riande
ms.date: 03/12/2010
ms.technology: aspnet
msc.legacyurl: ''
msc.type: content
ms.openlocfilehash: 5bdebcc226050afc2469840dc4a4dc97ec6b80b2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833974"
---
# <a name="aspnet-overview"></a>Panoramica di ASP.NET

ASP.NET è un framework web gratuito per la creazione di siti Web eccezionali e applicazioni web con HTML, CSS e JavaScript. È anche possibile creare API Web e usare tecnologie in tempo reale come WebSocket.

[ASP.NET Core](https://docs.microsoft.com/aspnet/core/) è un'alternativa ad ASP.NET.  Vedere le [materiale sussidiario su come scegliere tra ASP.NET e ASP.NET Core](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework).

## <a name="get-started"></a>Introduzione

[Visual Studio Community 2017](https://www.visualstudio.com/downloads/), un IDE gratuito per ASP.NET in Windows.

## <a name="websites-and-web-applications"></a>Siti e applicazioni web

 ASP.NET offre tre Framework per la creazione di applicazioni web: Web Form, ASP.NET MVC e ASP.NET Web Pages. Tutti i tre Framework sono stabili e maturo ed è possibile creare applicazioni web eccezionali con uno di essi. Indipendentemente da quali framework prescelte, otterrai tutti i vantaggi e le funzionalità di ASP.NET ovunque.

Ogni framework è destinato a uno stile di sviluppo diversi. Quello che scelto dipende da una combinazione degli asset di programmazione (conoscenze, capacità ed esperienza di sviluppo), il tipo di applicazione che si sta creando e si ha familiarità con l'approccio di sviluppo.

Di seguito viene fornita una panoramica della ognuno dei Framework e alcune idee su come scegliere tra di essi. Se si preferisce un video introduttivo, vedere [rendendo siti Web con ASP.NET](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET) e [What ' s Web Tools?](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | Se si ha esperienza | Stile di sviluppo | Competenze | 
|-----------|----------------------|-----------------------------------------------------|----------------|
| Web Form | Win Forms, WPF, .NET | Rapido sviluppo con un'avanzata libreria di controlli che incapsulano il markup HTML | RAD di livello intermedio, avanzate |
| MVC       | Ruby on Rails, .NET  | Controllo completo sul markup HTML, codice e markup separati e facile da scrivere i test. La scelta migliore per le applicazioni per dispositivi mobili e a pagina singola (SPA). | Il livello intermedio, avanzate |
| Pagine Web  | Classic ASP, PHP     | Markup HTML e il codice insieme nello stesso file | Nuovo, il livello intermedio |

### <a name="web-forms"></a>Web Form

Con Web Form ASP.NET, è possibile compilare siti Web dinamici usando un modello noto di trascinamento e rilascio, basata su eventi. Un'area di progettazione e centinaia di controlli e componenti che consentono di creare rapidamente potenti e sofisticati siti basati su interfaccia utente con accesso ai dati. 

[Altre informazioni sui Web Form](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC offre un modo potente, basato su modelli per creare siti Web dinamici che consente una netta separazione degli aspetti problematici e controllo completo sul markup per uno sviluppo agile piacevole. ASP.NET MVC include numerose funzionalità che consentono lo sviluppo veloce, sviluppo per la creazione di applicazioni complesse che utilizzano gli standard web più recenti. 

[Altre informazioni su MVC](mvc/index.md)

### <a name="aspnet-web-pages"></a>Pagine Web ASP.NET

ASP.NET Web Pages e la sintassi Razor forniscono un modo veloce, semplice e accessibile di combinare il codice server con HTML per creare contenuto web dinamico. Connettersi ai database, aggiungere video, collegare a siti di social networking e includere molte altre funzionalità che consentono di creare siti interessanti conformi agli standard web più recenti.

[Altre informazioni sulle pagine Web](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Note sui Web Form, MVC e pagine Web

Tutti i tre framework ASP.NET sono basati su .NET Framework e condividono le funzionalità principali di .NET e ASP.NET. Ad esempio, tutti i tre Framework di offrire un modello di sicurezza di account di accesso basato sulle appartenenze e condividono la stessa funzionalità per la gestione delle richieste, la gestione delle sessioni e così via che fanno parte di una funzionalità ASP.NET core.

Inoltre, i tre Framework non sono completamente indipendenti, e la scelta di uno non preclude l'uso di un altro. Poiché i Framework possono coesistere nella stessa applicazione web, non è insolito per visualizzare i singoli componenti delle applicazioni scritti usando Framework distinti. Ad esempio, per i clienti di parti di un'app potrebbero essere sviluppate in MVC per ottimizzare il markup, mentre l'accesso ai dati e parti amministrative vengono sviluppate in Web Form per sfruttare i vantaggi dei controlli di data e l'accesso ai dati semplice.

## <a name="web-apis"></a>API Web

API Web ASP.NET è un framework che rende più semplice compilare servizi HTTP che soddisfano una vasta gamma di client, inclusi browser e dispositivi mobili. API Web ASP.NET è la piattaforma ideale per compilare applicazioni RESTful in .NET Framework.

[Altre informazioni sull'API Web](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>Tecnologie in tempo reale

ASP.NET SignalR è una nuova libreria per sviluppatori ASP.NET che rende più semplice sviluppare funzionalità web in tempo reale. SignalR consente la comunicazione bidirezionale tra server e client. I server possono eseguire il push del contenuto ai client connessi immediatamente appena sarà disponibile. SignalR supporta i WebSocket e tenti di altre tecniche compatibile per i browser meno recenti. SignalR include le API per la gestione connessione (ad esempio, connettere e disconnettere gli eventi), il raggruppamento delle connessioni e le autorizzazioni.

[Altre informazioni su SignalR](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>Siti e App per dispositivi mobili 

ASP.NET può consentire l'App per dispositivi mobili native con un back-end API Web, nonché siti web per dispositivi mobili usando i Framework, ad esempio Twitter Bootstrap progettazione reattiva. Se si sta creando un'app per dispositivi mobili native, è facile creare un'API per l'accesso ai dati handle, autenticazione e le notifiche push per l'app Web basati su JSON. Se si sta creando un sito per dispositivi mobili reattivo, è possibile usare qualsiasi framework CSS o sistema griglia aperta si preferisce, o selezionare un sistema potenti per dispositivi mobili, ad esempio jQuery Mobile o Sencha ed eccezionali applicazioni per dispositivi mobili con PhoneGap.

[Altre informazioni sullo sviluppo di app e siti per dispositivi mobili](mobile/index.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>Applicazioni a singola pagina 

Applicazione a pagina singola ASP.NET (SPA) consente di compilare applicazioni che includono significative interazioni lato client usando HTML5, CSS 3 e JavaScript. Visual Studio include un modello per la compilazione di applicazioni a pagina singola usando Knockout. js e ASP.NET Web API. Oltre al modello di applicazione a singola pagina incorporato, creati dalla community SPA modelli sono disponibili anche per il download.

[Ulteriori informazioni sullo sviluppo di app a singola pagina](single-page-application/index.md)

## <a name="webhooks"></a>Webhook

I Webhook sono un pattern HTTP leggero che fornisce un modello pub/sub semplice per collegare tra loro servizi SaaS e API Web. Quando si verifica un evento in un servizio, sotto forma di una richiesta HTTP POST viene inviata una notifica ai sottoscrittori registrati. La richiesta POST contiene informazioni sull'evento che rende possibile per il ricevitore di agire di conseguenza.

I Webhook vengono esposte da un numero elevato di servizi tra cui Dropbox, GitHub, Instagram, MailChimp, PayPal, Slack, Trello e molto altro ancora. Ad esempio, un WebHook può indicare che è stato modificato un file in Dropbox, è stato eseguito il commit di una modifica del codice in GitHub, o un pagamento è stato avviato in PayPal o è stata creata una scheda in Trello.

[Altre informazioni sui Webhook](webhooks/index.md)





<!--
Create Deployment TOC based on https://www.asp.net/aspnet/overview/deployment
Copy deployment content map to MVC, WebForms, Web Pages, Web API sections.
Copy Web Deployment in Enterprise from WebForms to MVC
Move under ASP.NET Best practices
    What not to do in ASP.NET, and what to do instead https://review.docs.microsoft.cus/aspnet/aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
    Async and await https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/async-and-await
    Building Real World Cloud Apps with Azure https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
    Hands on Lab: Maintainable Azure Websites: Managing Change and Scale https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale

-->
