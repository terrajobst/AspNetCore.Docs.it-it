---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Bundling and Minifying asset in un Web ASP.NET, le pagine del sito (Razor) | Documenti Microsoft
author: microsoft
description: "Creazione di bundle e minimizzazione disponibili modi per rendere più veloce del sito. Creazione di bundle consente di combinare più file JavaScript (js) o più fogli di stile CSS (..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: df00b5c9e741e429011143d04121df97c1930602
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="af882-104">Bundling and Minifying asset in un sito ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="af882-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="af882-105">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="af882-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="af882-106">Creazione di bundle e minimizzazione disponibili modi per rendere più veloce del sito.</span><span class="sxs-lookup"><span data-stu-id="af882-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="af882-107">Creazione di bundle consente di combinare più JavaScript (*. js*) file o più fogli di stile CSS (*CSS*) file in modo che possono essere scaricati come un'unità, anziché uno alla volta.</span><span class="sxs-lookup"><span data-stu-id="af882-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="af882-108">Minimizzazione comprime out spazi vuoti ed esegue altri tipi di compressione per rendere i file scaricati come piccolo possibile.</span><span class="sxs-lookup"><span data-stu-id="af882-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="af882-109">La versione RC di ASP.NET Web Pages 2 non supporta l'aggregazione e la riduzione perché il pacchetto che contiene gli elementi necessari non è ancora disponibile in Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="af882-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="af882-110">Ci scusiamo per l'inconveniente.</span><span class="sxs-lookup"><span data-stu-id="af882-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="af882-111">Il pacchetto deve essere disponibile nella versione finale di ASP.NET Web Pages 2 e WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="af882-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
