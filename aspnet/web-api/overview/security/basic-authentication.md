---
uid: web-api/overview/security/basic-authentication
title: L'autenticazione di base nell'API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Viene descritto l'utilizzo dell'autenticazione di base nell'API Web ASP.NET.
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 7afafb6b7851f0d955d1f4292318f64d2a068a45
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838424"
---
<a name="basic-authentication-in-aspnet-web-api"></a><span data-ttu-id="4682b-103">Autenticazione di base nell'API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4682b-103">Basic Authentication in ASP.NET Web API</span></span>
====================
<span data-ttu-id="4682b-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4682b-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="4682b-105">L'autenticazione di base è definito in [RFC 2617, autenticazione HTTP: Basic and Digest Authentication accesso](http://www.ietf.org/rfc/rfc2617.txt).</span><span class="sxs-lookup"><span data-stu-id="4682b-105">Basic authentication is defined in [RFC 2617, HTTP Authentication: Basic and Digest Access Authentication](http://www.ietf.org/rfc/rfc2617.txt).</span></span>

<span data-ttu-id="4682b-106">Svantaggi</span><span class="sxs-lookup"><span data-stu-id="4682b-106">Disadvantages</span></span>

- <span data-ttu-id="4682b-107">Le credenziali dell'utente vengono inviate nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="4682b-107">User credentials are sent in the request.</span></span>
- <span data-ttu-id="4682b-108">Le credenziali vengono inviate come testo non crittografato.</span><span class="sxs-lookup"><span data-stu-id="4682b-108">Credentials are sent as plaintext.</span></span>
- <span data-ttu-id="4682b-109">Le credenziali vengono inviate con ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="4682b-109">Credentials are sent with every request.</span></span>
- <span data-ttu-id="4682b-110">Nessun modo per disconnettersi, a meno che non termina la sessione del browser.</span><span class="sxs-lookup"><span data-stu-id="4682b-110">No way to log out, except by ending the browser session.</span></span>
- <span data-ttu-id="4682b-111">Vulnerabile a XSS richiesta intersito falsa (CSRF); richiede misure Intersito.</span><span class="sxs-lookup"><span data-stu-id="4682b-111">Vulnerable to cross-site request forgery (CSRF); requires anti-CSRF measures.</span></span>

<span data-ttu-id="4682b-112">Vantaggi</span><span class="sxs-lookup"><span data-stu-id="4682b-112">Advantages</span></span>

- <span data-ttu-id="4682b-113">Standard di Internet.</span><span class="sxs-lookup"><span data-stu-id="4682b-113">Internet standard.</span></span>
- <span data-ttu-id="4682b-114">Supportato da tutti i principali browser.</span><span class="sxs-lookup"><span data-stu-id="4682b-114">Supported by all major browsers.</span></span>
- <span data-ttu-id="4682b-115">Protocollo relativamente semplice.</span><span class="sxs-lookup"><span data-stu-id="4682b-115">Relatively simple protocol.</span></span>

<span data-ttu-id="4682b-116">L'autenticazione di base funziona nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="4682b-116">Basic authentication works as follows:</span></span>

1. <span data-ttu-id="4682b-117">Se una richiesta richiede l'autenticazione, il server restituisce 401 (non autorizzato).</span><span class="sxs-lookup"><span data-stu-id="4682b-117">If a request requires authentication, the server returns 401 (Unauthorized).</span></span> <span data-ttu-id="4682b-118">La risposta include un'intestazione WWW-Authenticate, che indica che il server supporta l'autenticazione di base.</span><span class="sxs-lookup"><span data-stu-id="4682b-118">The response includes a WWW-Authenticate header, indicating the server supports Basic authentication.</span></span>
2. <span data-ttu-id="4682b-119">Il client invia un'altra richiesta, con le credenziali client nell'intestazione dell'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="4682b-119">The client sends another request, with the client credentials in the Authorization header.</span></span> <span data-ttu-id="4682b-120">Le credenziali vengono formattate come stringa "name: password", con codifica base64.</span><span class="sxs-lookup"><span data-stu-id="4682b-120">The credentials are formatted as the string "name:password", base64-encoded.</span></span> <span data-ttu-id="4682b-121">Le credenziali non vengono crittografate.</span><span class="sxs-lookup"><span data-stu-id="4682b-121">The credentials are not encrypted.</span></span>

