---
uid: mvc/overview/getting-started/mvc-learning-sequence
title: "MVC è consigliabile esercitazioni e articoli | Documenti Microsoft"
author: Rick-Anderson
description: Questa pagina contiene collegamenti a esercitazioni di ASP.NET MVC e la sequenza consigliata per utilizzarle.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 8513a57a-2d45-4d6b-881c-15a01c5cbb1c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/mvc-learning-sequence
msc.type: authoredcontent
ms.openlocfilehash: b6cc785a5ddaf156f49b15897577e733fb67736a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="mvc-recommended-tutorials-and-articles"></a>Esercitazioni e gli articoli consigliati MVC
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

<a id="pwd"></a>
## <a name="getting-started"></a>Introduzione

- [Introduzione a ASP.NET MVC 5](introduction/getting-started.md) serie 11 questo è un buon punto di partenza.
- [Nozioni di base Pluralsight ASP.NET MVC 5](https://pluralsight.com/training/Player?author=scott-allen&amp;name=aspdotnet-mvc5-fundamentals-m1-introduction&amp;mode=live&amp;clip=0&amp;course=aspdotnet-mvc5-fundamentals) (corso video)
- [Introduzione a ASP.NET MVC](https://www.microsoftvirtualacademy.com/training-courses/introduction-to-asp-net-mvc) Jon Galloway e Christopher Harrison
- [Ciclo di vita di un'applicazione ASP.NET MVC 5](lifecycle-of-an-aspnet-mvc-5-application.md) documento PDF che illustra il ciclo di vita di un'applicazione ASP.NET MVC 5.

<a id="con"></a>
## <a name="working-with-data"></a>Uso dei dati

- [Introduzione a Entity Framework 6 Code First con MVC 5](getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) qualificata di Tom Dykstra serie approfondisce deep Entity Framework.

<a id="wj"></a>
## <a name="security"></a>Sicurezza

- [Creare un'applicazione MVC ASP.NET con l'autenticazione e i database di SQL Server e distribuire in Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) questa esercitazione comune viene illustrata la creazione di una semplice app e aggiunta di ruoli e appartenenza.
- [Creare un'App di ASP.NET MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) in questa esercitazione viene illustrato come compilare un'applicazione web ASP.NET MVC 5 che consente agli utenti di effettuare l'accesso mediante OAuth 2.0 con le credenziali di un'autenticazione esterna provider, ad esempio Facebook, Twitter, LinkedIn, Microsoft o Google.
- [Creare un'app web di ASP.NET MVC 5 sicura con l'accesso, la reimpostazione di posta elettronica di conferma e la password](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) prima in una serie di identità, incluso il codice per [inviare nuovamente un collegamento di conferma](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md#rsend).
- [App di ASP.NET MVC 5 con SMS e posta elettronica di autenticazione a due fattori](../security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) seconda serie di identità.
- [Procedure consigliate per la distribuzione delle password e altri dati sensibili per ASP.NET e servizio App di Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)
- [Autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) `isPersistent` e il cookie di sicurezza, codice di un utente deve disporre di un account di posta elettronica convalidato prima di poter accedere, come le verifiche SignInManager per requisito 2FA e altro ancora.
- [Account conferma e il recupero della Password con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) fornisce informazioni dettagliate sull'identità non trovata [creare un'app web di ASP.NET MVC 5 sicura con l'accesso, la reimpostazione di posta elettronica di conferma e la password](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) , ad esempio illustrato come consentire gli utenti di reimpostare la password dimenticata.

<a id="da"></a>
## <a name="azure"></a>Azure

- [Creare un'app web ASP.NET in Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/) esercitazione breve e semplice per la distribuzione in Azure.
- [Creare un'applicazione MVC ASP.NET con l'autenticazione e i database di SQL Server e distribuire in Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)

<a id="perf"></a>
## <a name="performance-and-debugging"></a>Prestazioni e debug

- [Profilo ed eseguire il debug dell'app ASP.NET MVC con sguardo rapido](../performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse.md)
