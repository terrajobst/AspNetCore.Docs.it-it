---
uid: identity/overview/getting-started/introduction-to-aspnet-identity
title: Introduzione all'identità ASP.NET | Documenti Microsoft
author: jongalloway
description: Il sistema di appartenenze ASP.NET è stata introdotta con ASP.NET 2.0 nel 2005 e, poiché quindi typicall di applicazioni web di modi sono state apportate molte modifiche...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 38717fc1-5989-43cf-952d-4007cc1dd923
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/introduction-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 59272f4659256e108ee99b22eb3bd3e2583a617c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="introduction-to-aspnet-identity"></a>Introduzione all'identità di ASP.NET
====================
dal [Jon Galloway](https://github.com/jongalloway), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

> Il sistema di appartenenze ASP.NET è stata introdotta con ASP.NET 2.0 nel 2005 e, poiché quindi le modalità con le applicazioni web in genere di gestire l'autenticazione e autorizzazione sono state apportate molte modifiche. Identità di ASP.NET è un nuovo approccio alla quale deve essere il sistema di appartenenze quando si compilano applicazioni moderne per il web, un telefono o tablet.
> 
> In questo articolo è stato scritto dal Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Jon Galloway ([@jongalloway](https://twitter.com/jongalloway)), Tom Dykstra e Rick Anderson ([ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="background-membership-in-aspnet"></a>Sfondo: L'appartenenza ASP.NET

### <a name="aspnet-membership"></a>Appartenenza ASP.NET

[L'appartenenza ASP.NET](https://msdn.microsoft.com/library/yh26yfzy(v=VS.100).aspx) è stato progettato per soddisfare i requisiti di appartenenza sito comuni nel 2005, che comportano autenticazione basata su form e un database di SQL Server per i nomi utente, password e i dati di profilo. Oggi è una matrice di dimensioni molto più ampia di opzioni di archiviazione di dati per le applicazioni web e la maggior parte degli sviluppatori desiderano abilitare i siti di utilizzare il provider di identità di social networking per la funzionalità di autenticazione e autorizzazione. Le limitazioni di progettazione dell'appartenenza ASP.NET rendono difficile la transizione:

- Lo schema del database è stato progettato per SQL Server e non è possibile modificare. È possibile aggiungere informazioni sul profilo, ma i dati aggiuntivi vengono compressi in una tabella diversa, che rende difficile per l'accesso con qualsiasi mezzo, ad eccezione tramite l'API del Provider di profilo.
- Il sistema del provider consente di modificare l'archivio dati sottostante, ma il sistema viene progettato attorno a presupposti appropriati per un database relazionale. È possibile scrivere un provider per archiviare le informazioni di appartenenza in un meccanismo di archiviazione non relazionali, ad esempio tabelle di archiviazione di Azure, ma è necessario risolvere la struttura relazionale mediante la scrittura di una grande quantità di codice e molte `System.NotImplementedException` eccezioni per i metodi che non applicare ai database NoSQL.
- Poiché la funzionalità log in/log out si basa sull'autenticazione basata su form, il sistema di appartenenze non può usare [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md). OWIN include i componenti middleware per l'autenticazione, incluso il supporto per i provider di identità esterno (ad esempio Accounts Microsoft, Facebook, Google, Twitter) utilizzando l'account di accesso e di log aggiuntivi usando gli account aziendali da Active Directory locale o Azure Active Directory. OWIN include inoltre il supporto per OAuth 2.0 e JWT CORS.

### <a name="aspnet-simple-membership"></a>Appartenenza ASP.NET semplici

[Appartenenza semplice ASP.NET](../../../web-pages/overview/security/16-adding-security-and-membership.md) è stato sviluppato come un sistema di appartenenze di ASP.NET Web Pages. È stata rilasciata con WebMatrix e Visual Studio 2010 SP1. L'obiettivo di appartenenza semplice era rendono più semplice aggiungere funzionalità di appartenenza a un'applicazione Web Pages.

Appartenenza semplice rendere più semplice personalizzare le informazioni utente, ma ancora condivide gli altri problemi con l'appartenenza ASP.NET e presenta alcune limitazioni:

- È difficile da rendere persistenti i dati di sistema di appartenenza in un archivio non relazionali.
- È possibile utilizzarlo con OWIN.
- Non funziona correttamente con il provider di appartenenze ASP.NET esistenti e non è estendibile.

### <a name="aspnet-universal-providers"></a>Provider universali di ASP.NET

[ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) sono stati sviluppati per consentono di rendere persistenti le informazioni di appartenenza in Microsoft Azure SQL Database e funzionano anche con SQL Server Compact. I provider universali sono stati compilati su Entity Framework Code First, che significa che i provider universali possono essere usati per rendere persistenti i dati in qualsiasi archivio supportata da Entity Framework. Con i provider universali, sono stato eliminato numerose anche lo schema del database.

I provider universali sono basati sull'infrastruttura di appartenenza ASP.NET, che comportano comunque le stesse limitazioni di Provider di SqlMembership. Vale a dire che sono state progettate per i database relazionali ed è difficile personalizzare le informazioni del profilo e l'utente. Questi provider usare ancora autenticazione basata su form per la funzionalità di accesso e di log.

## <a name="aspnet-identity"></a>Identità ASP.NET

Come l'appartenenza storia in ASP.NET è stato migliorato nel corso degli anni, il team di ASP.NET ha appreso molto da commenti e suggerimenti dei clienti.

Si presuppone che gli utenti avranno accesso immettendo un nome utente e una password registrati in un'applicazione non è più valido. Il web è diventato più sociale. Gli utenti interagiscono tra loro in tempo reale tramite canali di social networking, ad esempio Facebook, Twitter e ad altri siti web sociali. Gli sviluppatori che gli utenti in grado di accedere con le identità di social networking, in modo che abbiano una ricca esperienza nei loro siti web. Un sistema di appartenenze moderna necessario abilitare l'account di accesso basata sul reindirizzamento al provider di autenticazione, ad esempio Facebook, Twitter e altri.

Come lo sviluppo web, pertanto, non i modelli di sviluppo web. Unit test del codice dell'applicazione è diventato un problema di base per gli sviluppatori di applicazioni. 2008 ASP.NET ha aggiunto un nuovo framework in base al modello Model-View-Controller (MVC), in parte per consentire agli sviluppatori di applicazioni ASP.NET testabili unità di compilazione. Gli sviluppatori che desiderano unit test di logica dell'applicazione a cui si desidera inoltre in grado di eseguire questa operazione con il sistema di appartenenze.

Considerando le modifiche nello sviluppo di applicazioni web, ASP.NET Identity è stata sviluppata con gli obiettivi seguenti:

- **Un sistema di identità di ASP.NET**

    - ASP.NET Identity può essere utilizzato con tutti i framework ASP.NET, ad esempio ASP.NET MVC, Web Form, pagine Web, Web API e SignalR.
    - Identità di ASP.NET può essere utilizzata quando si compilano applicazioni web, telefono, archivio o ibrida.
- **Facilità di inserimento di dati del profilo relativi all'utente**

    - Si possono controllare lo schema dell'utente e informazioni sul profilo. Ad esempio, è possibile abilitare facilmente il sistema archiviare le date di nascita immesse dagli utenti quando registrano un account nell'applicazione.

- **Controllo di persistenza**

    - Per impostazione predefinita, il sistema di identità ASP.NET archivia tutte le informazioni utente in un database. Identità di ASP.NET utilizza Code First di Entity Framework per implementare tutti un meccanismo di persistenza.
    - Poiché si controllano di schema del database, le attività comuni, ad esempio la modifica dei nomi di tabella o la modifica il tipo di dati di chiavi primarie è semplice.
    - È facile inserire i meccanismi di archiviazione diversi, ad esempio SharePoint, servizio tabella di archiviazione di Azure, nei database NoSQL, e così via, senza dover generare `System.NotImplementedExceptions` eccezioni.
- **Testabilità unità**

    - ASP.NET Identity rende l'applicazione web di altre unità verificabili. È possibile scrivere unit test per le parti dell'applicazione che utilizzano ASP.NET Identity.
- **Provider di ruoli**

    - È un provider di ruoli che consente di limitare l'accesso a parti dell'applicazione dai ruoli. Facilmente, è possibile creare ruoli, ad esempio "Admin" e aggiungere utenti ai ruoli.
- **Basata sulle attestazioni**

    - Identità di ASP.NET supporta l'autenticazione basata sulle attestazioni, in cui l'identità dell'utente è rappresentato come un set di attestazioni. Le attestazioni consentono agli sviluppatori di essere molto più espressivo descrivere l'identità dell'utente con più ruoli. Mentre l'appartenenza al ruolo è semplicemente un valore booleano (membro o membro non), un'attestazione può includere informazioni dettagliate sulle identità e l'appartenenza dell'utente.
- **Provider di accesso social networking**

    - Facilmente aggiungere account di accesso social networking, ad esempio Account Microsoft, Facebook, Twitter, Google e ad altri utenti nell'applicazione e archiviare i dati specifici dell'utente nell'applicazione.
- **Azure Active Directory**

    - È anche possibile aggiungere la funzionalità di accesso tramite Azure Active Directory e archiviare i dati specifici dell'utente nell'applicazione. Per ulteriori informazioni, vedere [account aziendali](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth) nella creazione di progetti Web ASP.NET in Visual Studio 2013
- **Integrazione di OWIN**

    - Autenticazione ASP.NET è basata su middleware OWIN che può essere usato in qualsiasi host basato su OWIN. Identità di ASP.NET non dispone di tutte le dipendenze in System. Web. È un framework OWIN completamente conforme e può essere utilizzato in qualsiasi applicazione OWIN ospitato.
    - Identità ASP.NET usa l'autenticazione OWIN per log in/log out di utenti nel sito web. Ciò significa che anziché FormsAuthentication per generare il cookie, l'applicazione utilizza OWIN CookieAuthentication a tale scopo.
- **Pacchetto NuGet**

    - ASP.NET Identity viene ridistribuito come pacchetto NuGet che viene installato nei modelli ASP.NET MVC, Web Form e l'API Web forniti con Visual Studio 2013. È possibile scaricare il pacchetto NuGet dalla raccolta NuGet.
    - Il rilascio di ASP.NET Identity come un NuGet pacchetto rende più semplice per il team ASP.NET eseguire l'iterazione su nuove funzionalità e correzioni di bug e il recapito per gli sviluppatori in modo agile.

## <a name="getting-started-with-aspnet-identity"></a>Introduzione a ASP.NET Identity

Identità di ASP.NET viene utilizzato nei modelli di progetto di Visual Studio 2013 per ASP.NET MVC, Web Form, API Web e SPA. In questa procedura dettagliata, si sarà viene illustrato come utilizzano ASP.NET Identity di modelli di progetto per aggiungere funzionalità a registrare, accedere e disconnettersi da un utente.

ASP.NET Identity viene implementata utilizzando la procedura seguente. Lo scopo di questo articolo è offrono una panoramica di alto livello dell'identità di ASP.NET. è possibile seguire dettagliatamente o solo leggere i dettagli. Per istruzioni più dettagliate sulla creazione di applicazioni utilizzando l'identità di ASP.NET, tra cui usando la nuova API per aggiungere gli utenti, ruoli e le informazioni sul profilo, vedere la sezione passaggi successivi alla fine di questo articolo.

1. Creare un'applicazione MVC ASP.NET con singoli account. È possibile utilizzare ASP.NET Identity in ASP.NET MVC, Web Form, API Web, e così via SignalR. In questo articolo si inizierà con un'applicazione MVC ASP.NET.  
  
    ![](introduction-to-aspnet-identity/_static/image1.png)
2. Il progetto creato contiene i seguenti tre pacchetti per l'identità ASP.NET.

    - [`Microsoft.AspNet.Identity.EntityFramework`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)  
   Questo pacchetto è l'implementazione di Entity Framework di ASP.NET Identity, che verranno mantenuti i dati di ASP.NET Identity e schema a SQL Server.
    - [`Microsoft.AspNet.Identity.Core`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Core/)  
   Questo pacchetto contiene le interfacce di base per ASP.NET Identity. Questo pacchetto può essere usato per scrivere un'implementazione per ASP.NET Identity che persistenza diverse destinazioni vengono archiviate come tabella di archiviazione di Azure, NoSQL database e così via.
    - [`Microsoft.AspNet.Identity.OWIN`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Owin/)  
   Questo pacchetto contiene funzionalità che vengono utilizzati per collegare l'autenticazione OWIN con identità di ASP.NET nelle applicazioni ASP.NET. Viene utilizzato quando si aggiunge log funzionalità all'applicazione e una chiamata nel middleware di autenticazione dei Cookie OWIN per generare un cookie.
3. Creazione di un utente.  
   Avviare l'applicazione e quindi fare clic su di **registrare** collegamento per creare un utente. La figura seguente mostra la pagina di registrazione che raccoglie il nome utente e password.  
  
    ![](introduction-to-aspnet-identity/_static/image2.png)  
  
   Quando l'utente sceglie il **registrare** pulsante, il `Register` azione del controller di Account consente all'utente chiamando l'API di identità di ASP.NET, come evidenziato qui sotto:

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample1.cs?highlight=8-9)]
4. Accedi.  
   Se l'utente è stato creato correttamente, viene registrata tramite il `SignInAsync` metodo.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample2.cs?highlight=12)]

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample3.cs?highlight=5-6)]

   Nel codice evidenziato sopra il `SignInAsync` metodo genera un [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). Poiché ASP.NET Identity e OWIN Cookie di autenticazione è basata sulle attestazioni di sistema, il framework richiede che l'app per generare un oggetto ClaimsIdentity per l'utente. ClaimsIdentity dispone di informazioni su tutte le attestazioni per l'utente, ad esempio i ruoli a cui appartiene l'utente. È anche possibile aggiungere ulteriori attestazioni per l'utente in questa fase.  
  
   Nel codice evidenziato di seguito il `SignInAsync` metodo accede l'utente tramite AuthenticationManager da OWIN e chiamare il metodo `SignIn` e passando l'oggetto ClaimsIdentity.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample4.cs?highlight=8-11)]
