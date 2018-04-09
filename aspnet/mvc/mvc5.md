---
uid: mvc/mvc5
title: ASP.NET MVC 5 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 5 ASP.NET MVC 5 è un framework per la compilazione di applicazioni web scalabili e basate su standard utilizzando schemi progettuali ben definiti e la potenza di AS....
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: f79fbf7f-59e5-4279-a832-c1a0294630f4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/mvc5
msc.type: content
ms.openlocfilehash: 1b3f920b51a70757ec0e20e36fa8e7dc329e663d
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
<a name="aspnet-mvc-5"></a>ASP.NET MVC 5
====================
## <a name="whats-new-in-aspnet-mvc-5"></a>Novità di ASP.NET MVC 5

### <a name="one-aspnet"></a>Uno di ASP.NET

I modelli di progetto Web MVC integrano con la nuova esperienza di ASP.NET uno. È possibile personalizzare il progetto MVC e configurare l'autenticazione mediante la creazione guidata progetto ASP.NET uno. Un'esercitazione introduttiva su ASP.NET MVC 5 è reperibile in [Introduzione a ASP.NET MVC 5](overview/getting-started/introduction/getting-started.md).

Per informazioni sull'aggiornamento di progetti MVC 4 e 5 MVC, vedere [come aggiornare un ASP.NET MVC 4 e il progetto API Web per ASP.NET MVC 5 e l'API Web 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>Identità ASP.NET

