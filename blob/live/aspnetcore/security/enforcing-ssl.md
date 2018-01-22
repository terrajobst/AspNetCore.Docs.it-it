---
title: Applicazione di SSL in un'applicazione ASP.NET di base
author: rick-anderson
description: Viene illustrato come richiedere SSL in un ASP.NET Core app web
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: 42d8767fda2d3f3545876f8ca18e0f6fbe6741b8
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="51eb9-103">Applicazione di SSL in un'applicazione ASP.NET di base</span><span class="sxs-lookup"><span data-stu-id="51eb9-103">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="51eb9-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="51eb9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="51eb9-105">Questo documento illustra come:</span><span class="sxs-lookup"><span data-stu-id="51eb9-105">This document shows how to:</span></span>

- <span data-ttu-id="51eb9-106">Richiedere SSL per tutte le richieste (solo richieste HTTPS).</span><span class="sxs-lookup"><span data-stu-id="51eb9-106">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="51eb9-107">Reindirizzare tutte le richieste HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="51eb9-107">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="51eb9-108">Richiedere SSL</span><span class="sxs-lookup"><span data-stu-id="51eb9-108">Require SSL</span></span>

<span data-ttu-id="51eb9-109">Il [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) viene utilizzato per richiedere SSL.</span><span class="sxs-lookup"><span data-stu-id="51eb9-109">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="51eb9-110">È possibile decorare controller o i metodi con questo attributo o è possibile applicare a livello globale, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="51eb9-110">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="51eb9-111">Aggiungere il seguente codice al `ConfigureServices` in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="51eb9-111">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

<span data-ttu-id="51eb9-112">Il codice evidenziato sopra è necessario utilizzano tutte le richieste `HTTPS`, pertanto vengono ignorate le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="51eb9-112">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="51eb9-113">Il seguente codice evidenziato reindirizza tutte le richieste HTTP a HTTPS:</span><span class="sxs-lookup"><span data-stu-id="51eb9-113">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

<span data-ttu-id="51eb9-114">Vedere [Middleware di riscrittura URL](xref:fundamentals/url-rewriting) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="51eb9-114">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="51eb9-115">Che richiedono HTTPS a livello globale (`options.Filters.Add(new RequireHttpsAttribute());`) è una procedura consigliata.</span><span class="sxs-lookup"><span data-stu-id="51eb9-115">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="51eb9-116">L'applicazione di `[RequireHttps]` attributo per tutti i controller non è considerata sicura come che richiedono HTTPS a livello globale.</span><span class="sxs-lookup"><span data-stu-id="51eb9-116">Applying the `[RequireHttps]` attribute to all controller is not considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="51eb9-117">Non è possibile garantire nuovi controller aggiunto all'app verranno memorizzate applicare il `[RequireHttps]` attributo.</span><span class="sxs-lookup"><span data-stu-id="51eb9-117">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