5. Esegue la disconnessione.  
   Fare clic su di **disconnettersi** collegamento chiama l'azione di disconnessione nel controller di account. 

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample5.cs?highlight=6)]

   Codice evidenziata precedente OWIN `AuthenticationManager.SignOut` metodo. Questo comportamento è analogo a [FormsAuthentication. SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) utilizzato dal metodo di [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modulo nel Web Form.

## <a name="components-of-aspnet-identity"></a>Componenti di ASP.NET Identity

Nel diagramma seguente vengono illustrati i componenti del sistema di identità di ASP.NET (fare clic su [questo](introduction-to-aspnet-identity/_static/image3.png) o nel diagramma per allargarla). I pacchetti in verde è costituiscono il sistema di identità di ASP.NET. Tutti gli altri pacchetti sono le dipendenze necessarie per utilizzare il sistema di identità di ASP.NET nelle applicazioni ASP.NET.

[![](introduction-to-aspnet-identity/_static/image5.png)](introduction-to-aspnet-identity/_static/image4.png)

Di seguito è fornita una breve descrizione dei pacchetti NuGet non indicato in precedenza:

- [Microsoft.Owin.Security.Cookies](http://www.nuget.org/packages/Microsoft.Owin.Security.Cookies/)  
 L'autenticazione, simile a ASP basata su middleware che consente a un'applicazione può utilizzare i cookie. Autenticazione basata su form del NET.
- [EntityFramework](http://www.nuget.org/packages/EntityFramework/)  
 Entity Framework è una tecnologia di accesso ai dati consigliati di Microsoft per i database relazionali.

## <a name="migrating-from-membership-to-aspnet-identity"></a>La migrazione dall'appartenenza di ASP.NET Identity

Ci auguriamo che fornire non appena le indicazioni sulla migrazione delle applicazioni esistenti che utilizzano l'appartenenza di ASP.NET o appartenenza semplice per il nuovo sistema di identità di ASP.NET.

## <a name="next-steps"></a>Passaggi successivi

- [Creare un'applicazione MVC ASP.NET 5 con Facebook, Google OAuth2 e OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)  
 L'esercitazione Usa l'API di identità di ASP.NET per aggiungere informazioni di profilo per il database utente e su come eseguire l'autenticazione con Google e Facebook.
- [Creare un'applicazione MVC ASP.NET con autenticazione e il database di SQL Server e distribuire in Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)  
 In questa esercitazione viene illustrato come utilizzare l'API di identità per aggiungere utenti e ruoli.
- [Account utente individuali](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#indauth) nella creazione di progetti Web ASP.NET in Visual Studio 2013
- [Gli account aziendali](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth) nella creazione di progetti Web ASP.NET in Visual Studio 2013
- [Personalizzazione delle informazioni del profilo in ASP.NET Identity nei modelli di Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)
- [Ottenere informazioni dai provider Social utilizzato nei modelli di progetto di Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [https://github.com/rustd/AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample)  
 Applicazione di esempio che illustra come aggiungere i ruoli di base e supporto per l'utente e eseguire i ruoli e gestione degli utenti.
