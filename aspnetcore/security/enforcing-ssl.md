---
title: Applicare HTTPS in una base di ASP.NET
author: rick-anderson
description: Viene illustrato come richiedere HTTPS/TLS in un Core di ASP.NET web app.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 2ebb975e1ea17698cee13ca00d3f5df4a5135e38
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="enforce-https-in-an-aspnet-core"></a><span data-ttu-id="1e1f5-103">Applicare HTTPS in una base di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1e1f5-103">Enforce HTTPS in an ASP.NET Core</span></span>

<span data-ttu-id="1e1f5-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1e1f5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1e1f5-105">Questo documento illustra come:</span><span class="sxs-lookup"><span data-stu-id="1e1f5-105">This document shows how to:</span></span>

- <span data-ttu-id="1e1f5-106">Utilizzare HTTPS per tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="1e1f5-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="1e1f5-107">Reindirizzare tutte le richieste HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1e1f5-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="1e1f5-108">Eseguire **non** utilizzare `RequireHttpsAttribute` sulle API Web che ricevono informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="1e1f5-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="1e1f5-109">`RequireHttpsAttribute` Usa i codici di stato HTTP per il reindirizzamento dei browser da HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1e1f5-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="1e1f5-110">Client dell'API non possono comprendere o rispettare i reindirizzamenti da HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1e1f5-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="1e1f5-111">Tali client possono inviare informazioni su HTTP.</span><span class="sxs-lookup"><span data-stu-id="1e1f5-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="1e1f5-112">API Web dovrebbero:</span><span class="sxs-lookup"><span data-stu-id="1e1f5-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="1e1f5-113">È in ascolto su HTTP.</span><span class="sxs-lookup"><span data-stu-id="1e1f5-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="1e1f5-114">Chiudere la connessione con il codice di stato 400 (Bad Request) e non rispondere alla richiesta.</span><span class="sxs-lookup"><span data-stu-id="1e1f5-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

## <a name="require-https"></a><span data-ttu-id="1e1f5-115">Utilizzo di HTTPS</span><span class="sxs-lookup"><span data-stu-id="1e1f5-115">Require HTTPS</span></span>

<span data-ttu-id="1e1f5-116">Il [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) viene utilizzato per richiedere HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1e1f5-116">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="1e1f5-117">`[RequireHttpsAttribute]` può decorare controller o i metodi, oppure può essere applicata a livello globale.</span><span class="sxs-lookup"><span data-stu-id="1e1f5-117">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="1e1f5-118">Per applicare l'attributo a livello globale, aggiungere il seguente codice al `ConfigureServices` in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="1e1f5-118">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="1e1f5-119">Il codice evidenziato precedente richiede l'utilizzano di tutte le richieste `HTTPS`; pertanto, le richieste HTTP vengono ignorate.</span><span class="sxs-lookup"><span data-stu-id="1e1f5-119">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="1e1f5-120">Il seguente codice evidenziato reindirizza tutte le richieste HTTP a HTTPS:</span><span class="sxs-lookup"><span data-stu-id="1e1f5-120">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="1e1f5-121">Per ulteriori informazioni, vedere [Middleware di riscrittura URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="1e1f5-121">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="1e1f5-122">Che richiedono HTTPS a livello globale (`options.Filters.Add(new RequireHttpsAttribute());`) è una procedura consigliata.</span><span class="sxs-lookup"><span data-stu-id="1e1f5-122">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="1e1f5-123">L'applicazione di `[RequireHttps]` attributo a tutte le pagine controller/Razor non è considerata sicura come che richiedono HTTPS a livello globale.</span><span class="sxs-lookup"><span data-stu-id="1e1f5-123">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="1e1f5-124">Non è possibile garantire il `[RequireHttps]` attributo viene applicato quando vengono aggiunti nuovi controller e le pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="1e1f5-124">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>
