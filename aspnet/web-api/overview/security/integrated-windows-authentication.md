---
uid: web-api/overview/security/integrated-windows-authentication
title: L'autenticazione Windows integrata | Microsoft Docs
author: MikeWasson
description: Viene descritto l'utilizzo l'autenticazione integrata di Windows nell'API Web ASP.NET.
ms.author: aspnetcontent
ms.date: 12/18/2012
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: c15677b7aa66619f1ff32819585340ff497b937d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820406"
---
<a name="integrated-windows-authentication"></a>Autenticazione integrata di Windows
====================
da [Mike Wasson](https://github.com/MikeWasson)

L'autenticazione integrata di Windows consente agli utenti di accedere con le proprie credenziali di Windows, tramite Kerberos o NTLM. Il client invia le credenziali nell'intestazione dell'autorizzazione. L'autenticazione di Windows è ideale per un ambiente intranet. Per altre informazioni, vedere [Windows autenticazione](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).

| Vantaggi | Svantaggi |
| --- | --- |
| -Integrata in IIS. -Non invia le credenziali dell'utente nella richiesta. -Se il computer client appartiene al dominio (ad esempio, applicazione intranet), l'utente non dovrà immettere le credenziali. | -Non è consigliato per applicazioni Internet. -Richiede il supporto di Kerberos o NTLM nel client. -Client deve essere nel dominio di Active Directory. |

> [!NOTE]
> Se l'applicazione è ospitata in Azure e si dispone di un dominio di Active Directory in locale, prendere in considerazione la federazione di AD locali con Azure Active Directory. In questo modo, gli utenti possono accedere con le proprie credenziali in locale, ma l'autenticazione viene eseguita da Azure AD. Per altre informazioni, vedere [l'autenticazione di Azure](../../../visual-studio/overview/2012/windows-azure-authentication.md).


Per creare un'applicazione che usa l'autenticazione Windows integrata, selezionare il modello "Applicazione Intranet" nella creazione guidata progetto MVC 4. Questo modello di progetto inserisce la seguente impostazione nel file Web. config:

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

Sul lato client, l'autenticazione integrata Windows funziona con qualsiasi browser che supporta il [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) schema di autenticazione, che include la maggior parte dei browser principali. Per le applicazioni client .NET, il **HttpClient** classe supporta l'autenticazione di Windows:

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

L'autenticazione di Windows è vulnerabile agli attacchi di tipo richiesta intersito falsa (CSRF) cross-site. Visualizzare [prevenzione degli attacchi di richiesta intersito falsa (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).
