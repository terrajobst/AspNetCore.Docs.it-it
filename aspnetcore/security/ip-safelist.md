---
title: Client IP safelist per ASP.NET Core
author: damienbod
description: Informazioni su come scrivere i filtri Middleware o azione per convalidare gli indirizzi IP remoti rispetto a un elenco di indirizzi IP approvati.
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: 286f199c0d9164fa70d511aba523210c85c2fdfd
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207732"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="dac1f-103">Client IP safelist per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dac1f-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="dac1f-104">Dal [Damien Bowden](https://twitter.com/damien_bod) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="dac1f-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="dac1f-105">Questo articolo illustra tre modi per implementare un safelist IP (noto anche come un elenco elementi consentiti) in un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dac1f-105">This article shows three ways to implement an IP safelist (also known as a whitelist) in an ASP.NET Core app.</span></span> <span data-ttu-id="dac1f-106">È possibile usare:</span><span class="sxs-lookup"><span data-stu-id="dac1f-106">You can use:</span></span>

* <span data-ttu-id="dac1f-107">Middleware per controllare l'indirizzo IP remoto di ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="dac1f-107">Middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="dac1f-108">Filtri dell'azione per controllare l'indirizzo IP remoto di richieste di controller specifici o i metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="dac1f-108">Action filters to check the remote IP address of requests for specific controllers or action methods.</span></span>
* <span data-ttu-id="dac1f-109">Razor Pages i filtri per controllare l'indirizzo IP remoto di richieste di pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="dac1f-109">Razor Pages filters to check the remote IP address of requests for Razor pages.</span></span>

<span data-ttu-id="dac1f-110">L'app di esempio illustra entrambi gli approcci.</span><span class="sxs-lookup"><span data-stu-id="dac1f-110">The sample app illustrates both approaches.</span></span> <span data-ttu-id="dac1f-111">In ogni caso, una stringa che contiene gli indirizzi IP client riconosciuto viene archiviata in un'impostazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="dac1f-111">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="dac1f-112">Il middleware o il filtro analizza la stringa in un elenco e verifica se l'indirizzo IP remoto è nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="dac1f-112">The middleware or filter parses the string into a list and  checks if the remote IP is in the list.</span></span> <span data-ttu-id="dac1f-113">In caso contrario, viene restituito un codice di stato HTTP 403-accesso negato.</span><span class="sxs-lookup"><span data-stu-id="dac1f-113">If not, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="dac1f-114">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dac1f-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="the-safelist"></a><span data-ttu-id="dac1f-115">Il safelist</span><span class="sxs-lookup"><span data-stu-id="dac1f-115">The safelist</span></span>

<span data-ttu-id="dac1f-116">L'elenco viene configurato nel *appSettings. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="dac1f-116">The list is configured in the *appsettings.json* file.</span></span> <span data-ttu-id="dac1f-117">È un elenco delimitato da punto e virgola e può contenere indirizzi IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="dac1f-117">It's a semicolon-delimited list and can contain IPv4 and IPv6 addresses.</span></span>

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a><span data-ttu-id="dac1f-118">Middleware</span><span class="sxs-lookup"><span data-stu-id="dac1f-118">Middleware</span></span>

<span data-ttu-id="dac1f-119">Il `Configure` metodo aggiunge il middleware e passa la stringa dell'elenco indirizzi attendibili in un parametro di costruttore.</span><span class="sxs-lookup"><span data-stu-id="dac1f-119">The `Configure` method adds the middleware and passes the safelist string to it in a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=7)]

<span data-ttu-id="dac1f-120">Il middleware analizza la stringa in una matrice e Cerca l'indirizzo IP remoto nella matrice.</span><span class="sxs-lookup"><span data-stu-id="dac1f-120">The middleware parses the string into an array and looks for the remote IP address in the array.</span></span> <span data-ttu-id="dac1f-121">Se non viene trovato l'indirizzo IP remoto, il middleware restituisce HTTP 401 accesso negato.</span><span class="sxs-lookup"><span data-stu-id="dac1f-121">If the remote IP address is not found, the middleware returns HTTP 401 Forbidden.</span></span> <span data-ttu-id="dac1f-122">Questo processo di convalida viene ignorato per le richieste HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="dac1f-122">This validation process is bypassed for HTTP Get requests.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="dac1f-123">Filtro azioni</span><span class="sxs-lookup"><span data-stu-id="dac1f-123">Action filter</span></span>

<span data-ttu-id="dac1f-124">Se si desidera un safelist solo per i metodi di azione o controller specifici, usare un filtro azione.</span><span class="sxs-lookup"><span data-stu-id="dac1f-124">If you want a safelist only for specific controllers or action methods, use an action filter.</span></span> <span data-ttu-id="dac1f-125">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="dac1f-125">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

<span data-ttu-id="dac1f-126">Il filtro azioni viene aggiunto al contenitore dei servizi.</span><span class="sxs-lookup"><span data-stu-id="dac1f-126">The action filter is added to the services container.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="dac1f-127">Il filtro è quindi utilizzabile in un controller o metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="dac1f-127">The filter can then be used on a controller or action method.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

<span data-ttu-id="dac1f-128">Nell'app di esempio, viene applicato il filtro per il `Get` (metodo).</span><span class="sxs-lookup"><span data-stu-id="dac1f-128">In the sample app, the filter is applied to the `Get` method.</span></span> <span data-ttu-id="dac1f-129">In questo caso quando si testa l'app mediante l'invio di un `Get` API richiesta, l'attributo è la convalida di indirizzo IP del client.</span><span class="sxs-lookup"><span data-stu-id="dac1f-129">So when you test the app by sending a `Get` API request, the attribute is validating the client IP address.</span></span> <span data-ttu-id="dac1f-130">Quando si testa chiamando l'API con qualsiasi altro metodo HTTP, il middleware convalida l'indirizzo IP del client.</span><span class="sxs-lookup"><span data-stu-id="dac1f-130">When you test by calling the API with any other HTTP method, the middleware is validating the client IP.</span></span>

## <a name="razor-pages-filter"></a><span data-ttu-id="dac1f-131">Filtrare le pagine Razor</span><span class="sxs-lookup"><span data-stu-id="dac1f-131">Razor Pages filter</span></span> 

<span data-ttu-id="dac1f-132">Se si desidera un safelist per un'app Razor Pages, usare un filtro di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="dac1f-132">If you want a safelist for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="dac1f-133">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="dac1f-133">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

<span data-ttu-id="dac1f-134">Questo filtro è abilitato, aggiungerlo alla raccolta di filtri MVC.</span><span class="sxs-lookup"><span data-stu-id="dac1f-134">This filter is enabled by adding it to the MVC Filters collection.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

<span data-ttu-id="dac1f-135">Quando si esegue l'app e richiedere una pagina Razor, il filtro di Razor Pages è convalida l'indirizzo IP del client.</span><span class="sxs-lookup"><span data-stu-id="dac1f-135">When you run the app and request a Razor page, the Razor Pages filter is validating the client IP.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dac1f-136">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="dac1f-136">Next steps</span></span>

<span data-ttu-id="dac1f-137">[Altre informazioni sul Middleware di ASP.NET Core](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="dac1f-137">[Learn more about ASP.NET Core Middleware](xref:fundamentals/middleware/index).</span></span>