<span data-ttu-id="4682b-122">L'autenticazione di base viene eseguita nel contesto di un' "area di autenticazione."</span><span class="sxs-lookup"><span data-stu-id="4682b-122">Basic authentication is performed within the context of a "realm."</span></span> <span data-ttu-id="4682b-123">Il server include il nome dell'area di autenticazione nell'intestazione WWW-Authenticate.</span><span class="sxs-lookup"><span data-stu-id="4682b-123">The server includes the name of the realm in the WWW-Authenticate header.</span></span> <span data-ttu-id="4682b-124">Le credenziali dell'utente sono valide all'interno di tale area di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="4682b-124">The user's credentials are valid within that realm.</span></span> <span data-ttu-id="4682b-125">L'ambito esatto di un'area di autenticazione è definito dal server.</span><span class="sxs-lookup"><span data-stu-id="4682b-125">The exact scope of a realm is defined by the server.</span></span> <span data-ttu-id="4682b-126">Ad esempio, è possibile definire diverse aree di autenticazione nell'ordine alle risorse di partizione.</span><span class="sxs-lookup"><span data-stu-id="4682b-126">For example, you might define several realms in order to partition resources.</span></span>

![](basic-authentication/_static/image1.png)

<span data-ttu-id="4682b-127">Perché le credenziali vengono inviate non crittografate, l'autenticazione di base è protetta solo tramite HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4682b-127">Because the credentials are sent unencrypted, Basic authentication is only secure over HTTPS.</span></span> <span data-ttu-id="4682b-128">Visualizzare [Working with SSL in Web API](working-with-ssl-in-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="4682b-128">See [Working with SSL in Web API](working-with-ssl-in-web-api.md).</span></span>

