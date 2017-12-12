---
uid: web-api/overview/security/integrated-windows-authentication
title: Autenticazione integrata di Windows | Documenti Microsoft
author: MikeWasson
description: Viene descritto l'utilizzo di autenticazione integrata di Windows nell'API Web ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/18/2012
ms.topic: article
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: bf5f55d98d61cdfdd246a847f41a6f1c65f00bfc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="integrated-windows-authentication"></a>Autenticazione integrata di Windows
====================
da [Mike Wasson](https://github.com/MikeWasson)

Autenticazione integrata di Windows consente agli utenti di accedere con le credenziali di Windows, tramite Kerberos o NTLM. Il client invia le credenziali nell'intestazione di autorizzazione. L'autenticazione di Windows è più adatta per un ambiente intranet. Per ulteriori informazioni, vedere [l'autenticazione di Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).

| Vantaggi | Svantaggi |
| --- | --- |
| -Incorporate in IIS. -Non invia le credenziali dell'utente nella richiesta. -Se il computer client appartiene al dominio (ad esempio, applicazione intranet), l'utente è necessario immettere le credenziali. | -Non è consigliata per le applicazioni Internet. -Richiede il supporto Kerberos o NTLM nel client. -Client deve essere nel dominio Active Directory. |

> [!NOTE]
> Se l'applicazione è ospitata in Azure e si dispone di un dominio di Active Directory locale, prendere in considerazione la federazione di Active Directory locale con Azure Active Directory. In questo modo, gli utenti possono accedere con le proprie credenziali locali, ma l'autenticazione viene eseguita da Azure AD. Per ulteriori informazioni, vedere [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).


Per creare un'applicazione che utilizza l'autenticazione integrata di Windows, selezionare il modello "Applicazione Intranet" Creazione guidata progetto MVC 4. Questo modello di progetto inserita l'impostazione seguente nel file Web. config:

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

Sul lato client, l'autenticazione integrata di Windows funziona con qualsiasi browser che supporta il [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) schema di autenticazione, che include la maggior parte dei browser principali. Per le applicazioni client .NET di **HttpClient** classe supporta l'autenticazione di Windows:

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

L'autenticazione di Windows è vulnerabile agli attacchi request forgery (CSRF) tra siti. Vedere [impedire attacchi di tipo Cross-Site Request Forgery (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).
