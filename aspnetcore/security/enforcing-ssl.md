---
title: Applicazione di SSL in un'applicazione ASP.NET di base
author: rick-anderson
description: Viene illustrato come richiedere SSL in un ASP.NET Core app web
keywords: ASP.NET Core, SSL, HTTPS, RequireHttpsAttribute, IIS Express
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: 35554939bd574b2826053004ed437bee46154c2b
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="596ac-104">Applicazione di SSL in un'applicazione ASP.NET di base</span><span class="sxs-lookup"><span data-stu-id="596ac-104">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="596ac-105">Da [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="596ac-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="596ac-106">Questo documento illustra come:</span><span class="sxs-lookup"><span data-stu-id="596ac-106">This document shows how to:</span></span>

- <span data-ttu-id="596ac-107">Richiedere SSL per tutte le richieste (solo richieste HTTPS).</span><span class="sxs-lookup"><span data-stu-id="596ac-107">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="596ac-108">Reindirizzare tutte le richieste HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="596ac-108">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="596ac-109">Richiedere SSL</span><span class="sxs-lookup"><span data-stu-id="596ac-109">Require SSL</span></span>

<span data-ttu-id="596ac-110">Il [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) viene utilizzato per richiedere SSL.</span><span class="sxs-lookup"><span data-stu-id="596ac-110">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="596ac-111">È possibile decorare controller o i metodi con questo attributo o è possibile applicare a livello globale, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="596ac-111">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="596ac-112">Aggiungere il seguente codice al `ConfigureServices` in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="596ac-112">Add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="596ac-113">[!code-csharp[Principale](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]</span><span class="sxs-lookup"><span data-stu-id="596ac-113">[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]</span></span>

<span data-ttu-id="596ac-114">Il codice evidenziato sopra è necessario utilizzano tutte le richieste `HTTPS`, pertanto vengono ignorate le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="596ac-114">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="596ac-115">Il seguente codice evidenziato reindirizza tutte le richieste HTTP a HTTPS:</span><span class="sxs-lookup"><span data-stu-id="596ac-115">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="596ac-116">[!code-csharp[Principale](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]</span><span class="sxs-lookup"><span data-stu-id="596ac-116">[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]</span></span>

<span data-ttu-id="596ac-117">Vedere [Middleware di riscrittura URL](xref:fundamentals/url-rewriting) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="596ac-117">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="596ac-118">Che richiedono HTTPS a livello globale (`options.Filters.Add(new RequireHttpsAttribute());`) è una procedura consigliata.</span><span class="sxs-lookup"><span data-stu-id="596ac-118">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="596ac-119">L'applicazione di `[RequireHttps]` attributo per tutti i controller non è considerata sicura come che richiedono HTTPS a livello globale.</span><span class="sxs-lookup"><span data-stu-id="596ac-119">Applying the `[RequireHttps]` attribute to all controller is not considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="596ac-120">Non è possibile garantire nuovi controller aggiunto all'app verranno memorizzate applicare il `[RequireHttps]` attributo.</span><span class="sxs-lookup"><span data-stu-id="596ac-120">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>

## <a name="set-up-iis-express-for-sslhttps"></a><span data-ttu-id="596ac-121">Configurare IIS Express per SSL e HTTPS</span><span class="sxs-lookup"><span data-stu-id="596ac-121">Set up IIS Express for SSL/HTTPS</span></span>

<span data-ttu-id="596ac-122">Vedere [impostazione HTTPS per lo sviluppo di ASP.NET Core](xref:security/https#iisxpress).</span><span class="sxs-lookup"><span data-stu-id="596ac-122">See [Setting up HTTPS for development in ASP.NET Core](xref:security/https#iisxpress).</span></span>
