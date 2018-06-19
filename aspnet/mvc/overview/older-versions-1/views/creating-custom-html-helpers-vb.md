---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: Creazione di helper HTML personalizzati (VB) | Documenti Microsoft
author: microsoft
description: L'obiettivo di questa esercitazione è dimostrare come è possibile creare l'helper HTML personalizzati che è possibile utilizzare all'interno di visualizzazioni MVC. L'utilizzo degli HTML Helper...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 6980026e2653eacb71697f9b34def9bc38638726
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871506"
---
<a name="creating-custom-html-helpers-vb"></a><span data-ttu-id="4e9a6-104">Creazione di helper HTML personalizzati (VB)</span><span class="sxs-lookup"><span data-stu-id="4e9a6-104">Creating Custom HTML Helpers (VB)</span></span>
====================
<span data-ttu-id="4e9a6-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="4e9a6-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="4e9a6-106">Scarica il PDF</span><span class="sxs-lookup"><span data-stu-id="4e9a6-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> <span data-ttu-id="4e9a6-107">L'obiettivo di questa esercitazione è dimostrare come è possibile creare l'helper HTML personalizzati che è possibile utilizzare all'interno di visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="4e9a6-108">Sfruttando degli helper HTML, è possibile ridurre la quantità di noiosa digitazione dei tag HTML che è necessario eseguire per creare una pagina HTML standard.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>


<span data-ttu-id="4e9a6-109">L'obiettivo di questa esercitazione è dimostrare come è possibile creare l'helper HTML personalizzati che è possibile utilizzare all'interno di visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="4e9a6-110">Sfruttando degli helper HTML, è possibile ridurre la quantità di noiosa digitazione dei tag HTML che è necessario eseguire per creare una pagina HTML standard.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="4e9a6-111">Nella prima parte di questa esercitazione, descrivo alcuni degli helper HTML esistente incluso con il framework di MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="4e9a6-112">Successivamente, descrivo due metodi di creazione di helper HTML personalizzati: illustrano come creare l'helper HTML personalizzati creando un metodo condiviso e la creazione di un metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a shared method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="4e9a6-113">Comprendere l'helper HTML</span><span class="sxs-lookup"><span data-stu-id="4e9a6-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="4e9a6-114">Un HTML Helper è semplicemente un metodo che restituisce una stringa.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="4e9a6-115">La stringa può rappresentare qualsiasi tipo di contenuto che si desidera.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="4e9a6-116">Ad esempio, è possibile utilizzare l'helper HTML per eseguire il rendering di tag HTML standard come HTML `<input>` e `<img>` tag.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="4e9a6-117">È inoltre possibile utilizzare helper HTML per il rendering del contenuto più complesso, ad esempio un elenco di schede o una tabella HTML di dati del database.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="4e9a6-118">Il framework di MVC ASP.NET include il seguente set di standard helper HTML (ciò non è un elenco completo):</span><span class="sxs-lookup"><span data-stu-id="4e9a6-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="4e9a6-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="4e9a6-119">Html.ActionLink()</span></span>
- <span data-ttu-id="4e9a6-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="4e9a6-120">Html.BeginForm()</span></span>
- <span data-ttu-id="4e9a6-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="4e9a6-121">Html.CheckBox()</span></span>
- <span data-ttu-id="4e9a6-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="4e9a6-122">Html.DropDownList()</span></span>
- <span data-ttu-id="4e9a6-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="4e9a6-123">Html.EndForm()</span></span>
- <span data-ttu-id="4e9a6-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="4e9a6-124">Html.Hidden()</span></span>
- <span data-ttu-id="4e9a6-125">Html.ListBox()</span><span class="sxs-lookup"><span data-stu-id="4e9a6-125">Html.ListBox()</span></span>
- <span data-ttu-id="4e9a6-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="4e9a6-126">Html.Password()</span></span>
- <span data-ttu-id="4e9a6-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="4e9a6-127">Html.RadioButton()</span></span>
- <span data-ttu-id="4e9a6-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="4e9a6-128">Html.TextArea()</span></span>
- <span data-ttu-id="4e9a6-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="4e9a6-129">Html.TextBox()</span></span>

<span data-ttu-id="4e9a6-130">Si consideri ad esempio il modulo nel listato 1.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="4e9a6-131">Questo modulo viene eseguito il rendering con l'aiuto di due degli helper HTML standard (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="4e9a6-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="4e9a6-132">Questo modulo Usa la `Html.BeginForm()` e `Html.TextBox()` metodi di supporto.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods.</span></span>


