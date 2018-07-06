---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: "[Procedura:]  Cache una pagina ASP.NET in base alle informazioni nell'intestazione HTTP | Microsoft Docs"
author: rick-anderson
description: In questo video Chris Pels illustra come mantenere una pagina nella cache di output ASP.NET in base alle informazioni nell'intestazione HTTP della pagina. Primo, l'intestazione HTTP potenziale...
ms.author: aspnetcontent
ms.date: 02/26/2009
ms.assetid: 0f8df1bd-080a-4eeb-980c-c2fbb05d30c2
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
msc.type: video
ms.openlocfilehash: 64c5c1d82376b1a3ef7c4423c3b3a372ce5ab238
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821457"
---
<a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a><span data-ttu-id="9d9bc-104">[Procedura:]  Cache una pagina ASP.NET in base alle informazioni nell'intestazione HTTP</span><span class="sxs-lookup"><span data-stu-id="9d9bc-104">[How Do I:]  Cache an ASP.NET Page Based Upon Information in the HTTP Header</span></span>
====================
<span data-ttu-id="9d9bc-105">da [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="9d9bc-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="9d9bc-106">In questo video Chris Pels illustra come mantenere una pagina nella cache di output ASP.NET in base alle informazioni nell'intestazione HTTP della pagina.</span><span class="sxs-lookup"><span data-stu-id="9d9bc-106">In this video Chris Pels shows how to keep a page in the ASP.NET output cache based upon information in the page's HTTP header.</span></span> <span data-ttu-id="9d9bc-107">In primo luogo, vengono esaminati i possibili valori delle intestazioni HTTP.</span><span class="sxs-lookup"><span data-stu-id="9d9bc-107">First, the potential HTTP header values are reviewed.</span></span> <span data-ttu-id="9d9bc-108">Quindi, viene creata una pagina di esempio e quindi la direttiva OutputCache viene usata con il VaryByHeader (attributo) che contiene un valore "accept-language", un'intestazione HTTP, per controllare la memorizzazione nella cache in base alla lingua del browser dell'utente.</span><span class="sxs-lookup"><span data-stu-id="9d9bc-108">Then, a sample page is created and then the OutputCache directive is used with the VaryByHeader attribute which contains a value "accept-language", an HTTP header, to control caching based upon the language of the user's browser.</span></span> <span data-ttu-id="9d9bc-109">Viene visualizzata la pagina di esempio in Internet Explorer che è impostata su inglese e quindi in FireFox che è configurato per usare il francese.</span><span class="sxs-lookup"><span data-stu-id="9d9bc-109">The sample page is viewed in IE which is set to English and then in FireFox which is set to use French.</span></span> <span data-ttu-id="9d9bc-110">Infine, viene illustrata l'opzione per spostare la definizione della cache per un CacheProfile nel file Web. config.</span><span class="sxs-lookup"><span data-stu-id="9d9bc-110">Finally, the option to move the cache definition to a CacheProfile in the web.config file is discussed.</span></span>

[<span data-ttu-id="9d9bc-111">&#9654;Guarda il video (12 minuti)</span><span class="sxs-lookup"><span data-stu-id="9d9bc-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
