---
uid: web-api/overview/security/forms-authentication
title: Autenticazione basata su form in ASP.NET Web API | Documenti Microsoft
author: MikeWasson
description: Viene descritto l'utilizzo di autenticazione basata su form in ASP.NET Web API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 9027d76bcf8854fc85f11906d3651511f350cd32
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="forms-authentication-in-aspnet-web-api"></a>Autenticazione basata su form in ASP.NET Web API
====================
da [Mike Wasson](https://github.com/MikeWasson)

Autenticazione basata su form Usa un modulo HTML per inviare le credenziali dell'utente al server. Non è uno standard Internet. Autenticazione basata su form è appropriata solo per le API che vengono chiamate da un'applicazione web, web in modo che l'utente può interagire con il formato HTML.

| Vantaggi | Svantaggi |
| --- | --- |
| -Facile da implementare: incorporato in ASP.NET. -Usa provider di appartenenze ASP.NET, che rende più semplice gestire gli account utente. | -Non un standard meccanismo di autenticazione HTTP; utilizza i cookie HTTP anziché l'intestazione di autorizzazione standard. -Richiede un browser client. -Le credenziali vengono inviate come testo non crittografato. -Vulnerabili a falsificazione della richiesta tra siti (CSRF); richiede le misure di anti-CSRF. -Difficili da utilizzare dai client non-browser. Account di accesso richiede un browser. -Credenziali utente vengono inviate nella richiesta. -Alcuni utenti disabilitano i cookie. |

In breve, l'autenticazione basata su form in ASP.NET funziona simile al seguente:

1. Il client richiede una risorsa che richiede l'autenticazione.
2. Se l'utente non è autenticato, il server restituisce HTTP 302 (trovato) e reindirizza a una pagina di accesso.
3. L'utente immette le credenziali e invia il form.
4. Il server restituisce un altro HTTP 302 che reindirizza l'URI originale. La risposta include un cookie di autenticazione.
5. Il client richiede la risorsa nuovamente. La richiesta include il cookie di autenticazione, il server concede la richiesta.

![](forms-authentication/_static/image1.png)

Per ulteriori informazioni, vedere [una panoramica dell'autenticazione basata su form.](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>Con l'API Web di autenticazione basata su form

Per creare un'applicazione che utilizza l'autenticazione basata su form, selezionare il modello "Applicazione Internet" Creazione guidata progetto MVC 4. Questo modello consente di creare controller MVC per la gestione di account. È inoltre possibile utilizzare il modello "Applicazione a pagina singola", disponibile in ASP.NET autunno 2012 Update.

Nei controller di web API, è possibile limitare l'accesso tramite il `[Authorize]` attributo, come descritto in [utilizzando l'attributo [Authorize]](authentication-and-authorization-in-aspnet-web-api.md#auth3).

Autenticazione basata su form Usa un cookie di sessione per autenticare le richieste. Browser invia automaticamente tutti i cookie pertinenti al sito web di destinazione. Questa funzionalità rende vulnerabile a intersito richiesta forgery (CSRF) attacks Vedere autenticazione basata su form [impedendo Cross-Site richiesta Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).

Autenticazione basata su form non crittografa le credenziali dell'utente. Autenticazione basata su form, pertanto, non è protetto solo se utilizzata con SSL. Vedere [Working with SSL in Web API](working-with-ssl-in-web-api.md).