<span data-ttu-id="4e9a6-133">[![Pagina sottoposta a rendering con helper HTML](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4e9a6-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span></span>

<span data-ttu-id="4e9a6-134">**Figura 01**: pagina sottoposta a rendering con helper HTML ([fare clic per visualizzare l'immagine ingrandita](creating-custom-html-helpers-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4e9a6-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image3.png))</span></span>


<span data-ttu-id="4e9a6-135">**Elenco 1: `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="4e9a6-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

<span data-ttu-id="4e9a6-136">Il `Html.BeginForm()` metodo Helper viene utilizzato per creare il codice HTML di apertura e chiusura `<form>` tag.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-136">The `Html.BeginForm()` Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="4e9a6-137">Si noti che il `Html.BeginForm()` metodo viene chiamato all'interno di un utilizzo istruzione.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="4e9a6-138">L'istruzione using garantisce che il `<form>` tag viene chiuso alla fine della mediante blocco.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="4e9a6-139">Se si preferisce, invece di creare un utilizzando blocco, è possibile chiamare il metodo di supporto Html.EndForm() per chiudere la `<form>` tag.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="4e9a6-140">Utilizzare qualsiasi approccio alla creazione di apertura e chiusura `<form>` tag apparentemente più intuitiva.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="4e9a6-141">Il `Html.TextBox()` vengono utilizzati metodi Helper nel listato 1 per il rendering HTML `<input>` tag.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="4e9a6-142">Se si seleziona HTML nel browser viene visualizzato l'origine HTML nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="4e9a6-143">Si noti che l'origine contiene tag HTML standard.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4e9a6-144">Si noti che il `Html.TextBox()`-HTML Helper viene eseguito il rendering con `<%= %>` tag anziché `<% %>` tag.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="4e9a6-145">Se non si include il segno di uguale, non viene eseguito nel browser.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="4e9a6-146">Il framework ASP.NET MVC contiene un piccolo set di supporti.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="4e9a6-147">Molto probabilmente, è necessario estendere il framework MVC con helper HTML personalizzati.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="4e9a6-148">Nel resto di questa esercitazione, si apprenderà due metodi di creazione di helper HTML personalizzati.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="4e9a6-149">**Elenco 2: `Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="4e9a6-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a><span data-ttu-id="4e9a6-150">Creazione di helper HTML con i metodi condivisi</span><span class="sxs-lookup"><span data-stu-id="4e9a6-150">Creating HTML Helpers with Shared Methods</span></span>

<span data-ttu-id="4e9a6-151">Il modo più semplice per creare un nuovo HTML Helper consiste nel creare un metodo condiviso che restituisce una stringa.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-151">The easiest way to create a new HTML Helper is to create a shared method that returns a string.</span></span> <span data-ttu-id="4e9a6-152">Si supponga, ad esempio, che si decide di creare un nuovo HTML Helper che esegue il rendering HTML `<label>` tag.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="4e9a6-153">È possibile utilizzare la classe nel listato 2 per il rendering di un `<label>`.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-153">You can use the class in Listing 2 to render a `<label>`.</span></span>

<span data-ttu-id="4e9a6-154">**Elenco 2: `Helpers\LabelHelper.vb`**</span><span class="sxs-lookup"><span data-stu-id="4e9a6-154">**Listing 2 – `Helpers\LabelHelper.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

<span data-ttu-id="4e9a6-155">Non c'è niente di speciale sulla classe listato 2.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="4e9a6-156">Il `Label()` metodo restituisce semplicemente una stringa.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="4e9a6-157">La visualizzazione dell'indice modificata nel listato 3 Usa il `LabelHelper` per il rendering HTML `<label>` tag.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="4e9a6-158">Si noti che la visualizzazione include un `<%@ imports %>` direttiva che importa lo spazio dei nomi Application1.Helpers.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-158">Notice that the view includes an `<%@ imports %>` directive that imports the Application1.Helpers namespace.</span></span>

<span data-ttu-id="4e9a6-159">**Elenco 2: `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="4e9a6-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="4e9a6-160">Creazione di helper HTML con metodi di estensione</span><span class="sxs-lookup"><span data-stu-id="4e9a6-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="4e9a6-161">Se si desidera creare helper HTML che funzionano solo come gli helper HTML standard incluso nel framework di MVC ASP.NET è necessario creare metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="4e9a6-162">Metodi di estensione consentono di aggiungere nuovi metodi a una classe esistente.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="4e9a6-163">Quando si crea un metodo HTML Helper, si aggiungono nuovi metodi per la `HtmlHelper` classe rappresentato dalla proprietà di una visualizzazione Html.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-163">When creating an HTML Helper method, you add new methods to the `HtmlHelper` class represented by a view's Html property.</span></span>

<span data-ttu-id="4e9a6-164">Il modulo di Visual Basic nel listato 3 aggiunge un metodo di estensione denominato `Label()` per la `HtmlHelper` classe.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-164">The Visual Basic module in Listing 3 adds an extension method named `Label()` to the `HtmlHelper` class.</span></span> <span data-ttu-id="4e9a6-165">Esistono un paio di aspetti che è opportuno notare su questo modulo.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-165">There are a couple of things that you should notice about this module.</span></span> <span data-ttu-id="4e9a6-166">In primo luogo, si noti che il modulo è decorato con il `<Extension()>` attributo.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-166">First, notice that the module is decorated with the `<Extension()>` attribute.</span></span> <span data-ttu-id="4e9a6-167">Per usare questo attributo, è necessario importare il `System.Runtime.CompilerServices` dello spazio dei nomi</span><span class="sxs-lookup"><span data-stu-id="4e9a6-167">In order to use this attribute, you must import the `System.Runtime.CompilerServices` namespace</span></span>

<span data-ttu-id="4e9a6-168">In secondo luogo, si noti che il primo parametro del `Label()` metodo rappresenta la `HtmlHelper` classe.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-168">Second, notice that the first parameter of the `Label()` method represents the `HtmlHelper` class.</span></span> <span data-ttu-id="4e9a6-169">Il primo parametro di un metodo di estensione indica la classe che estende il metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="4e9a6-170">**Elenco di 3: `Helpers\LabelExtensions.vb`**</span><span class="sxs-lookup"><span data-stu-id="4e9a6-170">**Listing 3 – `Helpers\LabelExtensions.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

<span data-ttu-id="4e9a6-171">Dopo aver creare un metodo di estensione e compilare correttamente l'applicazione, il metodo di estensione viene visualizzato in Visual Studio Intellisense, come tutti gli altri metodi di una classe (vedere la figura 2).</span><span class="sxs-lookup"><span data-stu-id="4e9a6-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="4e9a6-172">L'unica differenza è che estensione metodi vengono visualizzati con un simbolo speciale accanto agli (un'icona di una freccia verso il basso).</span><span class="sxs-lookup"><span data-stu-id="4e9a6-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>


<span data-ttu-id="4e9a6-173">[![Utilizzando il metodo di estensione Html.Label()](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="4e9a6-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span></span>

<span data-ttu-id="4e9a6-174">**Figura 02**: tramite il metodo di estensione Html.Label() ([fare clic per visualizzare l'immagine ingrandita](creating-custom-html-helpers-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="4e9a6-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image6.png))</span></span>


<span data-ttu-id="4e9a6-175">La visualizzazione dell'indice modificata listato 4 utilizza il metodo di estensione Html.Label() per eseguire il rendering di tutti i relativi &lt;etichetta&gt; tag.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its &lt;label&gt; tags.</span></span>

<span data-ttu-id="4e9a6-176">**Elenco di 4: `Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="4e9a6-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="4e9a6-177">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="4e9a6-177">Summary</span></span>

<span data-ttu-id="4e9a6-178">In questa esercitazione è stato due metodi di creazione di helper HTML personalizzati.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="4e9a6-179">In primo luogo, si è appreso come creare una classe personalizzata `Label()` HTML Helper mediante la creazione di un metodo condiviso che restituisce una stringa.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-179">First, you learned how to create a custom `Label()` HTML Helper by creating a shared method that returns a string.</span></span> <span data-ttu-id="4e9a6-180">Successivamente, si è appreso come creare una classe personalizzata `Label()` metodo HTML Helper mediante la creazione di un metodo di estensione sulla `HtmlHelper` classe.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="4e9a6-181">In questa esercitazione è incentrata sulla creazione di un metodo HTML Helper estremamente semplice.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="4e9a6-182">Tenere presente che un HTML Helper può essere complesso desiderato.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="4e9a6-183">È possibile compilare l'helper HTML che eseguono il rendering di contenuto complesso, ad esempio le visualizzazioni albero, menu o tabelle di dati di database.</span><span class="sxs-lookup"><span data-stu-id="4e9a6-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4e9a6-184">[Precedente](asp-net-mvc-views-overview-vb.md)
> [Successivo](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4e9a6-184">[Previous](asp-net-mvc-views-overview-vb.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span></span>
