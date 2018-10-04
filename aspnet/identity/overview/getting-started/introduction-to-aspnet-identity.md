---
uid: identity/overview/getting-started/introduction-to-aspnet-identity
title: Introduzione ad ASP.NET Identity | Microsoft Docs
author: jongalloway
description: Il sistema di appartenenze ASP.NET è stata introdotta con ASP.NET 2.0 il 2005 e, poiché quindi le richiede in genere le applicazioni web modi sono state apportate numerose modifiche...
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 38717fc1-5989-43cf-952d-4007cc1dd923
msc.legacyurl: /identity/overview/getting-started/introduction-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 1938de2b57c8fafa7ea8a656c0a42d2d3f1a6c81
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577873"
---
<a name="introduction-to-aspnet-identity"></a>Introduzione ad ASP.NET Identity
====================
dal [Jon Galloway](https://github.com/jongalloway), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

> Il sistema di appartenenze ASP.NET è stata introdotta con ASP.NET 2.0 il 2005 e, poiché quindi sono state apportate molte modifiche nei modi in cui le applicazioni web gestiscono in genere l'autenticazione e autorizzazione. ASP.NET Identity è un nuovo approccio alla cosa deve essere il sistema di appartenenze quando crei applicazioni moderne per il web, telefono o tablet.
> 
> Questo articolo è stato scritto di Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Jon Galloway ([@jongalloway](https://twitter.com/jongalloway)), Tom Dykstra e Rick Anderson ([ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="background-membership-in-aspnet"></a>Sfondo: Appartenenza ASP.NET

### <a name="aspnet-membership"></a>Appartenenza ASP.NET

[Sistema di appartenenze ASP.NET](https://msdn.microsoft.com/library/yh26yfzy(v=VS.100).aspx) è stato progettato per soddisfare i requisiti di appartenenza sito comuni nel 2005, tra cui coinvolta l'autenticazione basata su form e un database di SQL Server per i nomi utente, password e i dati del profilo. Oggi è presente una più ampia gamma di opzioni di archiviazione dei dati per le applicazioni web, mentre la maggior parte degli sviluppatori desiderano abilitare i relativi siti di usare provider di identità di social networking per la funzionalità di autenticazione e autorizzazione. Le limitazioni di progettazione dell'appartenenza ASP.NET rendono questa transizione è difficile:

- Lo schema del database è stato progettato per SQL Server e non è possibile modificarla. È possibile aggiungere le informazioni del profilo, ma i dati aggiuntivi vengono compressi in una tabella diversa, cosa che rende difficile per l'accesso con qualsiasi mezzo, ad eccezione tramite l'API del Provider del profilo.
- Il sistema del provider consente di modificare l'archivio dati sottostante, ma il sistema è progettato sulla base di supposizioni appropriati per un database relazionale. È possibile scrivere un provider per archiviare le informazioni di appartenenza in un meccanismo di archiviazione non relazionale, ad esempio tabelle di archiviazione di Azure, ma è necessario risolvere la struttura relazionale mediante la scrittura di una grande quantità di codice e molte `System.NotImplementedException` eccezioni per i metodi che non si applicano a database NoSQL.
- Poiché la funzionalità di log-in/log-out è basata su autenticazione basata su form, non è possibile usare il sistema di appartenenze [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md). OWIN include i componenti middleware per l'autenticazione, incluso il supporto per gli accessi tramite provider di identità esterni (ad esempio Accounts Microsoft, Facebook, Google, Twitter) e gli accessi tramite un account dell'organizzazione di Active Directory locale o Azure Active Directory. OWIN include anche il supporto per OAuth 2.0, token JWT e la condivisione CORS.

### <a name="aspnet-simple-membership"></a>Appartenenza ASP.NET semplice

[Sistema di appartenenze ASP.NET semplice](../../../web-pages/overview/security/16-adding-security-and-membership.md) è stato sviluppato come un sistema di appartenenze di ASP.NET Web Pages. È stato rilasciato con WebMatrix e Visual Studio 2010 SP1. L'obiettivo di appartenenza semplice è stata per renderlo semplice aggiungere funzionalità di appartenenza per un'applicazione Web Pages.

Appartenenza semplice è stata rendono più semplice personalizzare le informazioni del profilo utente, ma comunque condivide gli altri problemi con l'appartenenza ASP.NET e presenta alcune limitazioni:

- Era difficile da rendere persistenti i dati di sistema di appartenenza in un archivio non relazionali.
- È possibile utilizzarlo con OWIN.
- Non funziona bene con i provider di appartenenze ASP.NET esistenti e non è estensibile.

### <a name="aspnet-universal-providers"></a>Provider universali di ASP.NET

[ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) sono state sviluppate per consentono di rendere persistenti le informazioni di appartenenza a Microsoft, Database SQL di Azure e sono utilizzabili anche con SQL Server Compact. I provider universali sono stati compilati su Entity Framework Code First, che significa che i provider universali possono essere usati per rendere persistenti i dati in qualsiasi archivio supportato da Entity Framework. Con i provider universali, lo schema del database è stato pulito molto anche.

I provider universali sono basati sull'infrastruttura di appartenenza ASP.NET, quindi questi restano comunque le stesse limitazioni di SqlMembership Provider. Vale a dire, sono stati progettati per i database relazionali ed è difficile la personalizzazione del profilo e le informazioni utente. Questi provider comunque anche usano autenticazione basata su form per la funzionalità di accesso e di log-out.

## <a name="aspnet-identity"></a>Identità ASP.NET

Come l'appartenenza storia in ASP.NET si è evoluto negli anni, il team ASP.NET ha imparato molto dal feedback dei clienti.

Si presuppone che gli utenti che accederanno immettendo un nome utente e una password che si sono registrate nella propria applicazione non è più valido. Il web è diventata più basati su social network. Gli utenti interagiscono tra loro in tempo reale tramite canali social network come Facebook, Twitter e altri siti web basati su social network. Gli sviluppatori desidera che gli utenti siano in grado di accedere con le identità di social networking in modo che abbiano una ricca esperienza nei propri siti web. Un sistema di appartenenze moderne debba abilitare basata sul reindirizzamento gli accessi ai provider di autenticazione, ad esempio Facebook, Twitter e altri.

Come si è evoluta lo sviluppo web, fatto i modelli di sviluppo web. Unit test del codice dell'applicazione è diventato un problema di base per gli sviluppatori di applicazioni. Nel 2008 ASP.NET ha aggiunto un nuovo framework basato sul modello Model-View-Controller (MVC), in parte per aiutare gli sviluppatori di compilare applicazioni ASP.NET testabili unità. Gli sviluppatori che vogliono unit test la logica dell'applicazione desiderata inoltre essere in grado di eseguire l'operazione con il sistema di appartenenze.

Considerando queste modifiche nello sviluppo di applicazioni web, ASP.NET Identity è stata sviluppata con gli obiettivi seguenti:

- **Un sistema di identità ASP.NET**

    - ASP.NET Identity può essere utilizzato con tutti i framework ASP.NET, ad esempio ASP.NET MVC, Web Form, pagine Web, API Web e SignalR.
    - Identità ASP.NET può essere utilizzata quando si creano applicazioni web, telefono, archivio o ibrida.
- **Facilità di inserimento di dati del profilo relativi all'utente**

    - È possibile controllare lo schema dell'utente e le informazioni del profilo. Ad esempio, è possibile abilitare facilmente il sistema archiviare le date di nascita immesse dagli utenti quando registrano un account nell'applicazione.

- **Controllo di persistenza**

    - Per impostazione predefinita, il sistema di ASP.NET Identity archivia tutte le informazioni utente in un database. ASP.NET Identity utilizza Code First di Entity Framework per implementare tutti un meccanismo di persistenza.
    - Poiché si controllano i schema del database, le attività comuni, ad esempio modifica i nomi di tabella o la modifica il tipo di dati di chiavi primarie è semplice.
    - È facile inserire i meccanismi di archiviazione diverso, ad esempio SharePoint, servizio tabelle di archiviazione di Azure, i database NoSQL, e così via, senza dover generare `System.NotImplementedExceptions` eccezioni.
- **Testabilità unità**

    - ASP.NET Identity rende l'applicazione web di altre unità testabile. È possibile scrivere unit test per le parti dell'applicazione che usano ASP.NET Identity.
- **Provider di ruoli**

    - È disponibile un provider di ruoli che consente di limitare l'accesso a parti dell'applicazione dai ruoli. È facilmente possibile creare ruoli, ad esempio "Admin" e aggiungere utenti ai ruoli.
- **Basata sulle attestazioni**

    - ASP.NET Identity supporta l'autenticazione basata sulle attestazioni, in cui l'identità dell'utente viene rappresentato come un set di attestazioni. Le attestazioni consentono agli sviluppatori di essere molto più espressivo descrivere un'identità dell'utente rispetto a consentono di ruoli. Mentre l'appartenenza al ruolo è semplicemente un valore booleano (membro o non membro), un'attestazione può includere informazioni dettagliate sulle identità e appartenenza dell'utente.
- **Provider di accesso basati su social network**

    - È possibile facilmente aggiungere accessi basati su social network, ad esempio Account Microsoft, Facebook, Twitter, Google e altri utenti all'applicazione e archiviare i dati specifici dell'utente nell'applicazione.
- **Azure Active Directory**

    - È anche possibile aggiungere la funzionalità di accesso tramite Azure Active Directory e archiviare i dati specifici dell'utente nell'applicazione. Per altre informazioni, vedere [gli account aziendali](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth) nella creazione di progetti Web ASP.NET in Visual Studio 2013
- **Integrazione di OWIN**

    - Autenticazione ASP.NET ora si basa sul middleware OWIN che può essere usato in qualsiasi host basati su OWIN. ASP.NET Identity è privo di qualsiasi dipendenza in System. Web. È un framework OWIN completamente compatibile e può essere usato in qualsiasi applicazione OWIN ospitato.
    - ASP.NET Identity Usa l'autenticazione OWIN per log-in/log-out di utenti nel sito web. Ciò significa che anziché FormsAuthentication per generare il cookie, l'applicazione usa CookieAuthentication OWIN per eseguire questa operazione.
- **Pacchetto NuGet**

    - ASP.NET Identity viene ridistribuito come pacchetto NuGet che viene installato nei modelli di ASP.NET MVC, Web Form e API Web forniti con Visual Studio 2013. È possibile scaricare il pacchetto NuGet dalla raccolta NuGet.
    - Il rilascio di ASP.NET Identity come pacchetto NuGet package rende più semplice per il team di ASP.NET per eseguire l'iterazione su nuove funzionalità e correzioni di bug e offrire soluzioni per gli sviluppatori in modo agile.

## <a name="getting-started-with-aspnet-identity"></a>Introduzione a ASP.NET Identity

ASP.NET Identity viene utilizzata nei modelli di progetto Visual Studio 2013 per ASP.NET MVC, Web Form, API Web e applicazione a singola pagina. In questa procedura dettagliata verrà illustrato come i modelli di progetto usano ASP.NET Identity per aggiungere funzionalità a registrare, accedere e disconnettere un utente.

ASP.NET Identity viene implementato usando la procedura seguente. Lo scopo di questo articolo è fornire una panoramica generale dell'identità di ASP.NET. è possibile seguire esattamente ogni passaggio o semplicemente leggere i dettagli. Per istruzioni più dettagliate sulla creazione di App scritte in ASP.NET Identity, incluso l'uso la nuova API per aggiungere gli utenti, ruoli e le informazioni sul profilo, vedere la sezione passaggi successivi alla fine di questo articolo.

1. Creare un'applicazione ASP.NET MVC con singoli account. È possibile usare ASP.NET Identity in ASP.NET MVC, Web Form, API Web, SignalR e così via. In questo articolo si inizierà con un'applicazione ASP.NET MVC.  
  
    ![](introduction-to-aspnet-identity/_static/image1.png)
2. Il progetto creato contiene i seguenti tre pacchetti per ASP.NET Identity.

    - [`Microsoft.AspNet.Identity.EntityFramework`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)  
   Questo pacchetto include l'implementazione di Entity Framework di ASP.NET Identity verranno salvati in modo permanente i dati di ASP.NET Identity e lo schema in SQL Server.
    - [`Microsoft.AspNet.Identity.Core`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Core/)  
   Questo pacchetto include le interfacce di base per ASP.NET Identity. Questo pacchetto può essere utilizzato per scrivere un'implementazione per ASP.NET Identity che persistenza diverse destinazioni archivi, ad esempio archiviazione tabelle di Azure, database NoSQL database e così via.
    - [`Microsoft.AspNet.Identity.OWIN`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Owin/)  
   Questo pacchetto contiene funzionalità che consente di collegare l'autenticazione OWIN con ASP.NET Identity nelle applicazioni ASP.NET. Viene utilizzato quando si aggiungono log funzionalità all'applicazione e richiama nel middleware di autenticazione dei Cookie OWIN per generare un cookie.
3. Creazione di un utente.  
   Avviare l'applicazione e quindi fare clic sui **registrare** collegamento per creare un utente. L'immagine seguente mostra la pagina di registrazione che consente di raccogliere il nome utente e password.  
  
    ![](introduction-to-aspnet-identity/_static/image2.png)  
  
   Quando l'utente sceglie il **registrare** pulsante, il `Register` azione del controller di Account consente di creare l'utente chiamando l'API di identità di ASP.NET, come evidenziato di seguito:

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample1.cs?highlight=8-9)]
4. Accedi.  
   Se l'utente è stato creato correttamente, viene registrata dal `SignInAsync` (metodo).  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample2.cs?highlight=12)]

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample3.cs?highlight=5-6)]

   Nel codice evidenziato sopra il `SignInAsync` metodo genera un [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). Poiché ASP.NET Identity e l'autenticazione tramite Cookie OWIN sono basate sulle attestazioni di sistema, il framework richiede l'app genererà un oggetto ClaimsIdentity per l'utente. Oggetto ClaimsIdentity offre informazioni su tutte le attestazioni dell'utente, ad esempio i ruoli a cui appartiene l'utente. È anche possibile aggiungere altre attestazioni dell'utente in questa fase.  
  
   Nel codice evidenziato di seguito il `SignInAsync` metodo esegue l'accesso dell'utente tramite AuthenticationManager da OWIN e chiamando il metodo `SignIn` e passando l'oggetto ClaimsIdentity.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample4.cs?highlight=8-11)]
