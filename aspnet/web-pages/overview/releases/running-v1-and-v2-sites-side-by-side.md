---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: Che eseguono versioni diverse delle pagine Web ASP.NET (Razor) Side-by-Side | Microsoft Docs
author: tfitzmac
description: Questo articolo illustra come eseguire siti Web di ASP.NET Web Pages (Razor) nello stesso computer o server quando i siti Web configurati per l'uso di diverse versioni...
ms.author: aspnetcontent
ms.date: 02/10/2014
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: 86b1d5babac747eb4fa7ba8abde0e6155c8b17fb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810056"
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a><span data-ttu-id="5e70e-103">Esecuzione side-by-Side di versioni diverse di ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="5e70e-103">Running Different Versions of ASP.NET Web Pages (Razor) Side by Side</span></span>
====================
<span data-ttu-id="5e70e-104">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5e70e-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5e70e-105">Questo articolo illustra come eseguire siti Web di ASP.NET Web Pages (Razor) nello stesso computer o server quando i siti Web configurati per usare versioni diverse di ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="5e70e-105">This article explains how to run ASP.NET Web Pages (Razor) websites on the same computer or server when the websites are configured to use different versions of ASP.NET Web Pages.</span></span>
> 
> <span data-ttu-id="5e70e-106">Che cosa si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="5e70e-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="5e70e-107">Il comportamento predefinito novità di ASP.NET quando si dispongono di siti creati con ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="5e70e-107">What the default behavior is in ASP.NET when you have sites built with ASP.NET Web Pages.</span></span>
> - <span data-ttu-id="5e70e-108">Come configurare un nuovo sito per l'esecuzione con una versione precedente di ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="5e70e-108">How to configure a new site to run with an older version of ASP.NET Web Pages.</span></span>
>   
> 
> <span data-ttu-id="5e70e-109">Si tratta della funzionalità ASP.NET introdotta nell'articolo:</span><span class="sxs-lookup"><span data-stu-id="5e70e-109">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="5e70e-110">Il `webPages:Version` impostazione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="5e70e-110">The `webPages:Version` configuration setting.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="5e70e-111">Versioni del software</span><span class="sxs-lookup"><span data-stu-id="5e70e-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="5e70e-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="5e70e-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="5e70e-113">Questa esercitazione si integra inoltre con ASP.NET Web Pages 2 e le pagine Web ASP.NET 1.0.</span><span class="sxs-lookup"><span data-stu-id="5e70e-113">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="5e70e-114">ASP.NET Web Pages supporta la possibilità di eseguire siti Web di fianco a fianco.</span><span class="sxs-lookup"><span data-stu-id="5e70e-114">ASP.NET Web Pages supports the ability to run websites side by side.</span></span> <span data-ttu-id="5e70e-115">È possibile continuare a eseguire le applicazioni ASP.NET Web Pages precedenti, creare nuove applicazioni ASP.NET Web Pages ed eseguire tutti gli elementi nello stesso computer.</span><span class="sxs-lookup"><span data-stu-id="5e70e-115">This lets you continue to run your older ASP.NET Web Pages applications, build new ASP.NET Web Pages applications, and run all of them on the same computer.</span></span>

<span data-ttu-id="5e70e-116">Ecco alcuni aspetti da ricordare quando si installa le pagine Web con WebMatrix:</span><span class="sxs-lookup"><span data-stu-id="5e70e-116">Here are some things to remember when you install the Web Pages with WebMatrix:</span></span>

- <span data-ttu-id="5e70e-117">Per impostazione predefinita, le applicazioni Web Pages esistente verranno eseguita come la versione più recente nel computer.</span><span class="sxs-lookup"><span data-stu-id="5e70e-117">By default, existing Web Pages applications will run as the latest version on your computer.</span></span> <span data-ttu-id="5e70e-118">(Gli assembly vengono installati nella global assembly cache (GAC) e vengono usati automaticamente).</span><span class="sxs-lookup"><span data-stu-id="5e70e-118">(The assemblies are installed in the global assembly cache (GAC) and are used automatically.)</span></span>
- <span data-ttu-id="5e70e-119">Se si desidera eseguire un sito usando una versione diversa di ASP.NET Web Pages, è possibile configurare il sito a tale scopo.</span><span class="sxs-lookup"><span data-stu-id="5e70e-119">If you want to run a site using a different version of ASP.NET Web Pages, you can configure the site to do that.</span></span> <span data-ttu-id="5e70e-120">Se il sito non ha ancora un *Web. config* file nella radice del sito, crearne uno nuovo e copiarvi il codice XML seguente, sovrascrivendo il contenuto esistente.</span><span class="sxs-lookup"><span data-stu-id="5e70e-120">If your site doesn't already have a *web.config* file in the root of the site, create a new one and copy the following XML into it, overwriting the existing content.</span></span> <span data-ttu-id="5e70e-121">Se il sito contiene già un *Web. config* Aggiungi un' `<appSettings>` elemento simile a quello riportato di seguito per il `<configuration>` sezione.</span><span class="sxs-lookup"><span data-stu-id="5e70e-121">If the site already contains a *web.config* file, add an `<appSettings>` element like the following one to the `<configuration>` section.</span></span>

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  <span data-ttu-id="5e70e-122">'-Se non si specifica una versione nel *Web. config* file, un sito viene distribuito come la versione più recente.</span><span class="sxs-lookup"><span data-stu-id="5e70e-122">\`- If you do not specify a version in the *web.config* file, a site is deployed as the latest version.</span></span> <span data-ttu-id="5e70e-123">(Gli assembly vengono copiati i *bin* cartella nel sito distribuito.)</span><span class="sxs-lookup"><span data-stu-id="5e70e-123">(The assemblies are copied to the *bin* folder in the deployed site.)</span></span>
- <span data-ttu-id="5e70e-124">Le nuove applicazioni create con i modelli di sito in WebMatrix includono gli assembly della versione di pagine Web del sito *bin* cartella.</span><span class="sxs-lookup"><span data-stu-id="5e70e-124">New applications that you create using the site templates in Web Matrix include the Web Pages version assemblies in the site's *bin* folder.</span></span>

<span data-ttu-id="5e70e-125">In generale, è sempre possibile controllare quale versione di pagine Web da utilizzare con il sito con NuGet per installare gli assembly adatti al sito *bin* cartella.</span><span class="sxs-lookup"><span data-stu-id="5e70e-125">In general, you can always control which version of Web Pages to use with your site by using NuGet to install the appropriate assemblies into the site's *bin* folder.</span></span> <span data-ttu-id="5e70e-126">Per trovare i pacchetti, visitare [NuGet.org](http://NuGet.org).</span><span class="sxs-lookup"><span data-stu-id="5e70e-126">To find packages, visit [NuGet.org](http://NuGet.org).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5e70e-127">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="5e70e-127">Additional Resources</span></span>

[<span data-ttu-id="5e70e-128">Le funzionalità migliori in ASP.NET Web Pages 2</span><span class="sxs-lookup"><span data-stu-id="5e70e-128">The Top Features in ASP.NET Web Pages 2</span></span>](top-features-in-web-pages-2.md)
