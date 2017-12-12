---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Eseguire la ricerca per categorie:] La memorizzazione nella cache di una pagina ASP.NET in base alle informazioni personalizzate controllo | Documenti Microsoft'
author: rick-anderson
description: In questo video di Chris Pels viene illustrato come controllare i criteri per la memorizzazione nella cache di una pagina ASP.NET in base alle informazioni personalizzate. Creazione di una pagina di esempio e quindi a....
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/19/2009
ms.topic: article
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: b785de4e1161ae82ee458148aee4b30820801147
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="b8e78-104">[Eseguire la ricerca per categorie:] Controllo la memorizzazione nella cache di una pagina ASP.NET in base alle informazioni personalizzate</span><span class="sxs-lookup"><span data-stu-id="b8e78-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>
====================
<span data-ttu-id="b8e78-105">da [Chris PEL](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="b8e78-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="b8e78-106">In questo video di Chris Pels viene illustrato come controllare i criteri per la memorizzazione nella cache di una pagina ASP.NET in base alle informazioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="b8e78-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="b8e78-107">Una pagina di esempio viene creata e quindi la direttiva OutputCache viene utilizzata con l'attributo VaryByCustom che contiene un valore personalizzato.</span><span class="sxs-lookup"><span data-stu-id="b8e78-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="b8e78-108">Successivamente, il metodo GetVaryCustomByString() viene sottoposto a override nel modulo Global. asax che fornisce la gestione dell'attributo personalizzato.</span><span class="sxs-lookup"><span data-stu-id="b8e78-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="b8e78-109">In tale metodo viene restituita una stringa che identifica in modo univoco la versione memorizzata nella cache della pagina.</span><span class="sxs-lookup"><span data-stu-id="b8e78-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="b8e78-110">Infine, è disponibile una discussione su come la memorizzazione nella cache con un valore personalizzato utilizzabile in diversi modi per un sito web.</span><span class="sxs-lookup"><span data-stu-id="b8e78-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="b8e78-111">&#9654; Guardare video (12 minuti)</span><span class="sxs-lookup"><span data-stu-id="b8e78-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
