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
<a name="integrated-windows-authentication"></a><span data-ttu-id="a609b-103">Autenticazione integrata di Windows</span><span class="sxs-lookup"><span data-stu-id="a609b-103">Integrated Windows Authentication</span></span>
====================
<span data-ttu-id="a609b-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a609b-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="a609b-105">L'autenticazione integrata di Windows consente agli utenti di accedere con le proprie credenziali di Windows, tramite Kerberos o NTLM.</span><span class="sxs-lookup"><span data-stu-id="a609b-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="a609b-106">Il client invia le credenziali nell'intestazione dell'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="a609b-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="a609b-107">L'autenticazione di Windows è ideale per un ambiente intranet.</span><span class="sxs-lookup"><span data-stu-id="a609b-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="a609b-108">Per altre informazioni, vedere [Windows autenticazione](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span><span class="sxs-lookup"><span data-stu-id="a609b-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="a609b-109">Vantaggi</span><span class="sxs-lookup"><span data-stu-id="a609b-109">Advantages</span></span> | <span data-ttu-id="a609b-110">Svantaggi</span><span class="sxs-lookup"><span data-stu-id="a609b-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="a609b-111">-Integrata in IIS.</span><span class="sxs-lookup"><span data-stu-id="a609b-111">- Built into IIS.</span></span> <span data-ttu-id="a609b-112">-Non invia le credenziali dell'utente nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="a609b-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="a609b-113">-Se il computer client appartiene al dominio (ad esempio, applicazione intranet), l'utente non dovrà immettere le credenziali.</span><span class="sxs-lookup"><span data-stu-id="a609b-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="a609b-114">-Non è consigliato per applicazioni Internet.</span><span class="sxs-lookup"><span data-stu-id="a609b-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="a609b-115">-Richiede il supporto di Kerberos o NTLM nel client.</span><span class="sxs-lookup"><span data-stu-id="a609b-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="a609b-116">-Client deve essere nel dominio di Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a609b-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="a609b-117">Se l'applicazione è ospitata in Azure e si dispone di un dominio di Active Directory in locale, prendere in considerazione la federazione di AD locali con Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a609b-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="a609b-118">In questo modo, gli utenti possono accedere con le proprie credenziali in locale, ma l'autenticazione viene eseguita da Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a609b-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="a609b-119">Per altre informazioni, vedere [l'autenticazione di Azure](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="a609b-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>


<span data-ttu-id="a609b-120">Per creare un'applicazione che usa l'autenticazione Windows integrata, selezionare il modello "Applicazione Intranet" nella creazione guidata progetto MVC 4.</span><span class="sxs-lookup"><span data-stu-id="a609b-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="a609b-121">Questo modello di progetto inserisce la seguente impostazione nel file Web. config:</span><span class="sxs-lookup"><span data-stu-id="a609b-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="a609b-122">Sul lato client, l'autenticazione integrata Windows funziona con qualsiasi browser che supporta il [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) schema di autenticazione, che include la maggior parte dei browser principali.</span><span class="sxs-lookup"><span data-stu-id="a609b-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="a609b-123">Per le applicazioni client .NET, il **HttpClient** classe supporta l'autenticazione di Windows:</span><span class="sxs-lookup"><span data-stu-id="a609b-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="a609b-124">L'autenticazione di Windows è vulnerabile agli attacchi di tipo richiesta intersito falsa (CSRF) cross-site.</span><span class="sxs-lookup"><span data-stu-id="a609b-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="a609b-125">Visualizzare [prevenzione degli attacchi di richiesta intersito falsa (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="a609b-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>
