---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: Aggiunta di contenuto dinamico a una pagina memorizzata nella cache (c#) | Microsoft Docs
author: microsoft
description: Informazioni su come combinare il contenuto dinamico e memorizzati nella cache nella stessa pagina. Sostituzione post-cache consente di visualizzare il contenuto dinamico, ad esempio banner gli annunci o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 08f680e8d057f47a3f2802b1136edfb00634637d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364145"
---
<a name="adding-dynamic-content-to-a-cached-page-c"></a><span data-ttu-id="6459b-104">Aggiunta di contenuto dinamico a una pagina memorizzata nella cache (c#)</span><span class="sxs-lookup"><span data-stu-id="6459b-104">Adding Dynamic Content to a Cached Page (C#)</span></span>
====================
<span data-ttu-id="6459b-105">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="6459b-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="6459b-106">Informazioni su come combinare il contenuto dinamico e memorizzati nella cache nella stessa pagina.</span><span class="sxs-lookup"><span data-stu-id="6459b-106">Learn how to mix dynamic and cached content in the same page.</span></span> <span data-ttu-id="6459b-107">Sostituzione post-cache consente di visualizzare il contenuto dinamico, ad esempio pubblicitari o notizie, all'interno di una pagina in cui è stata di output memorizzate nella cache.</span><span class="sxs-lookup"><span data-stu-id="6459b-107">Post-cache substitution enables you to display dynamic content, such as banner advertisements or news items, within a page that has been output cached.</span></span>


<span data-ttu-id="6459b-108">Sfruttando i vantaggi della memorizzazione nella cache di output, è possibile migliorare notevolmente le prestazioni di un'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="6459b-108">By taking advantage of output caching, you can dramatically improve the performance of an ASP.NET MVC application.</span></span> <span data-ttu-id="6459b-109">Anziché la rigenerazione di una pagina ogni volta che viene richiesta la pagina, la pagina può essere generata una volta e memorizzata nella cache per più utenti.</span><span class="sxs-lookup"><span data-stu-id="6459b-109">Instead of regenerating a page each and every time the page is requested, the page can be generated once and cached in memory for multiple users.</span></span>

<span data-ttu-id="6459b-110">Ma si è verificato un problema.</span><span class="sxs-lookup"><span data-stu-id="6459b-110">But there is a problem.</span></span> <span data-ttu-id="6459b-111">Cosa accade se è necessario visualizzare il contenuto dinamico nella pagina?</span><span class="sxs-lookup"><span data-stu-id="6459b-111">What if you need to display dynamic content in the page?</span></span> <span data-ttu-id="6459b-112">Si supponga, ad esempio, che si desidera visualizzare un banner pubblicitario nella pagina.</span><span class="sxs-lookup"><span data-stu-id="6459b-112">For example, imagine that you want to display a banner advertisement in the page.</span></span> <span data-ttu-id="6459b-113">Non si desidera il banner pubblicitario da memorizzare nella cache in modo che ogni utente veda l'advertisement le stesse.</span><span class="sxs-lookup"><span data-stu-id="6459b-113">You don't want the banner advertisement to be cached so that every user sees the very same advertisement.</span></span> <span data-ttu-id="6459b-114">È non risulterebbero alcuna spesa in questo modo.</span><span class="sxs-lookup"><span data-stu-id="6459b-114">You wouldn't make any money that way!</span></span>

<span data-ttu-id="6459b-115">Fortunatamente, è una pratica soluzione.</span><span class="sxs-lookup"><span data-stu-id="6459b-115">Fortunately, there is an easy solution.</span></span> <span data-ttu-id="6459b-116">È possibile sfruttare i vantaggi di una funzionalità del framework ASP.NET chiamato *sostituzione post-cache*.</span><span class="sxs-lookup"><span data-stu-id="6459b-116">You can take advantage of a feature of the ASP.NET framework called *post-cache substitution*.</span></span> <span data-ttu-id="6459b-117">Sostituzione post-cache consente di sostituire il contenuto dinamico in una pagina in cui è stato memorizzato in memoria.</span><span class="sxs-lookup"><span data-stu-id="6459b-117">Post-cache substitution enables you to substitute dynamic content in a page that has been cached in memory.</span></span>


<span data-ttu-id="6459b-118">In genere, quando tramite l'attributo [OutputCache] è l'output della cache una pagina, la pagina viene memorizzato nella cache il server sia nel client (browser web).</span><span class="sxs-lookup"><span data-stu-id="6459b-118">Normally, when you output cache a page by using the [OutputCache] attribute, the page is cached on both the server and the client (the web browser).</span></span> <span data-ttu-id="6459b-119">Quando si usa la sostituzione post-cache, viene memorizzato nella cache una pagina solo sul server.</span><span class="sxs-lookup"><span data-stu-id="6459b-119">When you use post-cache substitution, a page is cached only on the server.</span></span>


#### <a name="using-post-cache-substitution"></a><span data-ttu-id="6459b-120">Usare la sostituzione post-Cache</span><span class="sxs-lookup"><span data-stu-id="6459b-120">Using Post-Cache Substitution</span></span>

<span data-ttu-id="6459b-121">L'utilizzo di sostituzione post-cache richiede due passaggi.</span><span class="sxs-lookup"><span data-stu-id="6459b-121">Using post-cache substitution requires two steps.</span></span> <span data-ttu-id="6459b-122">In primo luogo, è necessario definire un metodo che restituisce una stringa che rappresenta il contenuto dinamico che si desidera visualizzare nella pagina memorizzata nella cache.</span><span class="sxs-lookup"><span data-stu-id="6459b-122">First, you need to define a method that returns a string that represents the dynamic content that you want to display in the cached page.</span></span> <span data-ttu-id="6459b-123">Successivamente, chiamare il metodo HttpResponse.WriteSubstitution() per inserire il contenuto dinamico nella pagina.</span><span class="sxs-lookup"><span data-stu-id="6459b-123">Next, you call the HttpResponse.WriteSubstitution() method to inject the dynamic content into the page.</span></span>

<span data-ttu-id="6459b-124">Si supponga, ad esempio, che si desidera visualizzare in modo casuale diversi notizie in una pagina memorizzata nella cache.</span><span class="sxs-lookup"><span data-stu-id="6459b-124">Imagine, for example, that you want to randomly display different news items in a cached page.</span></span> <span data-ttu-id="6459b-125">La classe nel listato 1 espone un solo metodo, denominato RenderNews(), che restituisce in modo casuale un elemento di notizie da un elenco di tre articoli di notizie.</span><span class="sxs-lookup"><span data-stu-id="6459b-125">The class in Listing 1 exposes a single method, named RenderNews(), that randomly returns one news item from a list of three news items.</span></span>

<span data-ttu-id="6459b-126">**Listato 1 – Models\News.cs**</span><span class="sxs-lookup"><span data-stu-id="6459b-126">**Listing 1 – Models\News.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

<span data-ttu-id="6459b-127">Per sfruttare i vantaggi di sostituzione post-cache, chiamare il metodo HttpResponse.WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="6459b-127">To take advantage of post-cache substitution, you call the HttpResponse.WriteSubstitution() method.</span></span> <span data-ttu-id="6459b-128">Il metodo WriteSubstitution() imposta il codice per sostituire un'area della pagina memorizzata nella cache con contenuto dinamico.</span><span class="sxs-lookup"><span data-stu-id="6459b-128">The WriteSubstitution() method sets up the code to replace a region of the cached page with dynamic content.</span></span> <span data-ttu-id="6459b-129">Il metodo WriteSubstitution() consente di visualizzare la notizia casuali nella visualizzazione nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="6459b-129">The WriteSubstitution() method is used to display the random news item in the view in Listing 2.</span></span>

<span data-ttu-id="6459b-130">**Listato 2 – Views\Home\Index.aspx.**</span><span class="sxs-lookup"><span data-stu-id="6459b-130">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

<span data-ttu-id="6459b-131">Il metodo RenderNews viene passato al metodo WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="6459b-131">The RenderNews method is passed to the WriteSubstitution() method.</span></span> <span data-ttu-id="6459b-132">Si noti che non viene chiamato il metodo RenderNews (non esistono alcuna parentesi).</span><span class="sxs-lookup"><span data-stu-id="6459b-132">Notice that the RenderNews method is not called (there are no parentheses).</span></span> <span data-ttu-id="6459b-133">Viene invece passato un riferimento al metodo a WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="6459b-133">Instead a reference to the method is passed to WriteSubstitution().</span></span>

<span data-ttu-id="6459b-134">La visualizzazione dell'indice viene memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="6459b-134">The Index view is cached.</span></span> <span data-ttu-id="6459b-135">La vista viene restituita dal controller nel listato 3.</span><span class="sxs-lookup"><span data-stu-id="6459b-135">The view is returned by the controller in Listing 3.</span></span> <span data-ttu-id="6459b-136">Si noti che l'azione Index () è dotato di un attributo [OutputCache] che fa sì che la visualizzazione dell'indice da memorizzare nella cache per 60 secondi.</span><span class="sxs-lookup"><span data-stu-id="6459b-136">Notice that the Index() action is decorated with an [OutputCache] attribute that causes the Index view to be cached for 60 seconds.</span></span>

<span data-ttu-id="6459b-137">**Listato 3 – controllers\homecontroller.cs.**</span><span class="sxs-lookup"><span data-stu-id="6459b-137">**Listing 3 – Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

<span data-ttu-id="6459b-138">Anche se la visualizzazione dell'indice viene memorizzato nella cache, gli elementi diversi notizie casuali vengono visualizzati quando si richiede la pagina di indice.</span><span class="sxs-lookup"><span data-stu-id="6459b-138">Even though the Index view is cached, different random news items are displayed when you request the Index page.</span></span> <span data-ttu-id="6459b-139">Quando si richiede la pagina di indice, l'ora visualizzata dalla pagina rimane invariato per 60 secondi (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="6459b-139">When you request the Index page, the time displayed by the page does not change for 60 seconds (see Figure 1).</span></span> <span data-ttu-id="6459b-140">Il fatto che il cambiamento di ora non è noto che la pagina viene memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="6459b-140">The fact that the time does not change proves that the page is cached.</span></span> <span data-ttu-id="6459b-141">Tuttavia, il contenuto inserito dal metodo – casuale notizia – WriteSubstitution() viene modificato con ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="6459b-141">However, the content injected by the WriteSubstitution() method – the random news item – changes with each request .</span></span>

<span data-ttu-id="6459b-142">**Figura 1 – inserimento dinamici nuovi elementi in una pagina memorizzata nella cache**</span><span class="sxs-lookup"><span data-stu-id="6459b-142">**Figure 1 – Injecting dynamic news items in a cached page**</span></span>

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a><span data-ttu-id="6459b-144">Usare la sostituzione post-Cache nei metodi Helper</span><span class="sxs-lookup"><span data-stu-id="6459b-144">Using Post-Cache Substitution in Helper Methods</span></span>

<span data-ttu-id="6459b-145">Un modo più semplice per sfruttare i vantaggi di sostituzione post-cache è per incapsulare la chiamata al metodo WriteSubstitution() all'interno di un metodo helper personalizzati.</span><span class="sxs-lookup"><span data-stu-id="6459b-145">An easier way to take advantage of post-cache substitution is to encapsulate the call to the WriteSubstitution() method within a custom helper method.</span></span> <span data-ttu-id="6459b-146">Questo approccio è illustrato il metodo helper nel listato 4.</span><span class="sxs-lookup"><span data-stu-id="6459b-146">This approach is illustrated by the helper method in Listing 4.</span></span>

<span data-ttu-id="6459b-147">**Listato 4 – AdHelper.cs**</span><span class="sxs-lookup"><span data-stu-id="6459b-147">**Listing 4 – AdHelper.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

<span data-ttu-id="6459b-148">Listato 4 contiene una classe statica che espone due metodi: RenderBanner() e RenderBannerInternal().</span><span class="sxs-lookup"><span data-stu-id="6459b-148">Listing 4 contains a static class that exposes two methods: RenderBanner() and RenderBannerInternal().</span></span> <span data-ttu-id="6459b-149">Il metodo RenderBanner() rappresenta il metodo di supporto effettivo.</span><span class="sxs-lookup"><span data-stu-id="6459b-149">The RenderBanner() method represents the actual helper method.</span></span> <span data-ttu-id="6459b-150">Questo metodo estende la classe HtmlHelper MVC ASP.NET standard, in modo che sia possibile chiamare Html.RenderBanner() in una vista come qualsiasi altro metodo helper.</span><span class="sxs-lookup"><span data-stu-id="6459b-150">This method extends the standard ASP.NET MVC HtmlHelper class so that you can call Html.RenderBanner() in a view just like any other helper method.</span></span>

<span data-ttu-id="6459b-151">Il metodo RenderBanner() chiama il metodo HttpResponse.WriteSubstitution() passando il metodo RenderBannerInternal() al metodo WriteSubsitution().</span><span class="sxs-lookup"><span data-stu-id="6459b-151">The RenderBanner() method calls the HttpResponse.WriteSubstitution() method passing the RenderBannerInternal() method to the WriteSubsitution() method.</span></span>

<span data-ttu-id="6459b-152">Il metodo RenderBannerInternal() è un metodo privato.</span><span class="sxs-lookup"><span data-stu-id="6459b-152">The RenderBannerInternal() method is a private method.</span></span> <span data-ttu-id="6459b-153">Questo metodo non verrà esposto come un metodo helper.</span><span class="sxs-lookup"><span data-stu-id="6459b-153">This method won't be exposed as a helper method.</span></span> <span data-ttu-id="6459b-154">Il metodo RenderBannerInternal() restituisce in modo casuale un'immagine del banner degli annunci da un elenco di tre immagini annuncio banner.</span><span class="sxs-lookup"><span data-stu-id="6459b-154">The RenderBannerInternal() method randomly returns one banner advertisement image from a list of three banner advertisement images.</span></span>

<span data-ttu-id="6459b-155">La visualizzazione dell'indice modificata nel listato 5 viene illustrato come è possibile usare il metodo helper RenderBanner().</span><span class="sxs-lookup"><span data-stu-id="6459b-155">The modified Index view in Listing 5 illustrates how you can use the RenderBanner() helper method.</span></span> <span data-ttu-id="6459b-156">Si noti che un'ulteriore &lt;% @ % importazione&gt; direttiva è inclusa nella parte superiore della vista per importare lo spazio dei nomi MvcApplication1.Helpers.</span><span class="sxs-lookup"><span data-stu-id="6459b-156">Notice that an additional &lt;%@ Import %&gt; directive is included at the top of the view to import the MvcApplication1.Helpers namespace.</span></span> <span data-ttu-id="6459b-157">Se non si importa questo spazio dei nomi, il metodo RenderBanner() non sarà più visualizzato come un metodo sulla proprietà Html.</span><span class="sxs-lookup"><span data-stu-id="6459b-157">If you neglect to import this namespace, then the RenderBanner() method won't appear as a method on the Html property.</span></span>

<span data-ttu-id="6459b-158">**Listato 5 – Views\Home\Index.aspx. (con RenderBanner() metodo)**</span><span class="sxs-lookup"><span data-stu-id="6459b-158">**Listing 5 – Views\Home\Index.aspx (with RenderBanner() method)**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

<span data-ttu-id="6459b-159">Quando si richiede la pagina sottoposta a rendering da parte della vista nel listato 5, viene visualizzato un annuncio diversi banner con ogni richiesta (vedere la figura 2).</span><span class="sxs-lookup"><span data-stu-id="6459b-159">When you request the page rendered by the view in Listing 5, a different banner advertisement is displayed with each request (see Figure 2).</span></span> <span data-ttu-id="6459b-160">La pagina viene memorizzato nella cache, ma l'advertisement banner viene inserito in modo dinamico per il metodo helper RenderBanner().</span><span class="sxs-lookup"><span data-stu-id="6459b-160">The page is cached, but the banner advertisement is injected dynamically by the RenderBanner() helper method.</span></span>

<span data-ttu-id="6459b-161">**Figura 2: la visualizzazione dell'indice la visualizzazione di un annuncio di banner casuale**</span><span class="sxs-lookup"><span data-stu-id="6459b-161">**Figure 2 – The Index view displaying a random banner advertisement**</span></span>

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a><span data-ttu-id="6459b-163">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="6459b-163">Summary</span></span>

<span data-ttu-id="6459b-164">In questa esercitazione viene illustrato come è possibile aggiornare dinamicamente il contenuto in una pagina memorizzata nella cache.</span><span class="sxs-lookup"><span data-stu-id="6459b-164">This tutorial explained how you can dynamically update content in a cached page.</span></span> <span data-ttu-id="6459b-165">Si è appreso come usare il metodo HttpResponse.WriteSubstitution() per attivare il contenuto dinamico venga inserito in una pagina memorizzata nella cache.</span><span class="sxs-lookup"><span data-stu-id="6459b-165">You learned how to use the HttpResponse.WriteSubstitution() method to enable dynamic content to be injected in a cached page.</span></span> <span data-ttu-id="6459b-166">Anche appreso come incapsulare la chiamata al metodo WriteSubstitution() all'interno di un metodo di helper HTML.</span><span class="sxs-lookup"><span data-stu-id="6459b-166">You also learned how to encapsulate the call to the WriteSubstitution() method within an HTML helper method.</span></span>

<span data-ttu-id="6459b-167">Sfruttare i vantaggi della memorizzazione nella cache laddove possibile, può avere un impatto significativo sulle prestazioni delle applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="6459b-167">Take advantage of caching whenever possible – it can have a dramatic impact on the performance of your web applications.</span></span> <span data-ttu-id="6459b-168">Come illustrato in questa esercitazione, è possibile sfruttare i vantaggi della memorizzazione nella cache anche quando è necessario visualizzare il contenuto dinamico all'interno delle pagine.</span><span class="sxs-lookup"><span data-stu-id="6459b-168">As explained in this tutorial, you can take advantage of caching even when you need to display dynamic content in your pages.</span></span>

## 

## 

> [!div class="step-by-step"]
> <span data-ttu-id="6459b-169">[Precedente](improving-performance-with-output-caching-cs.md)
> [Successivo](creating-a-controller-cs.md)</span><span class="sxs-lookup"><span data-stu-id="6459b-169">[Previous](improving-performance-with-output-caching-cs.md)
[Next](creating-a-controller-cs.md)</span></span>
