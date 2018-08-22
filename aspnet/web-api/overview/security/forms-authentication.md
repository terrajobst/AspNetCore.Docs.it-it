---
uid: web-api/overview/security/forms-authentication
title: Autenticazione basata su form nell'API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Viene descritto l'utilizzo di autenticazione basata su form in ASP.NET Web API.
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 35d62a83382553085ed8a728dcdcdae0e93090b8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826988"
---
<a name="forms-authentication-in-aspnet-web-api"></a>Autenticazione basata su form nell'API Web ASP.NET
====================
da [Mike Wasson](https://github.com/MikeWasson)

Autenticazione basata su form Usa un modulo HTML per inviare le credenziali dell'utente al server. Non è uno standard Internet. Autenticazione basata su form è appropriata solo per API web che vengono chiamate da un'applicazione web, in modo che l'utente può interagire con il form HTML.

| Vantaggi | Svantaggi |
| --- | --- |
| -Facile da implementare: incorporato in ASP.NET. -Usa provider di appartenenze ASP.NET, che rende più semplice gestire gli account utente. | -Non un standard meccanismo di autenticazione HTTP; Usa i cookie HTTP anziché l'intestazione dell'autorizzazione standard. -Richiede un client del browser. -Le credenziali vengono inviate come testo non crittografato. -Vulnerabili a richiesta intersito falsa (CSRF); richiede misure Intersito. -Difficili da usare da client non-browser. Account di accesso è necessario un browser. -Credenziali utente vengono inviate nella richiesta. -Alcuni utenti disabilitano il cookie. |

In breve, autenticazione basata su form in ASP.NET è simile al seguente:

1. Il client richiede una risorsa che richiede l'autenticazione.
2. Se l'utente non è autenticato, il server restituisce HTTP 302 (trovato) e reindirizza a una pagina di accesso.
3. L'utente immette le credenziali e invia il form.
4. Il server restituisce un altro HTTP 302 che reindirizza all'URI originale. La risposta include un cookie di autenticazione.
5. Il client richiede la risorsa nuovamente. La richiesta include il cookie di autenticazione, in modo che il server concede la richiesta.

![](forms-authentication/_static/image1.png)

Per altre informazioni, vedere [una panoramica dell'autenticazione basata su form.](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>Usando l'autenticazione basata su form con l'API Web

Per creare un'applicazione che usa l'autenticazione basata su form, selezionare il modello "Applicazione Internet" Creazione guidata progetto MVC 4. Questo modello crea i controller MVC per la gestione degli account. È anche possibile usare il modello "Applicazione a pagina singola", disponibile in ASP.NET autunno 2012 Update.

Nei controller API web, è possibile limitare l'accesso usando il `[Authorize]` dell'attributo, come descritto in [usando l'attributo [Authorize]](authentication-and-authorization-in-aspnet-web-api.md#auth3).

Autenticazione basata su form Usa un cookie di sessione per autenticare le richieste. I browser inviano automaticamente tutti i cookie pertinenti al sito web di destinazione. Questa funzionalità rende vulnerabile a XSS richiesta intersito falsa (CSRF) attacks Vedere autenticazione basata su form [impedendo Cross-Site richiesta intersito falsa (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).

Autenticazione basata su form non crittografa le credenziali dell'utente. Pertanto, autenticazione basata su form non è sicura solo se usato con SSL. Visualizzare [Working with SSL in Web API](working-with-ssl-in-web-api.md).
