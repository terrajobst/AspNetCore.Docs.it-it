---
title: Client IP safelist per ASP.NET Core
author: damienbod
description: Informazioni su come scrivere i filtri Middleware o azione per convalidare gli indirizzi IP remoti rispetto a un elenco di indirizzi IP approvati.
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: ca05989efabea3a71c6912e98055a6746e0f5966
ms.sourcegitcommit: 1bf80f4acd62151ff8cce517f03f6fa891136409
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/15/2019
ms.locfileid: "68223926"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="265c4-103">Client IP safelist per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="265c4-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="265c4-104">Dal [Damien Bowden](https://twitter.com/damien_bod) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="265c4-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="265c4-105">Questo articolo illustra tre modi per implementare un safelist IP (noto anche come un elenco elementi consentiti) in un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="265c4-105">This article shows three ways to implement an IP safelist (also known as a whitelist) in an ASP.NET Core app.</span></span> <span data-ttu-id="265c4-106">È possibile usare:</span><span class="sxs-lookup"><span data-stu-id="265c4-106">You can use:</span></span>

* <span data-ttu-id="265c4-107">Middleware per controllare l'indirizzo IP remoto di ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="265c4-107">Middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="265c4-108">Filtri dell'azione per controllare l'indirizzo IP remoto di richieste di controller specifici o i metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="265c4-108">Action filters to check the remote IP address of requests for specific controllers or action methods.</span></span>
* <span data-ttu-id="265c4-109">Razor Pages i filtri per controllare l'indirizzo IP remoto di richieste di pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="265c4-109">Razor Pages filters to check the remote IP address of requests for Razor pages.</span></span>

<span data-ttu-id="265c4-110">In ogni caso, una stringa che contiene gli indirizzi IP client riconosciuto viene archiviata in un'impostazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="265c4-110">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="265c4-111">Il middleware o il filtro analizza la stringa in un elenco e verifica se l'indirizzo IP remoto è nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="265c4-111">The middleware or filter parses the string into a list and checks if the remote IP is in the list.</span></span> <span data-ttu-id="265c4-112">In caso contrario, viene restituito un codice di stato HTTP 403-accesso negato.</span><span class="sxs-lookup"><span data-stu-id="265c4-112">If not, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="265c4-113">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="265c4-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="the-safelist"></a><span data-ttu-id="265c4-114">Il safelist</span><span class="sxs-lookup"><span data-stu-id="265c4-114">The safelist</span></span>

<span data-ttu-id="265c4-115">L'elenco viene configurato nel *appSettings. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="265c4-115">The list is configured in the *appsettings.json* file.</span></span> <span data-ttu-id="265c4-116">È un elenco delimitato da punto e virgola e può contenere indirizzi IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="265c4-116">It's a semicolon-delimited list and can contain IPv4 and IPv6 addresses.</span></span>

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a><span data-ttu-id="265c4-117">Middleware</span><span class="sxs-lookup"><span data-stu-id="265c4-117">Middleware</span></span>

<span data-ttu-id="265c4-118">Il `Configure` metodo aggiunge il middleware e passa la stringa dell'elenco indirizzi attendibili in un parametro di costruttore.</span><span class="sxs-lookup"><span data-stu-id="265c4-118">The `Configure` method adds the middleware and passes the safelist string to it in a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=10)]

<span data-ttu-id="265c4-119">Il middleware analizza la stringa in una matrice e Cerca l'indirizzo IP remoto nella matrice.</span><span class="sxs-lookup"><span data-stu-id="265c4-119">The middleware parses the string into an array and looks for the remote IP address in the array.</span></span> <span data-ttu-id="265c4-120">Se non viene trovato l'indirizzo IP remoto, il middleware restituisce HTTP 401 accesso negato.</span><span class="sxs-lookup"><span data-stu-id="265c4-120">If the remote IP address is not found, the middleware returns HTTP 401 Forbidden.</span></span> <span data-ttu-id="265c4-121">Questo processo di convalida viene ignorato per le richieste HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="265c4-121">This validation process is bypassed for HTTP Get requests.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="265c4-122">Filtro azioni</span><span class="sxs-lookup"><span data-stu-id="265c4-122">Action filter</span></span>

<span data-ttu-id="265c4-123">Se si desidera un safelist solo per i metodi di azione o controller specifici, usare un filtro azione.</span><span class="sxs-lookup"><span data-stu-id="265c4-123">If you want a safelist only for specific controllers or action methods, use an action filter.</span></span> <span data-ttu-id="265c4-124">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="265c4-124">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

<span data-ttu-id="265c4-125">Il filtro azioni viene aggiunto al contenitore dei servizi.</span><span class="sxs-lookup"><span data-stu-id="265c4-125">The action filter is added to the services container.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="265c4-126">Il filtro è quindi utilizzabile in un controller o metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="265c4-126">The filter can then be used on a controller or action method.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

<span data-ttu-id="265c4-127">Nell'app di esempio, viene applicato il filtro per il `Get` (metodo).</span><span class="sxs-lookup"><span data-stu-id="265c4-127">In the sample app, the filter is applied to the `Get` method.</span></span> <span data-ttu-id="265c4-128">In questo caso quando si testa l'app mediante l'invio di un `Get` API richiesta, l'attributo è la convalida di indirizzo IP del client.</span><span class="sxs-lookup"><span data-stu-id="265c4-128">So when you test the app by sending a `Get` API request, the attribute is validating the client IP address.</span></span> <span data-ttu-id="265c4-129">Quando si testa chiamando l'API con qualsiasi altro metodo HTTP, il middleware convalida l'indirizzo IP del client.</span><span class="sxs-lookup"><span data-stu-id="265c4-129">When you test by calling the API with any other HTTP method, the middleware is validating the client IP.</span></span>

## <a name="razor-pages-filter"></a><span data-ttu-id="265c4-130">Filtrare le pagine Razor</span><span class="sxs-lookup"><span data-stu-id="265c4-130">Razor Pages filter</span></span> 

<span data-ttu-id="265c4-131">Se si desidera un safelist per un'app Razor Pages, usare un filtro di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="265c4-131">If you want a safelist for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="265c4-132">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="265c4-132">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

<span data-ttu-id="265c4-133">Questo filtro è abilitato, aggiungerlo alla raccolta di filtri MVC.</span><span class="sxs-lookup"><span data-stu-id="265c4-133">This filter is enabled by adding it to the MVC Filters collection.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

<span data-ttu-id="265c4-134">Quando si esegue l'app e richiedere una pagina Razor, il filtro di Razor Pages è convalida l'indirizzo IP del client.</span><span class="sxs-lookup"><span data-stu-id="265c4-134">When you run the app and request a Razor page, the Razor Pages filter is validating the client IP.</span></span>

## <a name="next-steps"></a><span data-ttu-id="265c4-135">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="265c4-135">Next steps</span></span>

<span data-ttu-id="265c4-136">[Altre informazioni sul Middleware di ASP.NET Core](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="265c4-136">[Learn more about ASP.NET Core Middleware](xref:fundamentals/middleware/index).</span></span>
