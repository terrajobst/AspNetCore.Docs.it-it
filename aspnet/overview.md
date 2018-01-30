---
uid: overview
title: Panoramica di ASP.NET | Documenti Microsoft
author: rick-anderson
description: Introduzione a ASP.NET, un framework per la creazione di siti Web, applicazioni web e API web gratuito.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2010
ms.topic: article
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: 
msc.type: content
ms.openlocfilehash: 3d4c34a35e2e34ed78f481c759eda3718edb4da6
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-overview"></a>Panoramica di ASP.NET

ASP.NET è un framework web gratuito per la compilazione di siti Web di grande e applicazioni web tramite HTML, CSS e JavaScript. È anche possibile creare API Web e utilizzare tecnologie in tempo reale come WebSocket.

[ASP.NET Core](https://docs.microsoft.com/aspnet/core/) rappresenta un'alternativa ad ASP.NET.  Vedere il [informazioni aggiuntive su come scegliere tra ASP.NET e ASP.NET Core](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework).

## <a name="get-started"></a>Introduzione

[Scaricare Visual Studio 2015](https://go.microsoft.com/fwlink/?LinkId=826064), liberare IDE per ASP.NET su Windows.

## <a name="websites-and-web-applications"></a>Siti e applicazioni web

 In ASP.NET sono disponibili tre strutture per la creazione di applicazioni web: Web Form e ASP.NET MVC ASP.NET Web Pages. Tutti e tre i Framework sono stabili e avanzata ed è possibile creare applicazioni web grande con uno di essi. Indipendentemente da quali framework scelto, si otterrà tutti i vantaggi e le funzionalità di ASP.NET ovunque.

Ognuno dei tre uno stile di sviluppo diversi. Il tipo di modello utilizzato dipende una combinazione delle risorse di programmazione (conoscenze, capacità ed esperienza di sviluppo), il tipo di applicazione che si creano e si ha familiarità con l'approccio di sviluppo.

Di seguito viene fornita una panoramica dei diversi Framework e alcune idee su come scegliere tra di essi. Se si preferisce un video di introduzione, vedere [effettua siti Web con ASP.NET](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET) e [Novità degli strumenti Web?](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | Se si ha esperienza | Stile di sviluppo | Esperienza | 
|-----------|----------------------|-----------------------------------------------------|----------------|
| Web Form | Windows Form, WPF, .NET | Sviluppo rapido utilizzando una libreria completa di controlli che incapsulano codice HTML | Sviluppo rapido di applicazioni di livello intermedio, avanzate |
| MVC       | Ruby su Guide, .NET  | Controllo completo su markup HTML, codice e markup separati e più semplice la scrittura di test. La scelta migliore per le applicazioni per dispositivi mobili e a pagina singola (SPA). | Livello intermedio, avanzate |
| Pagine Web  | Classica ASP, PHP     | Markup HTML e il codice insieme nello stesso file | Nuovi, di livello intermedio |

### <a name="web-forms"></a>Web Form

Con Web Form ASP.NET, è possibile compilare siti Web dinamici usando un comune modello di trascinamento e rilascio, basato su eventi. Un'area di progettazione e centinaia di controlli e componenti che consentono di creare rapidamente potenti siti basati su interfaccia utente con accesso ai dati. 

[Altre informazioni su Web Form](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC offre un modo potente, basato su modelli per creare siti Web dinamici che consentono una netta separazione delle problematiche e che consente il controllo completo su markup per uno sviluppo semplice e agile. ASP.NET MVC include numerose funzionalità che consentono uno sviluppo rapido e basato su per la creazione di applicazioni complesse che utilizzano gli standard web più recenti. 

[Altre informazioni su MVC](mvc/index.md)

### <a name="aspnet-web-pages"></a>Pagine Web ASP.NET

ASP.NET Web Pages e la sintassi Razor forniscono un modo rapido, accessibile e semplice per combinare codice lato server con HTML per creare contenuto web dinamico. Connettersi ai database, aggiungere video, collegarsi a siti di social networking e includere molte altre funzionalità che consentono di creare siti interessanti conformi agli standard web più recenti.

[Ulteriori informazioni sulle pagine Web](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Note su Web Form, MVC e pagine Web

Tutti e tre i framework ASP.NET sono basati su .NET Framework e condividono le funzionalità di base di .NET e di ASP.NET. Ad esempio, tutti e tre i Framework offrono un modello di sicurezza di accesso basato sulle appartenenze e tutte e tre condividono le stesse funzionalità per la gestione delle richieste, gestione delle sessioni e così via che fanno parte dei principali funzionalità ASP.NET.

Inoltre, tre strutture non sono completamente indipendenti, e la scelta di uno non preclude utilizzando un altro. Poiché i Framework possono coesistere nella stessa applicazione web, non è insolito per visualizzare i singoli componenti delle applicazioni scritti usando Framework diversi. Parti orientati ai clienti di un'app, ad esempio, potrebbero essere sviluppate in MVC per ottimizzare il markup, mentre l'accesso ai dati e parti amministrative vengono sviluppate in Web Form per sfruttare i controlli di dati e l'accesso ai dati semplice.

## <a name="web-apis"></a>API Web

API Web ASP.NET è un framework che rende più semplice compilare servizi HTTP in grado di raggiungono una vasta gamma di client, inclusi browser e dispositivi mobili. API Web ASP.NET è la piattaforma ideale per compilare applicazioni RESTful in .NET Framework.

[Altre informazioni sull'API Web](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>Tecnologie in tempo reale

ASP.NET SignalR è una nuova libreria per sviluppatori ASP.NET che facilita la funzionalità di sviluppo web in tempo reale. SignalR consente la comunicazione bidirezionale tra server e client. Server possono eseguire il push del contenuto ai client connessi immediatamente appena sarà disponibile. Supporta i WebSocket SignalR e tenti di altre tecniche per browser meno recenti compatibili. SignalR include le API per la gestione connessione (ad esempio, connettere e disconnettere eventi), il raggruppamento delle connessioni e le autorizzazioni.

[Altre informazioni su SignalR](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>Siti e applicazioni per dispositivi mobili 

ASP.NET è possibile accedere con un back-end Web API, nonché di siti web mobili utilizzando il framework di progettazione reattiva come Twitter Bootstrap native per dispositivi mobili. Se si sta compilando una app native per dispositivi mobili, è facile creare un'API Web basato su JSON per l'accesso ai dati handle, l'autenticazione e le notifiche push per l'app. Se si siano compilando un reattivo siti per dispositivi mobili, è possibile utilizzare qualsiasi framework CSS o un sistema di griglia aperta si preferisce, oppure seleziona un potente sistema mobile come jQuery Mobile o Sencha e applicazioni per dispositivi mobili con PhoneGap.

[Ulteriori informazioni sullo sviluppo di app e del sito per dispositivi mobili](mobile/index.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>Applicazioni a pagina singola 

Applicazione a pagina singola di ASP.NET (SPA) consente di compilare applicazioni che includono significativo sul lato client, le interazioni con HTML 5, 3 CSS e JavaScript. Visual Studio include un modello per la compilazione di applicazioni a pagina singola utilizzando knockout.js e ASP.NET Web API. Oltre al modello SPA predefinito, sono inoltre disponibili per il download modelli SPA creati dalla community.

[Altre informazioni sullo sviluppo di app a pagina singola](single-page-application/index.md)

## <a name="webhooks"></a>Webhook

Webhook sono un semplice modello HTTP fornisce un modello di pubblicazione/sottoscrizione semplice per collegare tra loro servizi API Web e SaaS. Quando si verifica un evento in un servizio, viene inviata una notifica sotto forma di una richiesta HTTP POST per i sottoscrittori registrati. La richiesta POST contiene informazioni sull'evento che rende possibile per il ricevitore di agire di conseguenza.

Webhook esposte da un numero elevato di servizi, inclusi Dropbox, GitHub, Instagram, MailChimp, PayPal, Slack, Trello e molto altro ancora. Ad esempio, un WebHook può indicare che un file è stato modificato in Dropbox, è stato eseguito il commit di una modifica del codice in GitHub, o un pagamento è stato avviato in PayPal o è stata creata una scheda in Trello.

[Altre informazioni su Webhook](webhooks/index.md)





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
