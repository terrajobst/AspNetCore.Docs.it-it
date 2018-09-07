---
title: Client IP safelist per ASP.NET Core
author: damienbod
description: Informazioni su come scrivere i filtri Middleware o azione per convalidare gli indirizzi IP remoti rispetto a un elenco di indirizzi IP approvati.
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: 40fe7b67359efd1692490099c3fb529ba4a6148f
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040110"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="c5dc2-103">Client IP safelist per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c5dc2-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="c5dc2-104">Dal [Damien Bowden](https://twitter.com/damien_bod) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c5dc2-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="c5dc2-105">Questo articolo illustra due modi per implementare un safelist IP (noto anche come un elenco elementi consentiti):</span><span class="sxs-lookup"><span data-stu-id="c5dc2-105">This article shows two ways to implement an IP safelist (also known as a whitelist):</span></span>

* <span data-ttu-id="c5dc2-106">Tramite middleware di ASP.NET Core per controllare l'indirizzo IP remoto di ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="c5dc2-106">By using ASP.NET Core middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="c5dc2-107">Mediante i filtri azione di ASP.NET Core per controllare l'indirizzo IP remoto di richieste per i metodi di azione specifica.</span><span class="sxs-lookup"><span data-stu-id="c5dc2-107">By using ASP.NET Core action filters to check the remote IP address of requests for specific action methods.</span></span>

<span data-ttu-id="c5dc2-108">L'app di esempio illustra entrambi gli approcci.</span><span class="sxs-lookup"><span data-stu-id="c5dc2-108">The sample app illustrates both approaches.</span></span> <span data-ttu-id="c5dc2-109">In ogni caso, una stringa che contiene gli indirizzi IP client riconosciuto viene archiviata in un'impostazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="c5dc2-109">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="c5dc2-110">Il middleware o il filtro analizza la stringa in un elenco e verifica se l'indirizzo IP remoto è nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="c5dc2-110">The middleware or filter parses the string into a list and  checks if the remote IP is in the list.</span></span> <span data-ttu-id="c5dc2-111">In caso contrario, viene restituito un codice di stato HTTP 403-accesso negato.</span><span class="sxs-lookup"><span data-stu-id="c5dc2-111">If not, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="c5dc2-112">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c5dc2-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-safelist"></a><span data-ttu-id="c5dc2-113">Il safelist</span><span class="sxs-lookup"><span data-stu-id="c5dc2-113">The safelist</span></span>

<span data-ttu-id="c5dc2-114">L'elenco viene configurato nel *appSettings. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="c5dc2-114">The list is configured in the *appsettings.json* file.</span></span> <span data-ttu-id="c5dc2-115">È un elenco delimitato da punto e virgola e può contenere indirizzi IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="c5dc2-115">It's a semicolon-delimited list and can contain IPv4 and IPv6 addresses.</span></span>

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a><span data-ttu-id="c5dc2-116">Middleware</span><span class="sxs-lookup"><span data-stu-id="c5dc2-116">Middleware</span></span>

<span data-ttu-id="c5dc2-117">Il `Configure` metodo aggiunge il middleware e passa la stringa dell'elenco indirizzi attendibili in un parametro di costruttore.</span><span class="sxs-lookup"><span data-stu-id="c5dc2-117">The `Configure` method adds the middleware and passes the safelist string to it in a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=7)]

<span data-ttu-id="c5dc2-118">Il middleware analizza la stringa in una matrice e Cerca l'indirizzo IP remoto nella matrice.</span><span class="sxs-lookup"><span data-stu-id="c5dc2-118">The middleware parses the string into an array and looks for the remote IP address in the array.</span></span> <span data-ttu-id="c5dc2-119">Se non viene trovato l'indirizzo IP remoto, il middleware restituisce HTTP 401 accesso negato.</span><span class="sxs-lookup"><span data-stu-id="c5dc2-119">If the remote IP address is not found, the middleware returns HTTP 401 Forbidden.</span></span> <span data-ttu-id="c5dc2-120">Questo processo di convalida viene ignorato per le richieste HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="c5dc2-120">This validation process is bypassed for HTTP Get requests.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="c5dc2-121">Filtro azioni</span><span class="sxs-lookup"><span data-stu-id="c5dc2-121">Action filter</span></span>

<span data-ttu-id="c5dc2-122">Se si desidera un safelist solo per i metodi di azione o controller specifici, usare un filtro azione.</span><span class="sxs-lookup"><span data-stu-id="c5dc2-122">If you want a safelist only for specific controllers or action methods, use an action filter.</span></span> <span data-ttu-id="c5dc2-123">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="c5dc2-123">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

<span data-ttu-id="c5dc2-124">Il filtro azioni viene aggiunto al contenitore dei servizi.</span><span class="sxs-lookup"><span data-stu-id="c5dc2-124">The action filter is added to the services container.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="c5dc2-125">Il filtro è quindi utilizzabile in un controller o metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="c5dc2-125">The filter can then be used on a controller or action method.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

<span data-ttu-id="c5dc2-126">Nell'app di esempio, viene applicato il filtro per il `Get` (metodo).</span><span class="sxs-lookup"><span data-stu-id="c5dc2-126">In the sample app, the filter is applied to the `Get` method.</span></span> <span data-ttu-id="c5dc2-127">In questo caso quando si testa l'app mediante l'invio di un `Get` API richiesta, l'attributo è la convalida di indirizzo IP del client.</span><span class="sxs-lookup"><span data-stu-id="c5dc2-127">So when you test the app by sending a `Get` API request, the attribute is validating the client IP address.</span></span> <span data-ttu-id="c5dc2-128">Quando si testa chiamando l'API con qualsiasi altro metodo HTTP, il middleware convalida l'indirizzo IP del client.</span><span class="sxs-lookup"><span data-stu-id="c5dc2-128">When you test by calling the API with any other HTTP method, the middleware is validating the client IP.</span></span>

## <a name="razor-pages-filter"></a><span data-ttu-id="c5dc2-129">Filtrare le pagine Razor</span><span class="sxs-lookup"><span data-stu-id="c5dc2-129">Razor Pages filter</span></span> 

<span data-ttu-id="c5dc2-130">Se si desidera un safelist per un'app Razor Pages, usare un filtro di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c5dc2-130">If you want a safelist for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="c5dc2-131">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="c5dc2-131">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

<span data-ttu-id="c5dc2-132">Questo filtro è abilitato, aggiungerlo alla raccolta di filtri MVC.</span><span class="sxs-lookup"><span data-stu-id="c5dc2-132">This filter is enabled by adding it to the MVC Filters collection.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

<span data-ttu-id="c5dc2-133">Quando si esegue l'app e richiedere una pagina Razor, il filtro di Razor Pages è convalida l'indirizzo IP del client.</span><span class="sxs-lookup"><span data-stu-id="c5dc2-133">When you run the app and request a Razor page, the Razor Pages filter is validating the client IP.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c5dc2-134">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="c5dc2-134">Next steps</span></span>

<span data-ttu-id="c5dc2-135">[Altre informazioni sul Middleware di ASP.NET Core](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="c5dc2-135">[Learn more about ASP.NET Core Middleware](xref:fundamentals/middleware/index).</span></span>