<span data-ttu-id="4682b-129">Inoltre, l'autenticazione di base è vulnerabile agli attacchi CSRF.</span><span class="sxs-lookup"><span data-stu-id="4682b-129">Basic authentication is also vulnerable to CSRF attacks.</span></span> <span data-ttu-id="4682b-130">Dopo che l'utente immette le credenziali, il browser invia automaticamente tali nelle richieste successive allo stesso dominio, per la durata della sessione.</span><span class="sxs-lookup"><span data-stu-id="4682b-130">After the user enters credentials, the browser automatically sends them on subsequent requests to the same domain, for the duration of the session.</span></span> <span data-ttu-id="4682b-131">Ciò include le richieste AJAX.</span><span class="sxs-lookup"><span data-stu-id="4682b-131">This includes AJAX requests.</span></span> <span data-ttu-id="4682b-132">Visualizzare [prevenzione degli attacchi di richiesta intersito falsa (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="4682b-132">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

## <a name="basic-authentication-with-iis"></a><span data-ttu-id="4682b-133">Autenticazione di base con IIS</span><span class="sxs-lookup"><span data-stu-id="4682b-133">Basic Authentication with IIS</span></span>

<span data-ttu-id="4682b-134">IIS supporta l'autenticazione di base, ma è presente una sola avvertenza: l'utente viene autenticato presso le proprie credenziali di Windows.</span><span class="sxs-lookup"><span data-stu-id="4682b-134">IIS supports Basic authentication, but there is a caveat: The user is authenticated against their Windows credentials.</span></span> <span data-ttu-id="4682b-135">Ciò significa che l'utente deve avere un account nel dominio del server.</span><span class="sxs-lookup"><span data-stu-id="4682b-135">That means the user must have an account on the server's domain.</span></span> <span data-ttu-id="4682b-136">Per un sito web rivolte al pubblico, in genere consigliabile eseguire l'autenticazione in un provider di appartenenze ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4682b-136">For a public-facing web site, you typically want to authenticate against an ASP.NET membership provider.</span></span>

<span data-ttu-id="4682b-137">Per abilitare l'autenticazione di base tramite IIS, impostare la modalità di autenticazione su "Windows" in Web. config del progetto ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="4682b-137">To enable Basic authentication using IIS, set the authentication mode to "Windows" in the Web.config of your ASP.NET project:</span></span>

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

<span data-ttu-id="4682b-138">In questa modalità, IIS Usa le credenziali di Windows per eseguire l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="4682b-138">In this mode, IIS uses Windows credentials to authenticate.</span></span> <span data-ttu-id="4682b-139">Inoltre, è necessario abilitare l'autenticazione di base in IIS.</span><span class="sxs-lookup"><span data-stu-id="4682b-139">In addition, you must enable Basic authentication in IIS.</span></span> <span data-ttu-id="4682b-140">In Gestione IIS, Vai a visualizzazione funzionalità, selezionare l'autenticazione e abilitare l'autenticazione di base.</span><span class="sxs-lookup"><span data-stu-id="4682b-140">In IIS Manager, go to Features View, select Authentication, and enable Basic authentication.</span></span>

![](basic-authentication/_static/image2.png)

<span data-ttu-id="4682b-141">Nel progetto API Web, aggiungere il `[Authorize]` attributo per eventuali azioni del controller che richiedono l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="4682b-141">In your Web API project, add the `[Authorize]` attribute for any controller actions that need authentication.</span></span>

<span data-ttu-id="4682b-142">Un client si autentica impostando l'intestazione di autorizzazione nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="4682b-142">A client authenticates itself by setting the Authorization header in the request.</span></span> <span data-ttu-id="4682b-143">I client del browser di eseguire automaticamente questo passaggio.</span><span class="sxs-lookup"><span data-stu-id="4682b-143">Browser clients perform this step automatically.</span></span> <span data-ttu-id="4682b-144">I client non-browser saranno necessario impostare l'intestazione.</span><span class="sxs-lookup"><span data-stu-id="4682b-144">Nonbrowser clients will need to set the header.</span></span>

## <a name="basic-authentication-with-custom-membership"></a><span data-ttu-id="4682b-145">Autenticazione di base con appartenenze personalizzato</span><span class="sxs-lookup"><span data-stu-id="4682b-145">Basic Authentication with Custom Membership</span></span>

<span data-ttu-id="4682b-146">Come accennato, l'autenticazione di base compilato in IIS utilizza le credenziali di Windows.</span><span class="sxs-lookup"><span data-stu-id="4682b-146">As mentioned, the Basic Authentication built into IIS uses Windows credentials.</span></span> <span data-ttu-id="4682b-147">Pertanto, che è necessario creare account per gli utenti nel server di hosting.</span><span class="sxs-lookup"><span data-stu-id="4682b-147">That means you need to create accounts for your users on the hosting server.</span></span> <span data-ttu-id="4682b-148">Ma per un'applicazione internet, gli account utente vengono in genere archiviati in un database esterno.</span><span class="sxs-lookup"><span data-stu-id="4682b-148">But for an internet application, user accounts are typically stored in an external database.</span></span>

<span data-ttu-id="4682b-149">Nell'esempio di codice come un modulo HTTP che esegue l'autenticazione di base.</span><span class="sxs-lookup"><span data-stu-id="4682b-149">The following code how an HTTP module that performs Basic Authentication.</span></span> <span data-ttu-id="4682b-150">È possibile collegare facilmente in un provider di appartenenze ASP.NET, sostituendo il `CheckPassword` metodo, che è un metodo in questo esempio fittizio.</span><span class="sxs-lookup"><span data-stu-id="4682b-150">You can easily plug in an ASP.NET membership provider by replacing the `CheckPassword` method, which is a dummy method in this example.</span></span>

<span data-ttu-id="4682b-151">Nell'API Web 2, è necessario considerare la scrittura un' [filtro di autenticazione](authentication-filters.md) oppure [middleware OWIN](../../../aspnet/overview/owin-and-katana/index.md), invece di un modulo HTTP.</span><span class="sxs-lookup"><span data-stu-id="4682b-151">In Web API 2, you should consider writing an [authentication filter](authentication-filters.md) or [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), instead of an HTTP module.</span></span>

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

<span data-ttu-id="4682b-152">Per abilitare il modulo HTTP, aggiungere quanto segue al file Web. config nel **System. webServer** sezione:</span><span class="sxs-lookup"><span data-stu-id="4682b-152">To enable the HTTP module, add the following to your web.config file in the **system.webServer** section:</span></span>

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

<span data-ttu-id="4682b-153">Sostituire "NomeAssembly" con il nome dell'assembly (senza includere l'estensione "dll").</span><span class="sxs-lookup"><span data-stu-id="4682b-153">Replace "YourAssemblyName" with the name of the assembly (not including the "dll" extension).</span></span>

<span data-ttu-id="4682b-154">È consigliabile disabilitare gli altri schemi di autenticazione, ad esempio autenticazione form o Windows.</span><span class="sxs-lookup"><span data-stu-id="4682b-154">You should disable other authentication schemes, such as Forms or Windows auth.</span></span>
