---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: Utilizzo della classe TagBuilder per compilare l'helper HTML (c#) | Documenti Microsoft
author: StephenWalther
description: "Stephen Walther presenta una classe di utilità nel framework di MVC ASP.NET denominato la classe TagBuilder. È possibile utilizzare facilmente la classe TagBuilder per..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: ddc4e91bb14082c7c5e889d064d29d2bf91f7329
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a><span data-ttu-id="ed45f-104">Utilizzo della classe TagBuilder per compilare l'helper HTML (c#)</span><span class="sxs-lookup"><span data-stu-id="ed45f-104">Using the TagBuilder Class to Build HTML Helpers (C#)</span></span>
====================
<span data-ttu-id="ed45f-105">da [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="ed45f-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="ed45f-106">Stephen Walther presenta una classe di utilità nel framework di MVC ASP.NET denominato la classe TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="ed45f-106">Stephen Walther introduces you to a useful utility class in the ASP.NET MVC framework named the TagBuilder class.</span></span> <span data-ttu-id="ed45f-107">È possibile utilizzare la classe TagBuilder per compilare con facilità i tag HTML.</span><span class="sxs-lookup"><span data-stu-id="ed45f-107">You can use the TagBuilder class to easily build HTML tags.</span></span>


<span data-ttu-id="ed45f-108">Il framework di MVC ASP.NET include una classe di utilità denominata la classe TagBuilder che è possibile utilizzare quando si compila l'helper HTML.</span><span class="sxs-lookup"><span data-stu-id="ed45f-108">The ASP.NET MVC framework includes a useful utility class named the TagBuilder class that you can use when building HTML helpers.</span></span> <span data-ttu-id="ed45f-109">La classe TagBuilder, come suggerisce il nome della classe, consente di compilare con facilità i tag HTML.</span><span class="sxs-lookup"><span data-stu-id="ed45f-109">The TagBuilder class, as the name of the class suggests, enables you to easily build HTML tags.</span></span> <span data-ttu-id="ed45f-110">In questa breve esercitazione verranno fornite una panoramica della classe TagBuilder e illustrato come utilizzare questa classe quando la compilazione di un helper HTML semplice che esegue il rendering HTML &lt;img&gt; tag.</span><span class="sxs-lookup"><span data-stu-id="ed45f-110">In this brief tutorial, you are provided with an overview of the TagBuilder class and you learn how to use this class when building a simple HTML helper that renders HTML &lt;img&gt; tags.</span></span>

## <a name="overview-of-the-tagbuilder-class"></a><span data-ttu-id="ed45f-111">Panoramica della classe TagBuilder</span><span class="sxs-lookup"><span data-stu-id="ed45f-111">Overview of the TagBuilder Class</span></span>

<span data-ttu-id="ed45f-112">La classe TagBuilder è contenuta nello spazio dei nomi System.</span><span class="sxs-lookup"><span data-stu-id="ed45f-112">The TagBuilder class is contained in the System.Web.Mvc namespace.</span></span> <span data-ttu-id="ed45f-113">Contiene i cinque metodi:</span><span class="sxs-lookup"><span data-stu-id="ed45f-113">It has five methods:</span></span>

- <span data-ttu-id="ed45f-114">AddCssClass() - consente di aggiungere un nuovo *classe = ""* attributo a un tag.</span><span class="sxs-lookup"><span data-stu-id="ed45f-114">AddCssClass() - Enables you to add a new *class=""* attribute to a tag.</span></span>
- <span data-ttu-id="ed45f-115">GenerateId() - consente di aggiungere un attributo id a un tag.</span><span class="sxs-lookup"><span data-stu-id="ed45f-115">GenerateId() - Enables you to add an id attribute to a tag.</span></span> <span data-ttu-id="ed45f-116">Questo metodo sostituisce automaticamente i punti nell'id (per impostazione predefinita, punti vengono sostituiti da caratteri di sottolineatura)</span><span class="sxs-lookup"><span data-stu-id="ed45f-116">This method automatically replaces periods in the id (by default, periods are replaced by underscores)</span></span>
- <span data-ttu-id="ed45f-117">MergeAttribute() - consente di aggiungere attributi a un tag.</span><span class="sxs-lookup"><span data-stu-id="ed45f-117">MergeAttribute() - Enables you to add attributes to a tag.</span></span> <span data-ttu-id="ed45f-118">Sono disponibili più overload di questo metodo.</span><span class="sxs-lookup"><span data-stu-id="ed45f-118">There are multiple overloads of this method.</span></span>
- <span data-ttu-id="ed45f-119">SetInnerText() - consente di impostare il testo interno del tag.</span><span class="sxs-lookup"><span data-stu-id="ed45f-119">SetInnerText() - Enables you to set the inner text of the tag.</span></span> <span data-ttu-id="ed45f-120">Il testo interno è HTML codifica automaticamente.</span><span class="sxs-lookup"><span data-stu-id="ed45f-120">The inner text is HTML encode automatically.</span></span>
- <span data-ttu-id="ed45f-121">ToString () - consente di eseguire il rendering del tag.</span><span class="sxs-lookup"><span data-stu-id="ed45f-121">ToString() - Enables you to render the tag.</span></span> <span data-ttu-id="ed45f-122">È possibile specificare se si desidera creare un tag normale, un tag di inizio, un tag di fine o un tag di chiusura automatica.</span><span class="sxs-lookup"><span data-stu-id="ed45f-122">You can specify whether you want to create a normal tag, a start tag, an end tag, or a self-closing tag.</span></span>
  

<span data-ttu-id="ed45f-123">La classe TagBuilder ha quattro proprietà importanti:</span><span class="sxs-lookup"><span data-stu-id="ed45f-123">The TagBuilder class has four important properties:</span></span>

- <span data-ttu-id="ed45f-124">Attributi - rappresenta tutti gli attributi del tag.</span><span class="sxs-lookup"><span data-stu-id="ed45f-124">Attributes - Represents all of the attributes of the tag.</span></span>
- <span data-ttu-id="ed45f-125">IdAttributeDotReplacement - rappresenta il carattere utilizzato dal metodo GenerateId() per sostituire i (il valore predefinito è un carattere di sottolineatura).</span><span class="sxs-lookup"><span data-stu-id="ed45f-125">IdAttributeDotReplacement - Represents the character used by the GenerateId() method to replace periods (the default is an underscore).</span></span>
- <span data-ttu-id="ed45f-126">InnerHTML - rappresenta il contenuto interno del tag.</span><span class="sxs-lookup"><span data-stu-id="ed45f-126">InnerHTML - Represents the inner contents of the tag.</span></span> <span data-ttu-id="ed45f-127">Assegnare una stringa per questa proprietà *non* HTML codificare la stringa.</span><span class="sxs-lookup"><span data-stu-id="ed45f-127">Assigning a string to this property *does not* HTML encode the string.</span></span>
- <span data-ttu-id="ed45f-128">TagName - rappresenta il nome del tag.</span><span class="sxs-lookup"><span data-stu-id="ed45f-128">TagName - Represents the name of the tag.</span></span>

<span data-ttu-id="ed45f-129">Questi metodi e proprietà consentono di tutti i metodi di base e proprietà che è necessario creare un tag HTML.</span><span class="sxs-lookup"><span data-stu-id="ed45f-129">These methods and properties give you all of the basic methods and properties that you need to build up an HTML tag.</span></span> <span data-ttu-id="ed45f-130">Non è effettivamente necessario utilizzare la classe TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="ed45f-130">You don't really need to use the TagBuilder class.</span></span> <span data-ttu-id="ed45f-131">In alternativa, è possibile utilizzare una classe StringBuilder.</span><span class="sxs-lookup"><span data-stu-id="ed45f-131">You could use a StringBuilder class instead.</span></span> <span data-ttu-id="ed45f-132">Tuttavia, la classe TagBuilder semplifica la vita leggermente.</span><span class="sxs-lookup"><span data-stu-id="ed45f-132">However, the TagBuilder class makes your life a little easier.</span></span>

## <a name="creating-an-image-html-helper"></a><span data-ttu-id="ed45f-133">Creazione di un HTML Helper immagine</span><span class="sxs-lookup"><span data-stu-id="ed45f-133">Creating an Image HTML Helper</span></span>

<span data-ttu-id="ed45f-134">Quando si crea un'istanza della classe TagBuilder, si passa il nome del tag che si desidera compilare al costruttore TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="ed45f-134">When you create an instance of the TagBuilder class, you pass the name of the tag that you want to build to the TagBuilder constructor.</span></span> <span data-ttu-id="ed45f-135">Successivamente, è possibile chiamare i metodi, ad esempio i metodi AddCssClass e MergeAttribute() per modificare gli attributi del tag.</span><span class="sxs-lookup"><span data-stu-id="ed45f-135">Next, you can call methods like the AddCssClass and MergeAttribute() methods to modify the attributes of the tag.</span></span> <span data-ttu-id="ed45f-136">Infine, chiamare il metodo ToString () per il rendering del tag.</span><span class="sxs-lookup"><span data-stu-id="ed45f-136">Finally, you call the ToString() method to render the tag.</span></span>

<span data-ttu-id="ed45f-137">Ad esempio, listato 1 contiene un helper HTML di immagine.</span><span class="sxs-lookup"><span data-stu-id="ed45f-137">For example, Listing 1 contains an Image HTML helper.</span></span> <span data-ttu-id="ed45f-138">L'helper di immagine viene implementato internamente con un TagBuilder che rappresenta un elemento HTML &lt;img&gt; tag.</span><span class="sxs-lookup"><span data-stu-id="ed45f-138">The Image helper is implemented internally with a TagBuilder that represents an HTML &lt;img&gt; tag.</span></span>

<span data-ttu-id="ed45f-139">**Elenco 1 - Helpers\ImageHelper.cs**</span><span class="sxs-lookup"><span data-stu-id="ed45f-139">**Listing 1 - Helpers\ImageHelper.cs**</span></span>

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

<span data-ttu-id="ed45f-140">La classe nel listato 1 contiene due metodi di overload statici denominati immagine.</span><span class="sxs-lookup"><span data-stu-id="ed45f-140">The class in Listing 1 contains two static overloaded methods named Image.</span></span> <span data-ttu-id="ed45f-141">Quando si chiama il metodo Image(), è possibile passare un oggetto che rappresenta un set di attributi HTML o meno.</span><span class="sxs-lookup"><span data-stu-id="ed45f-141">When you call the Image() method, you can pass an object which represents a set of HTML attributes or not.</span></span>

<span data-ttu-id="ed45f-142">Si noti come il metodo di TagBuilder.MergeAttribute() viene utilizzato per aggiungere singoli attributi, ad esempio l'attributo src di TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="ed45f-142">Notice how the TagBuilder.MergeAttribute() method is used to add individual attributes such as the src attribute to the TagBuilder.</span></span> <span data-ttu-id="ed45f-143">Si noti, inoltre, come il metodo di TagBuilder.MergeAttributes() viene utilizzato per aggiungere una raccolta di attributi per il TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="ed45f-143">Notice, furthermore, how the TagBuilder.MergeAttributes() method is used to add a collection of attributes to the TagBuilder.</span></span> <span data-ttu-id="ed45f-144">Il metodo MergeAttributes() accetta un dizionario&lt;stringa, oggetto&gt; parametro.</span><span class="sxs-lookup"><span data-stu-id="ed45f-144">The MergeAttributes() method accepts a Dictionary&lt;string,object&gt; parameter.</span></span> <span data-ttu-id="ed45f-145">La classe di RouteValueDictionary viene usata per convertire l'oggetto che rappresenta la raccolta di attributi in un dizionario&lt;stringa, oggetto&gt;.</span><span class="sxs-lookup"><span data-stu-id="ed45f-145">The The RouteValueDictionary class is used to convert the Object representing the collection of attributes into a Dictionary&lt;string,object&gt;.</span></span>

<span data-ttu-id="ed45f-146">Dopo aver creato il supporto di immagine, è possibile utilizzare l'helper in visualizzazioni MVC ASP.NET come uno degli altri helper HTML standard.</span><span class="sxs-lookup"><span data-stu-id="ed45f-146">After you create the Image helper, you can use the helper in your ASP.NET MVC views just like any of the other standard HTML helpers.</span></span> <span data-ttu-id="ed45f-147">La visualizzazione nel listato 2 utilizza l'helper di immagine da visualizzare due volte la stessa immagine di Xbox (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="ed45f-147">The view in Listing 2 uses the Image helper to display the same image of an Xbox twice (see Figure 1).</span></span> <span data-ttu-id="ed45f-148">L'helper Image() viene chiamato con e senza una raccolta di attributi HTML.</span><span class="sxs-lookup"><span data-stu-id="ed45f-148">The Image() helper is called both with and without an HTML attribute collection.</span></span>

<span data-ttu-id="ed45f-149">**Elenco di 2 - Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="ed45f-149">**Listing 2 - Home\Index.aspx**</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]


<span data-ttu-id="ed45f-150">[![La finestra di dialogo Nuovo progetto](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ed45f-150">[![The New Project dialog box](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="ed45f-151">**Figura 01**: utilizzo dell'helper di immagine ([fare clic per visualizzare l'immagine ingrandita](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="ed45f-151">**Figure 01**: Using the Image helper([Click to view full-size image](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))</span></span>


<span data-ttu-id="ed45f-152">Si noti che è necessario importare lo spazio dei nomi associato all'helper immagine nella parte superiore della visualizzazione Index.aspx.</span><span class="sxs-lookup"><span data-stu-id="ed45f-152">Notice that you must import the namespace associated with the Image helper at the top of the Index.aspx view.</span></span> <span data-ttu-id="ed45f-153">L'helper viene importata con la direttiva seguente:</span><span class="sxs-lookup"><span data-stu-id="ed45f-153">The helper is imported with the following directive:</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

>[!div class="step-by-step"]
<span data-ttu-id="ed45f-154">[Precedente](creating-custom-html-helpers-cs.md)
[Successivo](creating-page-layouts-with-view-master-pages-cs.md)</span><span class="sxs-lookup"><span data-stu-id="ed45f-154">[Previous](creating-custom-html-helpers-cs.md)
[Next](creating-page-layouts-with-view-master-pages-cs.md)</span></span>
