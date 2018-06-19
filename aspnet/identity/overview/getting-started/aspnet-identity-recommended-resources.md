---
uid: identity/overview/getting-started/aspnet-identity-recommended-resources
title: ASP.NET Identity consigliato risorse | Documenti Microsoft
author: Rick-Anderson
description: In questo argomento vengono forniti collegamenti alle risorse di documentazione su come usare l'identità di ASP.NET. Se si conosce post di blog, thread di stackoverflow o qualsiasi altro lin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2015
ms.topic: article
ms.assetid: 0f78aec2-f509-46fa-b20f-d5208425d8ec
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-recommended-resources
msc.type: authoredcontent
ms.openlocfilehash: f2e1693a32fce6956ddb1e095e6f208b9f0faab6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876072"
---
<a name="aspnet-identity-recommended-resources"></a>ASP.NET Identity consigliato risorse
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> In questo argomento vengono forniti collegamenti alle risorse di documentazione su come usare l'identità di ASP.NET.
> 
> Se si conosce un grande blog post, [stackoverflow](http://stackoverflow.com) thread o qualsiasi altro tipo di collegamento che può essere utile, [inviare un messaggio di posta elettronica](mailto:aspnetue@microsoft.com?subject=Identity recommended resources) con il collegamento o semplicemente lasciare un messaggio nella parte inferiore della pagina.


- [Guida introduttiva ad ASP.NET Identity](#gettingstarted)
- [Nuovi articoli lettura necessario in primo piano](#feat)
- [Intermedio ASP.NET Identity](#adv)
- [Video](#video)
- [Posizione in cui porre domande, richieste di funzionalità, segnalare un bug e le compilazioni notturne](#samp)
- [Post di blog sull'identità](#blog)
- [Provider di archiviazione personalizzato per l'identità ASP.NET](#cust)
- [Risorse aggiuntive di identità](#additional)
- [Domande e &amp; un (domanda)](#qand)

<a id="gettingstarted"></a>
## <a name="getting-started-with-aspnet-identity"></a>Introduzione a ASP.NET Identity

- [App di MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) in questa esercitazione viene illustrato come scrivere un'applicazione ASP.NET MVC 5 con autorizzazione di Facebook e Google OAuth 2. Viene inoltre illustrato come aggiungere ulteriori dati al database di identità.
- [Distribuire un'app protetta ASP.NET MVC con appartenenza, OAuth e il Database SQL a un Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). In questa esercitazione aggiunge distribuzione di Azure, come proteggere le app con i ruoli, come utilizzare l'API di appartenenza per aggiungere utenti e ruoli e funzionalità di sicurezza aggiuntive.
- [Introduzione ad ASP.NET Identity](introduction-to-aspnet-identity.md)
- [Creare un'app web ASP.NET MVC 5 sicura con l'accesso, la reimpostazione della password e conferma tramite posta elettronica](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)
- [App ASP.NET MVC 5 con autenticazione a due fattori tramite SMS e posta elettronica](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)

<a id="feat"></a>
## <a name="new-featured-must-read-articles"></a>Nuovi articoli lettura necessario in primo piano

- [Procedura dettagliata: MVC ASP.NET Identity con l'autenticazione di Account Microsoft](http://www.benday.com/2014/02/25/walkthrough-asp-net-mvc-identity-with-microsoft-account-authentication/) da [Benjamin giorno](http://www.benday.com/about/)
- [Modelli di identità di estensione di identità di ASP.NET 2.0 e utilizzando chiavi Integer anziché stringhe](http://typecastexception.com/post/2014/07/13/ASPNET-Identity-20-Extending-Identity-Models-and-Using-Integer-Keys-Instead-of-Strings.aspx)
- [Autenticazione con Token AngularJS utilizzando ASP.NET Web API 2, Owin e identità](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
- [Thinktecture.IdentityManager come una sostituzione per il WSAT](http://www.hanselman.com/blog/ThinktectureIdentityManagerAsAReplacementForTheASPNETWebSiteAdministrationTool.aspx)
- [Identità di ASP.NET 2.0: Personalizzazione di utenti e ruoli](http://typecastexception.com/post/2014/06/22/ASPNET-Identity-20-Customizing-Users-and-Roles.aspx)

<a id="adv"></a>
## <a name="intermediate-aspnet-identity"></a>Intermedio ASP.NET Identity

- [La conferma dell'account e il recupero della Password con identità di ASP.NET](../features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity](../features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Migrazione di un sito Web esistente dall'appartenenza SQL ad ASP.NET Identity](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Aggiunta di ASP.NET Identity a un progetto Web Form vuoto o esistente](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md)
- MSDN Magazine [autenticazione esterno con ASP.NET Identity](https://msdn.microsoft.com/magazine/dn745860.aspx) da Dino Esposito
- MSDN Magazine[un primo sguardo a ASP.NET Identity](https://msdn.microsoft.com/magazine/dn605872.aspx) da Dino Esposito
- [Identità di ASP.NET – blocco utente](http://tech.trailmax.info/2014/06/asp-net-identity-user-lockout/)

<a id="samp"></a>
## <a name="where-to-ask-questions-request-features-report-a-bug-and-nightly-builds"></a>Posizione in cui porre domande, richieste di funzionalità, segnalare un bug e le compilazioni notturne

- Per StackOverflow, utilizzare il tag [aspnet-identity](http://stackoverflow.com/questions/tagged/asp.net-identity)
- Per i forum ASP.NET, registrare il [forum sulla sicurezza](https://forums.asp.net/25.aspx) e aggiungere **ASP.NET Identity** al titolo.
- [ASP.NET Identity su GitHub](https://github.com/aspnet/AspNetIdentity) Get build notturne, le funzionalità di richiesta, aprire i bug.

<a id="blog"></a>
## <a name="blog-posts-on-identity"></a>Post di blog sull'identità

- [Che cos'è un SecurityStamp in ASP.NET Identity?](http://stackoverflow.com/questions/19487322/what-is-asp-net-identitys-iusersecuritystampstoretuser-interface/19505060#19505060)
- Da [John Atten](https://twitter.com/xivSolutions)

    - [Modelli di identità di estensione di identità di ASP.NET 2.0 e utilizzando chiavi Integer anziché stringhe](http://typecastexception.com/post/2014/07/13/ASPNET-Identity-20-Extending-Identity-Models-and-Using-Integer-Keys-Instead-of-Strings.aspx)
    - [Identità di ASP.NET 2.0: Personalizzazione di utenti e ruoli](http://typecastexception.com/post/2014/06/22/ASPNET-Identity-20-Customizing-Users-and-Roles.aspx)
    - [ASP.NET MVC e identità 2.0: nozioni di base](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx)
    - [Impostazione di convalida dell'Account e l'autorizzazione a due fattori](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)
    - [Configurazione di connessione al database e la migrazione di Code-First per gli account di identità in ASP.NET MVC 5 e Visual Studio 2013](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx)
- Da [Taiseer Joudeh](http://bitoftech.net/taiseer-joudeh-blog/)

    - [Autenticazione basata su token utilizzando ASP.NET Web API 2, middleware Owin e ASP.NET Identity](http://bitoftech.net/2014/06/01/token-based-authentication-asp-net-web-api-2-owin-asp-net-identity/)
    - [Autenticazione con Token AngularJS utilizzando ASP.NET Web API 2, Owin e identità](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
    - [Abilitare i token di aggiornamento OAuth nell'App AngularJS usando ASP .NET Web API 2 e Owin-parte 3.](http://bitoftech.net/2014/07/16/enable-oauth-refresh-tokens-angularjs-app-using-asp-net-web-api-2-owin/)
- Da [Anders etichetta](https://twitter.com/anders_abel)

    - [Informazioni sulla Pipeline di autenticazione Owin esterno](http://coding.abel.nu/2014/06/understanding-the-owin-external-authentication-pipeline/)
    - [Cenni preliminari su Owin e identità di ASP.NET](http://coding.abel.nu/2014/06/asp-net-identity-and-owin-overview/)

  Da [K. Scott Allen](https://twitter.com/OdeToCode) su Ode al codice

    - [ASP.NET Identity Core](http://odetocode.com/blogs/scott/archive/2013/11/25/asp-net-core-identity.aspx) in questo blog vengono esaminate le astrazioni di base, tra cui IUser, IUserStore e la\*archiviare interfacce.
    - [ASP.NET Identity con Entity Framework](http://odetocode.com/blogs/scott/archive/2014/01/03/asp-net-identity-with-the-entity-framework.aspx) singoli account utente in App MVC 5, API Web e SPA, le stringhe di connessione e la gestione dei contesti
    - [Opzioni di personalizzazione con identità di ASP.NET](http://odetocode.com/blogs/scott/archive/2014/01/09/customization-options-with-asp-net-identity.aspx)
    - [Implementazione di ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- [Giorno Benjamin](http://www.benday.com/about/)[procedura dettagliata: identità di ASP.NET MVC con l'autenticazione di Account Microsoft](http://www.benday.com/2014/02/25/walkthrough-asp-net-mvc-identity-with-microsoft-account-authentication/)
- [Allen Brock](https://twitter.com/BrockLAllen)

    - [Nozioni di base sul provider di accesso esterno (account di accesso social) con il middleware di autenticazione OWIN/Katana](http://brockallen.com/2014/01/09/a-primer-on-external-login-providers-social-logins-with-owinkatana-authentication-middleware/)
    - [Introduzione a IdentityReboot](http://brockallen.com/2014/02/11/introducing-identityreboot/): un set di estensioni per ASP.NET Identity che implementano le principali funzionalità mancante ho effetti su.
- [Pranav Rastogi](https://twitter.com/rustd)

    - [Ottenere altre informazioni da Social provider](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [@beabigrockstar](https://twitter.com/beabigrockstar) (Jerrie Pelser)

    - [2 factor authentication](http://www.beabigrockstar.com/blog/2-factor-authentication-with-asp-net-identity-2-0-beta-1/)
    - [Utilizzo di Google Authenticator con ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)
    - [Guide di autenticazione ASP.NET MVC 5](http://www.beabigrockstar.com/)
- [Ottenere informazioni dai provider Social utilizzato nei modelli di progetto di Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [Creazione di una semplice applicazione ToDo con ASP.NET Identity e associare gli utenti ToDoes](https://blogs.msdn.com/b/webdev/archive/2013/10/20/building-a-simple-todo-application-with-asp-net-identity-and-associating-users-with-todoes.aspx)
- [Problemi di integrazione di OpenId di Google con ASP.NET Identity](http://blog.technovert.com/2014/01/google-openid-integration-issues-asp-net-identity/) se viene visualizzato l'errore: 404.15 errore HTTP: non trovato il modulo filtro delle richieste è configurato per negare una richiesta in cui la stringa di query è troppo lunga
- [Thinktecture.IdentityManager come una sostituzione per il WSAT](http://www.hanselman.com/blog/ThinktectureIdentityManagerAsAReplacementForTheASPNETWebSiteAdministrationTool.aspx)
- [Autenticazione con Token AngularJS utilizzando ASP.NET Web API 2, Owin e identità](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
- [Semplice Asp.net Identity Core senza Entity Framework](https://code.msdn.microsoft.com/Simple-Aspnet-Identiy-Core-7475a961)
- [Utilizzo dei ruoli in ASP.NET Identity per MVC](http://www.dotnetfunda.com/articles/show/2898/working-with-roles-in-aspnet-identity-for-mvc) da [Sheo Narayan](http://www.dotnetfunda.com/profile/sheonarayan.aspx)
- [Lo spostamento in ASP.NET Identity dall'appartenenza ASP.NET](http://webdojo.sharepoint.com/ajmatthews/_layouts/15/start.aspx#/Lists/Posts/Post.aspx?ID=2) da Alistair Matthews

<a id="video"></a>
## <a name="videos"></a>Video

- Channel 9 [protezione delle applicazioni ASP.NET e servizi: si rinnova di sicurezza per le applicazioni moderne](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B421#fbid=PhVT9E1WRtr?hashlink=fbid) da Ido Flatow
- Channel 9 [introduzione di ASP.NET Identity](https://channel9.msdn.com/Events/dotnetConf/2014/ASP-NET-Identity-Security) da Pranav Rastogi
- Channel 9 [autenticazione ASP.NET utilizzando l'identità ASP.NET](https://channel9.msdn.com/Shows/Web+Camps+TV/Special-Movember-Episode-ASPNET-Authentication-Provider) da Cory Fowler
- Channel 9 [creazione di applicazioni Web moderne: ASP.NET Identity](https://channel9.msdn.com/Series/Building-Modern-Web-Apps/03) da Jeff Koch
- Channel 9 [proteggere il sito Web con ASP.NET Identity](https://channel9.msdn.com/Events/TechDays/Techdays-2014-the-Netherlands/Securing-your-website-with-ASP-NET-Identity) da Alex Thissen
- [Utilizzare ASP.NET Identity su un modello di database esistente](https://www.youtube.com/watch?v=elfqejow5hM) da Alexander Schmidt
- [Identità ASP.NET uno](https://www.youtube.com/watch?v=w8GD-QIusKk) da Ivaylo Kenov di Telerik
- [Ceco ASP.NET Identity](https://www.youtube.com/watch?v=tVbZp5brcpY) In questa lezione viene illustrato come distribuire l'autenticazione di base, come aggiungere il supporto per i provider di identità esterno, ad esempio Facebook o Twitter e come utilizzare le password monouso (OTP). [ASP.NET Identity (Esci) nástupce appartenenza a un provider di ruoli&#367; v ASP.NET, tedy knihovna zajišt pro&#283;ní autentizace uživatel&#367;. V této p&#345;ednášce dimen ukážeme, jak nasad]

<a id="cust"></a>
## <a name="custom-storage-providers-for-aspnet-identity"></a>Provider di archiviazione personalizzato per l'identità ASP.NET

Se si desidera scrivere un provider personalizzato, leggere [panoramica dell'archiviazione provider personalizzati per l'identità ASP.NET](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md) e [implementazione ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx) e quindi esaminare l'origine di uno dei progetti di sistemi operativi elencati di seguito.

- Esercitazione: [panoramica dei provider di archiviazione personalizzato per l'identità ASP.NET](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md) da Tom FitzMacken
- Blog: [implementazione di ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Esercitazione:[impostazione degli account di identità base e li verso un database esterno](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). Da [ @xivSolutions ](https://twitter.com/xivSolutions).
- Esercitazione[: implementazione di un Provider di archiviazione di ASP.NET Identity MySQL personalizzato](../extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [Entità CodeFluent](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) da [SoftFluent](http://www.softfluent.com/)
- [Archiviazione tabelle di Azure](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) da James Randall.
- Archiviazione tabelle di Azure: [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) da [ @stuartleeks ](https://twitter.com/stuartleeks).
- [CouchDB / Cloudant da Daniel Wertheim.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Eseg elastico[h:%m identità elastico](https://github.com/bmbsqd/elastic-identity) da Bombsquad ab
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) da Sheely Lorenzo Sheely Lorenzo.
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) da Antônio Milesi Bastos.
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) da [ @tourismgeek ](https://twitter.com/tourismgeek).
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) da [ILMServices](http://www.ilmservice.com/).
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- Modelli T4 per generare EF code per un archivio utente "del database prima di tutto": [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)

<a id="additional"></a>
## <a name="additional-aspnet-identity-resources"></a>Risorse aggiuntive ASP.NET Identity

- [Introduzione ai provider di sicurezza Yahoo e OAuth di LinkedIn per OWIN](http://blog.beabigrockstar.com/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) da Jerrie Pelser per le istruzioni di Yahoo e LinkedIn.

<a id="qand"></a>
## <a name="qampa-questionanswer"></a>Domande e&amp;un (domanda)

- Bloccato d: gli utenti che hanno attivato "Memorizza password" (in modo che sia necessario scorrere 2FA su tale computer/browser) non è bloccato. Come e perché si impedisce che? Risposta [qui](http://stackoverflow.com/questions/24312247/locked-out-users-can-login-if-they-have-auth-cookie).
- **Domande e**: come è possibile memorizzare le attestazioni personalizzate, ad esempio il nome dell'utente reale, nel cookie ASP.NET Identity per evitare che le query di database non necessaria a ogni richiesta. Risposta [qui](http://stackoverflow.com/questions/23622047/identity-cookie-loses-custom-claim-information-after-a-period-of-time).
- **D: l'aggiornamento AspNetUser Password Hash**: ho 2 progetti. Uno di essi viene utilizzata l'autenticazione di ASP.NET, l'altro utilizza l'autenticazione di Windows, che è il lato di amministrazione. Si desidera il progetto di amministratore per essere in grado di gestire gli utenti di altro. È possibile modificare tutto tranne la password. [Rispondere qui](http://stackoverflow.com/questions/23880666/updating-aspnetuser-password-hash).
- **Domande e**: come è possibile reimpostare la password come amministratore ad altri utenti? Risposta [qui](http://stackoverflow.com/questions/23783249/identity-2-0-reset-password-by-admin/24211766#24211766).
- **Domande e**: è possibile modificare il nome visualizzato del campo nome utente in ASP.NET MVC IdentityUser? Risposta [qui](http://stackoverflow.com/questions/23256650/can-i-change-the-displayed-name-of-the-username-field-in-asp-net-mvc-identityuse).
- **Domande e**: come si possono gran agli utenti le autorizzazioni per aggiungere altri utenti a determinati ruoli? Risposta [qui](http://stackoverflow.com/questions/23695373/allow-users-to-grant-permissions-to-other-users-for-their-account-in-asp-net-ide).
- **Domande e**: l'archiviazione di informazioni sul profilo nella tabella AspNetUsers e la tabella AspNetUserClaims. Risposta [qui](http://stackoverflow.com/questions/23215727/is-there-any-benefit-to-storing-user-information-in-aspnetuserclaims-with-asp-ne).
- **Domande e**: memorizzazione dei dati quando si utilizza un provider di autenticazione esterno. Risposta [qui](http://stackoverflow.com/questions/23180896/how-to-remember-the-login-in-mvc5-when-an-external-provider-is-used).
- **Domande e**: perché ogni richiesta richiede un ApplicationDBContext, non è una quantità eccessiva overhead?. Risposta, No, l'overhead è bassa.
- D: come ottenere un elenco di utenti ha effettuato l'accesso? Risposta [qui](http://stackoverflow.com/questions/22995653/getting-a-list-of-logged-in-users-in-asp-net-identity/).
- D: come è possibile rilevare quando un utente accede con ASPNET? Risposta [qui](http://stackoverflow.com/questions/22956486/how-can-i-detect-when-a-user-logs-in-with-microsoft-aspnet-identity/22970698#22970698).
- D: come ottenere i messaggi di errore localizzato per l'identità? Risposta [qui](http://stackoverflow.com/questions/22835981/asp-net-identity-localization-publickeytoken/22845864#22845864).
- D: come è possibile configurare CookieMiddleware per ottenere attestazioni aggiornate ogni 30 minuti? Risposta [qui](http://stackoverflow.com/questions/22682663/how-to-hold-the-cookies-claims-updated-with-mcv5-owin/22796932#22796932).
- D: come modificare le attestazioni per l'utente dopo che hanno effettuato l'accesso? Risposta [qui](http://stackoverflow.com/questions/22768836/can-i-modify-claims-in-asp-net-identity-with-owin-after-calling-signin/22769963#22769963).
- D: come invalidare i token di sicurezza? Risposta [qui](http://stackoverflow.com/questions/22755700/revoke-token-generated-by-usertokenprovider-in-asp-net-identity-2-0/22767286#22767286).
- D: come è archivio attestazioni nel middleware del cookie? Risposta [qui](http://stackoverflow.com/questions/22320632/storing-retrieving-user-data-without-database-when-using-owin-cookie-authenticat/22541856#22541856).
- D: Vorrei dispone di un PIN o un controllo di protezione di ogni metodo di azione nell'applicazione MVC, ma desidera archiviare il successo di utenti in modo che non dispongono di immissione del PIN a ogni richiesta a tale metodo di azione. Risposta [qui](http://stackoverflow.com/questions/22479958/security-check-an-user-to-a-access-controller-action/22486075#22486075).
- D: Desidero per salvare l'indirizzo di posta elettronica restituito da un provider del social al database, come devo fare? Risposta [qui](http://stackoverflow.com/questions/22888397/save-claims-to-db-on-login/22970969#22970969):
- D: come è possibile rilevare quando un utente effettua l'accesso sia con/con-out un cookie "Memorizza password"? Risposta [qui](http://stackoverflow.com/questions/22956486/how-can-i-detect-when-a-user-logs-in-with-microsoft-aspnet-identity/22970698#22970698).
- D: è possibile modificare le attestazioni in ASP.NET Identity con OWIN dopo la chiamata SignIn? Risposta: La chiamata SignIn è esattamente si suppone che da eseguire quando si desidera modificare le attestazioni per l'utente. Fondamentalmente, comporta l'oggetto ClaimsIdentity da serializzare nel cookie, motivo per cui si vedere nuove attestazioni visualizzati nelle richieste successive.
