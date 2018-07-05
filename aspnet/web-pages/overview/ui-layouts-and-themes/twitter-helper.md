---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Helper di Twitter con pagine Web ASP.NET | Microsoft Docs
author: tfitzmac
description: Questo argomento e l'applicazione illustrano come aggiungere un Helper di Twitter per WebMatrix 3 progetto. Contiene il codice Helper di Twitter e viene illustrato come chiamare il supporto...
ms.author: aspnetcontent
ms.date: 02/07/2014
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 1824b677a7ba96ea6fc5119610725a30d472764e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817655"
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="208b8-104">Helper di Twitter con pagine Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="208b8-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="208b8-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="208b8-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="208b8-106">Questo argomento e l'applicazione illustrano come aggiungere un Helper di Twitter per WebMatrix 3 progetto.</span><span class="sxs-lookup"><span data-stu-id="208b8-106">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="208b8-107">Contiene il codice Helper di Twitter e viene illustrato come chiamare i metodi di supporto.</span><span class="sxs-lookup"><span data-stu-id="208b8-107">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="208b8-108">Questo codice per il file Twitter.cshtml sviluppato **Tian Pan** di Microsoft.</span><span class="sxs-lookup"><span data-stu-id="208b8-108">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="208b8-109">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="208b8-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="208b8-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="208b8-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="208b8-111">Questa esercitazione si integra inoltre con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="208b8-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="208b8-112">Introduzione</span><span class="sxs-lookup"><span data-stu-id="208b8-112">Introduction</span></span>

<span data-ttu-id="208b8-113">In questo argomento viene illustrato come aggiungere un Helper di Twitter per l'applicazione e usare la sintassi Razor per chiamare i metodi helper.</span><span class="sxs-lookup"><span data-stu-id="208b8-113">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="208b8-114">L'Helper di Twitter rende più facile incorporare widget nell'applicazione e i pulsanti di Twitter.</span><span class="sxs-lookup"><span data-stu-id="208b8-114">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="208b8-115">Per usare un widget di Twitter, ad esempio diario dell'utente o i risultati della ricerca per un hashtag, è necessario creare innanzitutto le [widget su Twitter](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="208b8-115">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="208b8-116">Dopo aver creato il widget, si riceverà un id widget. Questo id widget passare come parametro quando si chiamano i metodi di supporto che mostrano i widget.</span><span class="sxs-lookup"><span data-stu-id="208b8-116">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="208b8-117">In questo argomento è stato scritto per la versione 1.1 dell'API di Twitter.</span><span class="sxs-lookup"><span data-stu-id="208b8-117">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="208b8-118">Aggiungendo direttamente il codice Helper di Twitter per il progetto, è possibile aggiornare il codice helper se cambia l'API di Twitter.</span><span class="sxs-lookup"><span data-stu-id="208b8-118">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="208b8-119">Per informazioni sull'installazione di WebMatrix, vedere [Introduzione a ASP.NET Web Pages 2 - Introduzione a](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="208b8-119">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="208b8-120">Aggiungere al progetto Helper di Twitter</span><span class="sxs-lookup"><span data-stu-id="208b8-120">Add Twitter Helper to your project</span></span>

<span data-ttu-id="208b8-121">Per aggiungere l'Helper di Twitter, in primo luogo, aggiungere una cartella denominata **App\_codice** al progetto.</span><span class="sxs-lookup"><span data-stu-id="208b8-121">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="208b8-122">Quindi, creare un file denominato **Twitter.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="208b8-122">Then, create a file named **Twitter.cshtml**.</span></span>

![Nella cartella App_Code](twitter-helper/_static/image1.png)

<span data-ttu-id="208b8-124">Sostituire il codice predefinito in Twitter.cshtml con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="208b8-124">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="208b8-125">Chiamare i metodi di Twitter dalle pagine web</span><span class="sxs-lookup"><span data-stu-id="208b8-125">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="208b8-126">Nell'esempio seguente viene illustrato come utilizzare i metodi Helper di Twitter da una pagina nel progetto.</span><span class="sxs-lookup"><span data-stu-id="208b8-126">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="208b8-127">Nel progetto, si dovranno sostituire i valori dei parametri con valori attinenti alle proprie esigenze.</span><span class="sxs-lookup"><span data-stu-id="208b8-127">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="208b8-128">È possibile usare gli ID widget forniti per esplorare il funzionamento dei metodi, ma è opportuno generare widget personalizzati per il progetto.</span><span class="sxs-lookup"><span data-stu-id="208b8-128">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="208b8-129">Non tutti i parametri riportati di seguito sono obbligatori.</span><span class="sxs-lookup"><span data-stu-id="208b8-129">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="208b8-130">I parametri facoltativi vengono utilizzati per personalizzare come viene visualizzato il pulsante o un widget.</span><span class="sxs-lookup"><span data-stu-id="208b8-130">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="208b8-131">Ad esempio, il pulsante seguire richiede solo il nome utente da seguire, ma l'esempio illustra come includere il numero di follower e come specificare le dimensioni del pulsante e la lingua.</span><span class="sxs-lookup"><span data-stu-id="208b8-131">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="208b8-132">Visualizzare i risultati</span><span class="sxs-lookup"><span data-stu-id="208b8-132">See the results</span></span>

<span data-ttu-id="208b8-133">Il codice precedente produce i seguenti pulsanti e widget.</span><span class="sxs-lookup"><span data-stu-id="208b8-133">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="208b8-134">Questi pulsanti e widget sono completamente funzionali, non schermate.</span><span class="sxs-lookup"><span data-stu-id="208b8-134">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="208b8-135">Il pulsante seguire viene visualizzato in spagnolo perché il parametro language è impostato su **es**.</span><span class="sxs-lookup"><span data-stu-id="208b8-135">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="208b8-136">Seguire pulsante</span><span class="sxs-lookup"><span data-stu-id="208b8-136">Follow Button</span></span>

<span data-ttu-id="208b8-137">[Seguire @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="208b8-137">[Follow @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="tweet-button"></a><span data-ttu-id="208b8-138">Pulsante del TWEET</span><span class="sxs-lookup"><span data-stu-id="208b8-138">Tweet Button</span></span>

<span data-ttu-id="208b8-139">[TWEET](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="208b8-139">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="208b8-140">Sequenza temporale dell'utente (profilo)</span><span class="sxs-lookup"><span data-stu-id="208b8-140">User Timeline (Profile)</span></span>

<span data-ttu-id="208b8-141">[TWEET da @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="208b8-141">[Tweets by @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="favorites"></a><span data-ttu-id="208b8-142">Preferiti</span><span class="sxs-lookup"><span data-stu-id="208b8-142">Favorites</span></span>

<span data-ttu-id="208b8-143">[TWEET a Preferiti @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="208b8-143">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="list"></a><span data-ttu-id="208b8-144">List</span><span class="sxs-lookup"><span data-stu-id="208b8-144">List</span></span>

<span data-ttu-id="208b8-145">[Da un TWEET @Microsoft/MS \_Consumer\_bande](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="208b8-145">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="search"></a><span data-ttu-id="208b8-146">Cerca</span><span class="sxs-lookup"><span data-stu-id="208b8-146">Search</span></span>

<span data-ttu-id="208b8-147">[I TWEET su &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="208b8-147">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>
