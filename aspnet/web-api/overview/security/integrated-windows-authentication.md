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
ms.locfileid: "26508160"
---
<a name="integrated-windows-authentication"></a><span data-ttu-id="001a3-103">Autenticazione integrata di Windows</span><span class="sxs-lookup"><span data-stu-id="001a3-103">Integrated Windows Authentication</span></span>
====================
<span data-ttu-id="001a3-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="001a3-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="001a3-105">Autenticazione integrata di Windows consente agli utenti di accedere con le credenziali di Windows, tramite Kerberos o NTLM.</span><span class="sxs-lookup"><span data-stu-id="001a3-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="001a3-106">Il client invia le credenziali nell'intestazione di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="001a3-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="001a3-107">L'autenticazione di Windows è più adatta per un ambiente intranet.</span><span class="sxs-lookup"><span data-stu-id="001a3-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="001a3-108">Per ulteriori informazioni, vedere [l'autenticazione di Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span><span class="sxs-lookup"><span data-stu-id="001a3-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="001a3-109">Vantaggi</span><span class="sxs-lookup"><span data-stu-id="001a3-109">Advantages</span></span> | <span data-ttu-id="001a3-110">Svantaggi</span><span class="sxs-lookup"><span data-stu-id="001a3-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="001a3-111">-Incorporate in IIS.</span><span class="sxs-lookup"><span data-stu-id="001a3-111">- Built into IIS.</span></span> <span data-ttu-id="001a3-112">-Non invia le credenziali dell'utente nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="001a3-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="001a3-113">-Se il computer client appartiene al dominio (ad esempio, applicazione intranet), l'utente è necessario immettere le credenziali.</span><span class="sxs-lookup"><span data-stu-id="001a3-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="001a3-114">-Non è consigliata per le applicazioni Internet.</span><span class="sxs-lookup"><span data-stu-id="001a3-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="001a3-115">-Richiede il supporto Kerberos o NTLM nel client.</span><span class="sxs-lookup"><span data-stu-id="001a3-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="001a3-116">-Client deve essere nel dominio Active Directory.</span><span class="sxs-lookup"><span data-stu-id="001a3-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="001a3-117">Se l'applicazione è ospitata in Azure e si dispone di un dominio di Active Directory locale, prendere in considerazione la federazione di Active Directory locale con Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="001a3-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="001a3-118">In questo modo, gli utenti possono accedere con le proprie credenziali locali, ma l'autenticazione viene eseguita da Azure AD.</span><span class="sxs-lookup"><span data-stu-id="001a3-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="001a3-119">Per ulteriori informazioni, vedere [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="001a3-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>


<span data-ttu-id="001a3-120">Per creare un'applicazione che utilizza l'autenticazione integrata di Windows, selezionare il modello "Applicazione Intranet" Creazione guidata progetto MVC 4.</span><span class="sxs-lookup"><span data-stu-id="001a3-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="001a3-121">Questo modello di progetto inserita l'impostazione seguente nel file Web. config:</span><span class="sxs-lookup"><span data-stu-id="001a3-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="001a3-122">Sul lato client, l'autenticazione integrata di Windows funziona con qualsiasi browser che supporta il [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) schema di autenticazione, che include la maggior parte dei browser principali.</span><span class="sxs-lookup"><span data-stu-id="001a3-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="001a3-123">Per le applicazioni client .NET di **HttpClient** classe supporta l'autenticazione di Windows:</span><span class="sxs-lookup"><span data-stu-id="001a3-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="001a3-124">L'autenticazione di Windows è vulnerabile agli attacchi request forgery (CSRF) tra siti.</span><span class="sxs-lookup"><span data-stu-id="001a3-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="001a3-125">Vedere [impedire attacchi di tipo Cross-Site Request Forgery (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="001a3-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>