I modelli di progetto MVC sono stati aggiornati per l'utilizzo di ASP.NET Identity per l'autenticazione e la gestione delle identità. Un'esercitazione che presenta l'autenticazione di Facebook e Google e la nuova API di appartenenza è reperibile in [creare un'App di ASP.NET MVC 5 con Facebook, Google OAuth2 e OpenID Sign-on](overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) e [distribuire un'app protetta ASP.NET MVC con L'appartenenza, OAuth e il Database SQL in un sito Web di Azure Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

### <a name="bootstrap"></a>Bootstrap

Il modello di progetto MVC è stato aggiornato per utilizzare [Bootstrap](http://getbootstrap.com/) per fornire un elegante e reattiva aspetto che è possibile personalizzare facilmente. Per ulteriori informazioni, vedere [Bootstrap nei modelli di progetto web di Visual Studio 2013](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtri di autenticazione

[Filtri di autenticazione](http://www.dotnetcurry.com/showarticle.aspx?ID=957) sono un nuovo tipo di filtro in ASP.NET MVC che vengono eseguiti prima di filtri di autorizzazione nella pipeline ASP.NET MVC e consentono di specificare l'autenticazione della logica per ogni azione, per ogni controller o a livello globale per tutti i controller. Filtri di autenticazione nella richiesta di credenziali di elaborare e forniscono un'entità corrispondente. Filtri di autenticazione possono anche aggiungere richieste di autenticazione in risposta alle richieste non autorizzate. Vedere [filtri di autenticazione ASP.NET MVC 5](http://www.dotnetcurry.com/showarticle.aspx?ID=957), [filtri di autenticazione in ASP.NET MVC 5](http://theshravan.net/blog/authentication-filters-in-asp-net-mvc-5/).

### <a name="filter-overrides"></a>Esegue l'override di filtro

È ora possibile sostituire i filtri da applicare a un metodo di azione specificato o un controller, specificando un [override filtro](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5). I filtri di sostituzione è specificare un set di tipi di filtro che non devono essere eseguiti per un determinato ambito (azione o controller). Ciò consente di configurare i filtri che si applicano globalmente ma quindi escludere alcuni filtri globali dall'applicazione per azioni specifiche o controller. Vedere [nuovo filtro esegue l'override di funzionalità in ASP.NET MVC 5 e ASP.NET Web API 2](https://weblogs.asp.net/imranbaloch/archive/2013/09/25/new-filter-overrides-in-asp-net-mvc-5-and-asp-net-web-api-2.aspx), [illustrato come utilizzare la funzionalità di esegue l'override di filtro di ASP.NET MVC 5](http://hackwebwith.net/how-to-use-the-asp-net-mvc-5-filter-overrides-feature/), e [esegue l'override di filtri in ASP.NET MVC 5](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)

### <a name="attribute-routing"></a>Routing con attributi

ASP.NET MVC sono ora supportate [attributo routing](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx), grazie a un contributo Tim McCall, l'autore del [ http://attributerouting.net ](http://attributerouting.net). Con il routing di attributo è possibile specificare i percorsi di annotando azioni e controller.

## <a name="new-web-project-experience"></a>Nuova esperienza di progetto Web

È stata migliorata l'esperienza di creazione di nuovi progetti web in Visual Studio 2013. Nel **nuovo progetto Web ASP.NET** finestra di dialogo è possibile selezionare il tipo di progetto si desidera Configura qualsiasi combinazione di tecnologie (Web Form, MVC, Web API), configurare le opzioni di autenticazione e aggiungere un progetto di unit test.

![Nuovo progetto ASP.NET](mvc5/_static/image1.png)

La nuova finestra di dialogo consente di modificare le opzioni di autenticazione predefinito per la maggior parte dei modelli. Ad esempio, quando si crea un progetto di Web Form ASP.NET è possibile selezionare una delle opzioni seguenti:

- Nessuna autenticazione
- Account utente (appartenenza ASP.NET o provider social Accedi)
- Account aziendale (Active Directory in un'applicazione internet)
- Autenticazione di Windows (Active Directory in un'applicazione intranet)

![Opzioni di autenticazione](mvc5/_static/image2.png)

Per ulteriori informazioni sul processo di nuovo per la creazione di progetti web, vedere [creazione di progetti Web ASP.NET in Visual Studio 2013](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md). Per ulteriori informazioni sulle nuove opzioni di autenticazione, vedere [ASP.NET Identity](../identity/overview/index.md).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>Scaffolding di ASP.NET

Lo Scaffolding di ASP.NET è un framework di generazione di codice per applicazioni Web ASP.NET. Che consente di aggiungere il codice di boilerplate al progetto che interagisce con un modello di dati.

Nelle versioni precedenti di Visual Studio, lo scaffolding è limitato ai progetti ASP.NET MVC. Con Visual Studio 2013, è ora possibile usare lo scaffolding per qualsiasi progetto ASP.NET, inclusi Web Form. Visual Studio 2013 non supporta attualmente la generazione di pagine per un progetto di Web Form, ma è comunque possibile utilizzare lo scaffolding con Web Form aggiungendo le dipendenze MVC al progetto. Verrà aggiunto il supporto per la generazione di pagine Web Form in un aggiornamento futuro.

Quando si usa lo scaffolding, è assicurare che tutti obbligatori le dipendenze siano installate nel progetto. Ad esempio, se si avvia con un progetto di Web Form ASP.NET e quindi Usa lo scaffolding per aggiungere un Controller API Web, i pacchetti NuGet necessari e i riferimenti vengono aggiunti al progetto automaticamente.

Per aggiungere lo scaffolding di MVC a un progetto di Web Form, aggiungere un **nuovo elemento di scaffolding** e selezionare **MVC 5 dipendenze** nella finestra di dialogo. Sono disponibili due opzioni per lo scaffolding MVC; Minimo e completo. Se si seleziona minima, solo i pacchetti NuGet e i riferimenti per ASP.NET MVC vengono aggiunti al progetto. Se si seleziona l'opzione completo, vengono aggiunte le dipendenze minime, nonché i file di contenuto richiesti per un progetto MVC.

Supporto per lo scaffolding controller asincrono utilizza le nuove funzionalità asincrone di Entity Framework 6.

Per ulteriori informazioni ed esercitazioni, vedere [Panoramica lo Scaffolding di ASP.NET](../visual-studio/overview/2013/aspnet-scaffolding-overview.md).

### <a name="getting-help-and-reporting-issues"></a>Ottenere informazioni della Guida e segnalare eventuali problemi

- [Problemi noti ed elenco di modifiche di rilievo](../visual-studio/overview/2013/release-notes.md#knownissues)
- Visualizzare la Guida e discutere ASP.NET MVC 5 nel [forum](https://forums.asp.net/1146.aspx)
- [Segnalare un bug in ASP.NET MVC 5](https://github.com/aspnet/AspNetWebStack/issues)
- [Effettuare una richiesta di funzionalità](http://aspnet.uservoice.com/forums/41201-asp-net-mvc)

### <a name="upgrading-from-aspnet-mvc-4"></a>L'aggiornamento da MVC ASP.NET 4

Vedere [come aggiornare un ASP.NET MVC 4 e progetto API Web API 2 e MVC ASP.NET 5 Web](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)