5. Esegue la disconnessione.  
   Facendo clic sui **disconnettersi** collegamento chiama l'azione di disconnessione nel controller account. 

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample5.cs?highlight=6)]

   Il codice evidenziato riportato in precedenza mostra OWIN `AuthenticationManager.SignOut` (metodo). Questo comportamento è analogo a [FormsAuthentication. SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) metodo utilizzato per il [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) modulo in Web Form.

## <a name="components-of-aspnet-identity"></a>Componenti di ASP.NET Identity

Il diagramma seguente illustra i componenti del sistema di identità di ASP.NET (fare clic su [ciò](introduction-to-aspnet-identity/_static/image3.png) o del diagramma per ingrandirlo). I pacchetti in verde rappresentano il sistema di identità di ASP.NET. Tutti gli altri pacchetti sono dipendenze necessarie per usare il sistema di identità di ASP.NET in applicazioni ASP.NET.

[![](introduction-to-aspnet-identity/_static/image5.png)](introduction-to-aspnet-identity/_static/image4.png)

Di seguito è una breve descrizione dei pacchetti NuGet non indicato in precedenza:

- [Microsoft.Owin.Security.Cookies](http://www.nuget.org/packages/Microsoft.Owin.Security.Cookies/)  
 L'autenticazione, simile a ASP basata su middleware che consente a un'applicazione di usare i cookie. Autenticazione basata su form di NET.
- [EntityFramework](http://www.nuget.org/packages/EntityFramework/)  
 Entity Framework è una tecnologia di accesso ai dati consigliati di Microsoft per i database relazionali.

## <a name="migrating-from-membership-to-aspnet-identity"></a>Eseguire la migrazione dall'appartenenza ad ASP.NET Identity

Ci auguriamo di presto forniscono indicazioni sulla migrazione di App esistenti che usano le appartenenze di ASP.NET o appartenenza semplice nel nuovo sistema di identità di ASP.NET.

## <a name="next-steps"></a>Passaggi successivi

- [Creare un'App ASP.NET MVC 5 con Facebook e Google OAuth2 e OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)  
 L'esercitazione Usa l'API di identità di ASP.NET per aggiungere informazioni di profilo al database utente e su come eseguire l'autenticazione con Google e Facebook.
- [Creare un'app ASP.NET MVC con autenticazione e database SQL e distribuirla nel servizio App di Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)  
 Questa esercitazione illustra come usare l'API di identità per aggiungere utenti e ruoli.
- [Account utente individuali](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#indauth) nella creazione di progetti Web ASP.NET in Visual Studio 2013
- [Gli account aziendali](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth) nella creazione di progetti Web ASP.NET in Visual Studio 2013
- [Personalizzazione delle informazioni di profilo in ASP.NET Identity nei modelli di Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)
- [Ottenere altre informazioni dal provider di social networking utilizzati nei modelli di progetto Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [https://github.com/rustd/AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample)  
 Applicazione di esempio che illustra come aggiungere ruoli di base e supporto utente e su come eseguire operazioni di gestione degli utenti e ruoli.
