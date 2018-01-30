---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: '[Eseguire la ricerca per categorie:]  Cache di una pagina ASP.NET in base alle informazioni nell''intestazione HTTP | Documenti Microsoft'
author: rick-anderson
description: In questo video di Chris Pels viene illustrato come mantenere una pagina nella cache di output ASP.NET in base alle informazioni nell'intestazione HTTP della pagina. Primo, il potenziale hea HTTP...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2009
ms.topic: article
ms.assetid: 0f8df1bd-080a-4eeb-980c-c2fbb05d30c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
msc.type: video
ms.openlocfilehash: ce5ea10396d0fe31d72425e2431102a0cb0c3bd0
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
<a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a><span data-ttu-id="74792-104">[Eseguire la ricerca per categorie:]  Cache di una pagina ASP.NET in base alle informazioni nell'intestazione HTTP</span><span class="sxs-lookup"><span data-stu-id="74792-104">[How Do I:]  Cache an ASP.NET Page Based Upon Information in the HTTP Header</span></span>
====================
<span data-ttu-id="74792-105">da [Chris PEL](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="74792-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="74792-106">In questo video di Chris Pels viene illustrato come mantenere una pagina nella cache di output ASP.NET in base alle informazioni nell'intestazione HTTP della pagina.</span><span class="sxs-lookup"><span data-stu-id="74792-106">In this video Chris Pels shows how to keep a page in the ASP.NET output cache based upon information in the page's HTTP header.</span></span> <span data-ttu-id="74792-107">In primo luogo, vengono esaminati i possibili valori dell'intestazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="74792-107">First, the potential HTTP header values are reviewed.</span></span> <span data-ttu-id="74792-108">Quindi, viene creata una pagina di esempio e quindi la direttiva OutputCache viene utilizzata con l'attributo VaryByHeader che contiene un valore "accept-language", un'intestazione HTTP, per controllare la memorizzazione nella cache in base alla lingua del browser dell'utente.</span><span class="sxs-lookup"><span data-stu-id="74792-108">Then, a sample page is created and then the OutputCache directive is used with the VaryByHeader attribute which contains a value "accept-language", an HTTP header, to control caching based upon the language of the user's browser.</span></span> <span data-ttu-id="74792-109">In Internet Explorer che sono impostate su inglese e quindi in FireFox impostato in modo da utilizzare francese, viene visualizzata la pagina di esempio.</span><span class="sxs-lookup"><span data-stu-id="74792-109">The sample page is viewed in IE which is set to English and then in FireFox which is set to use French.</span></span> <span data-ttu-id="74792-110">Infine, viene illustrata l'opzione per spostare la definizione della cache per un CacheProfile nel file Web. config.</span><span class="sxs-lookup"><span data-stu-id="74792-110">Finally, the option to move the cache definition to a CacheProfile in the web.config file is discussed.</span></span>

[<span data-ttu-id="74792-111">&#9654; Guardare video (12 minuti)</span><span class="sxs-lookup"><span data-stu-id="74792-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
