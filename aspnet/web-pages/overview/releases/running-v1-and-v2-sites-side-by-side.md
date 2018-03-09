---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: Che eseguono versioni diverse delle pagine Web ASP.NET (Razor) Side-by | Documenti Microsoft
author: tfitzmac
description: In questo articolo viene illustrato come eseguire i siti Web di ASP.NET Web Pages (Razor) nello stesso computer o server quando i siti Web sono configurati per utilizzare versioni diverse...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: c11399b0bde59d18fa378ed48c15844454c1f956
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a><span data-ttu-id="b0c82-103">Esecuzione side-by-Side di versioni diverse delle pagine Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="b0c82-103">Running Different Versions of ASP.NET Web Pages (Razor) Side by Side</span></span>
====================
<span data-ttu-id="b0c82-104">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b0c82-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b0c82-105">In questo articolo viene illustrato come eseguire i siti Web di ASP.NET Web Pages (Razor) nello stesso computer o server quando i siti Web sono configurati per utilizzare versioni diverse di ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="b0c82-105">This article explains how to run ASP.NET Web Pages (Razor) websites on the same computer or server when the websites are configured to use different versions of ASP.NET Web Pages.</span></span>
> 
> <span data-ttu-id="b0c82-106">Illustra quanto segue:</span><span class="sxs-lookup"><span data-stu-id="b0c82-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="b0c82-107">Il comportamento predefinito novità in ASP.NET quando si dispone di siti creati con ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="b0c82-107">What the default behavior is in ASP.NET when you have sites built with ASP.NET Web Pages.</span></span>
> - <span data-ttu-id="b0c82-108">Come configurare un nuovo sito per l'esecuzione con una versione precedente di ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="b0c82-108">How to configure a new site to run with an older version of ASP.NET Web Pages.</span></span>
>   
> 
> <span data-ttu-id="b0c82-109">Si tratta della funzionalità ASP.NET introdotta nell'articolo:</span><span class="sxs-lookup"><span data-stu-id="b0c82-109">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="b0c82-110">Il `webPages:Version` impostazione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="b0c82-110">The `webPages:Version` configuration setting.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="b0c82-111">Versioni del software</span><span class="sxs-lookup"><span data-stu-id="b0c82-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="b0c82-112">Pagine Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="b0c82-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="b0c82-113">In questa esercitazione si integra inoltre con ASP.NET Web Pages 2 e pagine Web ASP.NET 1.0.</span><span class="sxs-lookup"><span data-stu-id="b0c82-113">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="b0c82-114">Le pagine Web ASP.NET supporta la possibilità di eseguire siti Web side-by.</span><span class="sxs-lookup"><span data-stu-id="b0c82-114">ASP.NET Web Pages supports the ability to run websites side by side.</span></span> <span data-ttu-id="b0c82-115">Ciò consente di continuare a eseguire le applicazioni ASP.NET Web Pages precedenti, creare nuove applicazioni ASP.NET Web Pages ed eseguire tutti gli elementi nello stesso computer.</span><span class="sxs-lookup"><span data-stu-id="b0c82-115">This lets you continue to run your older ASP.NET Web Pages applications, build new ASP.NET Web Pages applications, and run all of them on the same computer.</span></span>

<span data-ttu-id="b0c82-116">Di seguito sono illustrati alcuni aspetti da ricordare quando si installano le pagine Web con WebMatrix:</span><span class="sxs-lookup"><span data-stu-id="b0c82-116">Here are some things to remember when you install the Web Pages with WebMatrix:</span></span>

- <span data-ttu-id="b0c82-117">Per impostazione predefinita, le applicazioni esistenti di pagine Web verranno eseguita come la versione più recente nel computer in uso.</span><span class="sxs-lookup"><span data-stu-id="b0c82-117">By default, existing Web Pages applications will run as the latest version on your computer.</span></span> <span data-ttu-id="b0c82-118">(Gli assembly vengono installati nella global assembly cache (GAC) e vengono usati automaticamente).</span><span class="sxs-lookup"><span data-stu-id="b0c82-118">(The assemblies are installed in the global assembly cache (GAC) and are used automatically.)</span></span>
- <span data-ttu-id="b0c82-119">Se si desidera eseguire un sito con una versione diversa di ASP.NET Web Pages, è possibile configurare il sito a tale scopo.</span><span class="sxs-lookup"><span data-stu-id="b0c82-119">If you want to run a site using a different version of ASP.NET Web Pages, you can configure the site to do that.</span></span> <span data-ttu-id="b0c82-120">Se il sito non ha ancora un *Web. config* file nella radice del sito, crearne uno nuovo e copiarvi il seguente codice XML, sovrascrivendo il contenuto esistente.</span><span class="sxs-lookup"><span data-stu-id="b0c82-120">If your site doesn't already have a *web.config* file in the root of the site, create a new one and copy the following XML into it, overwriting the existing content.</span></span> <span data-ttu-id="b0c82-121">Se il sito contiene già un *Web. config* file, aggiungere un `<appSettings>` elemento simile a quello riportato di seguito per il `<configuration>` sezione.</span><span class="sxs-lookup"><span data-stu-id="b0c82-121">If the site already contains a *web.config* file, add an `<appSettings>` element like the following one to the `<configuration>` section.</span></span>

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
<span data-ttu-id="b0c82-122">'-Se non si specifica una versione di *Web. config* file, un sito viene distribuito come la versione più recente.</span><span class="sxs-lookup"><span data-stu-id="b0c82-122">\`- If you do not specify a version in the *web.config* file, a site is deployed as the latest version.</span></span> <span data-ttu-id="b0c82-123">(Gli assembly vengono copiati il *bin* cartella nel sito distribuito.)</span><span class="sxs-lookup"><span data-stu-id="b0c82-123">(The assemblies are copied to the *bin* folder in the deployed site.)</span></span>
- <span data-ttu-id="b0c82-124">Le nuove applicazioni create utilizzando i modelli di sito Web matrice includono gli assembly della versione pagine Web del sito *bin* cartella.</span><span class="sxs-lookup"><span data-stu-id="b0c82-124">New applications that you create using the site templates in Web Matrix include the Web Pages version assemblies in the site's *bin* folder.</span></span>

<span data-ttu-id="b0c82-125">In generale, è sempre possibile controllare quale versione delle pagine Web da usare con il sito usando NuGet per installare gli assembly appropriati al sito *bin* cartella.</span><span class="sxs-lookup"><span data-stu-id="b0c82-125">In general, you can always control which version of Web Pages to use with your site by using NuGet to install the appropriate assemblies into the site's *bin* folder.</span></span> <span data-ttu-id="b0c82-126">Per trovare i pacchetti, visitare [NuGet.org](http://NuGet.org).</span><span class="sxs-lookup"><span data-stu-id="b0c82-126">To find packages, visit [NuGet.org](http://NuGet.org).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b0c82-127">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b0c82-127">Additional Resources</span></span>

[<span data-ttu-id="b0c82-128">Le funzionalità principali di ASP.NET Web Pages 2</span><span class="sxs-lookup"><span data-stu-id="b0c82-128">The Top Features in ASP.NET Web Pages 2</span></span>](top-features-in-web-pages-2.md)
