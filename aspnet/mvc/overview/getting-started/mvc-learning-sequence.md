---
uid: mvc/overview/getting-started/mvc-learning-sequence
title: Esercitazioni e articoli consigliati su MVC | Microsoft Docs
author: Rick-Anderson
description: Questa pagina contiene collegamenti a esercitazioni di ASP.NET MVC e la sequenza consigliata per seguono gli utenti.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 8513a57a-2d45-4d6b-881c-15a01c5cbb1c
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/mvc-learning-sequence
msc.type: authoredcontent
ms.openlocfilehash: fd972549fd4c1f46f5bdeafbb466baab7a21e9df
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363989"
---
<a name="mvc-recommended-tutorials-and-articles"></a>Esercitazioni e articoli consigliati su MVC
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

<a id="pwd"></a>
## <a name="getting-started"></a>Introduzione

- [Introduzione a ASP.NET MVC 5](introduction/getting-started.md) serie parte 11 questo è un buon punto di partenza.
- [Nozioni di base di Pluralsight ASP.NET MVC 5](https://pluralsight.com/training/Player?author=scott-allen&amp;name=aspdotnet-mvc5-fundamentals-m1-introduction&amp;mode=live&amp;clip=0&amp;course=aspdotnet-mvc5-fundamentals) (esercitazione video)
- [Introduzione ad ASP.NET MVC](https://www.microsoftvirtualacademy.com/training-courses/introduction-to-asp-net-mvc) Jon Galloway e Christopher Harrison
- [Ciclo di vita di un'applicazione ASP.NET MVC 5](lifecycle-of-an-aspnet-mvc-5-application.md) documento PDF che rappresenta graficamente i ciclo di vita di un'app ASP.NET MVC 5.

<a id="con"></a>
## <a name="working-with-data"></a>Uso dei dati

- [Introduzione a Entity Framework 6 Code First con MVC 5](getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) del premio di Tom Dykstra serie approfondisce profonde Entity Framework.

<a id="wj"></a>
## <a name="security"></a>Sicurezza

- [Creare un'app ASP.NET MVC con autenticazione e database SQL e distribuire in Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) in questa esercitazione più diffusi illustra come la creazione di una semplice app e aggiunta di ruoli e appartenenza.
- [Creare un'App ASP.NET MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) questa esercitazione illustra come compilare un'applicazione web ASP.NET MVC 5 che consente agli utenti di accedere tramite OAuth 2.0 con le credenziali di un'autenticazione esterna provider, ad esempio Facebook, Twitter, LinkedIn, Microsoft o Google.
- [Creare un'app web ASP.NET MVC 5 sicura con accesso, messaggio di posta elettronica conferma e reimpostazione della password](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) prima di tutto in una serie sull'identità, incluso il codice per [inviare di nuovo un collegamento di conferma](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md#rsend).
- [App ASP.NET MVC 5 con SMS e posta elettronica l'autenticazione a due fattori](../security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) secondo della serie di identità.
- [Procedure consigliate per la distribuzione delle password e di altri dati sensibili in ASP.NET e nel servizio app di Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)
- [Autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) `isPersistent` e il cookie di sicurezza, codice in modo da richiedere all'utente di disporre di un account di posta elettronica convalidato prima di poter accedere, come le verifiche SignInManager per requisito 2FA e altro ancora.
- [Conferma dell'account e recupero della Password con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) fornisce informazioni dettagliate sull'identità non trovata nel [creare un'app web ASP.NET MVC 5 sicura con accesso, messaggio di posta elettronica conferma e reimpostazione della password](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) , ad esempio come consentire gli utenti di reimpostare la password dimenticata.

<a id="da"></a>
## <a name="azure"></a>Azure

- [Creare un'app web ASP.NET in Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/) breve e semplice esercitazione per la distribuzione in Azure.
- [Creare un'app ASP.NET MVC con autenticazione e database SQL e distribuire in Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)

<a id="perf"></a>
## <a name="performance-and-debugging"></a>Prestazioni e il debug

- [Eseguire la profilatura e il debug dell'app ASP.NET MVC con Glimpse](../performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse.md)
