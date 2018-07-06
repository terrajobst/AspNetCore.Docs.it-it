---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Creazione di helper HTML personalizzati (c#) | Microsoft Docs
author: microsoft
description: L'obiettivo di questa esercitazione è illustrare come è possibile creare helper HTML personalizzati che è possibile usare all'interno delle visualizzazioni MVC. Grazie all'uso di HTML Helper...
ms.author: aspnetcontent
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 0606ec3b5595fbe73918b82e32b393871e8533a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839409"
---
<a name="creating-custom-html-helpers-c"></a><span data-ttu-id="30f10-104">Creazione di helper HTML personalizzati (c#)</span><span class="sxs-lookup"><span data-stu-id="30f10-104">Creating Custom HTML Helpers (C#)</span></span>
====================
<span data-ttu-id="30f10-105">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="30f10-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="30f10-106">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="30f10-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> <span data-ttu-id="30f10-107">L'obiettivo di questa esercitazione è illustrare come è possibile creare helper HTML personalizzati che è possibile usare all'interno delle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="30f10-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="30f10-108">Grazie all'uso di helper HTML, è possibile ridurre la quantità di noioso digitazione dei tag HTML che è necessario eseguire per creare una pagina HTML standard.</span><span class="sxs-lookup"><span data-stu-id="30f10-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>


<span data-ttu-id="30f10-109">L'obiettivo di questa esercitazione è illustrare come è possibile creare helper HTML personalizzati che è possibile usare all'interno delle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="30f10-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="30f10-110">Grazie all'uso di helper HTML, è possibile ridurre la quantità di noioso digitazione dei tag HTML che è necessario eseguire per creare una pagina HTML standard.</span><span class="sxs-lookup"><span data-stu-id="30f10-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="30f10-111">Nella prima parte di questa esercitazione, descriverò alcuni degli helper HTML esistenti incluse con il framework ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="30f10-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="30f10-112">Successivamente, descrivono due metodi di creazione di helper HTML personalizzati: spiegherò come creare helper HTML personalizzati mediante la creazione di un metodo statico e la creazione di un metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="30f10-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a static method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="30f10-113">Comprendere gli helper HTML</span><span class="sxs-lookup"><span data-stu-id="30f10-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="30f10-114">Un HTML Helper è semplicemente un metodo che restituisce una stringa.</span><span class="sxs-lookup"><span data-stu-id="30f10-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="30f10-115">La stringa può rappresentare qualsiasi tipo di contenuto che desidera.</span><span class="sxs-lookup"><span data-stu-id="30f10-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="30f10-116">Ad esempio, è possibile utilizzare gli helper HTML per eseguire il rendering di tag HTML standard come HTML `<input>` e `<img>` tag.</span><span class="sxs-lookup"><span data-stu-id="30f10-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="30f10-117">È inoltre possibile utilizzare gli helper HTML per il rendering del contenuto più complesso, ad esempio un elenco di schede o una tabella HTML di dati del database.</span><span class="sxs-lookup"><span data-stu-id="30f10-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="30f10-118">Il framework ASP.NET MVC include il set seguente di helper HTML standard (non un elenco completo):</span><span class="sxs-lookup"><span data-stu-id="30f10-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="30f10-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="30f10-119">Html.ActionLink()</span></span>
- <span data-ttu-id="30f10-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="30f10-120">Html.BeginForm()</span></span>
- <span data-ttu-id="30f10-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="30f10-121">Html.CheckBox()</span></span>
- <span data-ttu-id="30f10-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="30f10-122">Html.DropDownList()</span></span>
- <span data-ttu-id="30f10-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="30f10-123">Html.EndForm()</span></span>
- <span data-ttu-id="30f10-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="30f10-124">Html.Hidden()</span></span>
- <span data-ttu-id="30f10-125">Html.ListBox()</span><span class="sxs-lookup"><span data-stu-id="30f10-125">Html.ListBox()</span></span>
- <span data-ttu-id="30f10-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="30f10-126">Html.Password()</span></span>
- <span data-ttu-id="30f10-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="30f10-127">Html.RadioButton()</span></span>
- <span data-ttu-id="30f10-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="30f10-128">Html.TextArea()</span></span>
- <span data-ttu-id="30f10-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="30f10-129">Html.TextBox()</span></span>

<span data-ttu-id="30f10-130">Si consideri, ad esempio, il modulo nel listato 1.</span><span class="sxs-lookup"><span data-stu-id="30f10-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="30f10-131">Questo modulo viene eseguito il rendering con l'aiuto di due degli helper HTML standard (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="30f10-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="30f10-132">Questo modulo Usa la `Html.BeginForm()` e `Html.TextBox()` metodi Helper per eseguire il rendering di un semplice form HTML.</span><span class="sxs-lookup"><span data-stu-id="30f10-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods to render a simple HTML form.</span></span>


<span data-ttu-id="30f10-133">[![Pagina sottoposta a rendering con gli helper HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="30f10-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="30f10-134">**Figura 01**: pagina sottoposta a rendering con gli helper HTML ([fare clic per visualizzare l'immagine con dimensioni normali](creating-custom-html-helpers-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="30f10-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image3.png))</span></span>


<span data-ttu-id="30f10-135">**Listato 1: `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="30f10-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

<span data-ttu-id="30f10-136">Il metodo Helper Html.BeginForm() viene utilizzato per creare il codice HTML di apertura e chiusura `<form>` tag.</span><span class="sxs-lookup"><span data-stu-id="30f10-136">The Html.BeginForm() Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="30f10-137">Si noti che il `Html.BeginForm()` viene chiamato all'interno di un utilizzo istruzione.</span><span class="sxs-lookup"><span data-stu-id="30f10-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="30f10-138">L'istruzione using garantisce che il `<form>` tag viene chiuso al termine del tramite blocco.</span><span class="sxs-lookup"><span data-stu-id="30f10-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="30f10-139">Se si preferisce, invece di crearne un'usando blocco, è possibile chiamare il metodo Helper Html.EndForm() per chiudere il `<form>` tag.</span><span class="sxs-lookup"><span data-stu-id="30f10-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="30f10-140">Usare qualsiasi approccio alla creazione di apertura e chiusura `<form>` tag che sembra più intuitivo per l'utente.</span><span class="sxs-lookup"><span data-stu-id="30f10-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="30f10-141">Il `Html.TextBox()` metodi di supporto vengono usati nel listato 1 per il rendering HTML `<input>` tag.</span><span class="sxs-lookup"><span data-stu-id="30f10-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="30f10-142">Se si seleziona HTML nel browser viene visualizzato l'origine HTML nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="30f10-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="30f10-143">Si noti che l'origine contiene tag HTML standard.</span><span class="sxs-lookup"><span data-stu-id="30f10-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="30f10-144">Si noti che il `Html.TextBox()`-HTML Helper viene eseguito il rendering con `<%= %>` tag anziché `<% %>` tag.</span><span class="sxs-lookup"><span data-stu-id="30f10-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="30f10-145">Se non si include il segno di uguale, quindi non viene sottoposto a rendering nel browser.</span><span class="sxs-lookup"><span data-stu-id="30f10-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="30f10-146">Il framework ASP.NET MVC contiene un piccolo set di supporti.</span><span class="sxs-lookup"><span data-stu-id="30f10-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="30f10-147">È molto probabilmente, sarà necessario estendere il framework MVC con gli helper HTML personalizzati.</span><span class="sxs-lookup"><span data-stu-id="30f10-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="30f10-148">La parte restante di questa esercitazione, si descrive due metodi di creazione di helper HTML personalizzati.</span><span class="sxs-lookup"><span data-stu-id="30f10-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="30f10-149">**Listato 2: `Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="30f10-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a><span data-ttu-id="30f10-150">Creazione di helper HTML con metodi statici</span><span class="sxs-lookup"><span data-stu-id="30f10-150">Creating HTML Helpers with Static Methods</span></span>

<span data-ttu-id="30f10-151">Il modo più semplice per creare un nuovo HTML Helper consiste nel creare un metodo statico che restituisce una stringa.</span><span class="sxs-lookup"><span data-stu-id="30f10-151">The easiest way to create a new HTML Helper is to create a static method that returns a string.</span></span> <span data-ttu-id="30f10-152">Si supponga, ad esempio, che si decide di creare un nuovo HTML Helper che esegue il rendering HTML `<label>` tag.</span><span class="sxs-lookup"><span data-stu-id="30f10-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="30f10-153">È possibile usare la classe nel listato 2 per eseguire il rendering di un `<label>` .</span><span class="sxs-lookup"><span data-stu-id="30f10-153">You can use the class in Listing 2 to render a `<label>` .</span></span>

<span data-ttu-id="30f10-154">**Listato 2: `Helpers\LabelHelper.cs`**</span><span class="sxs-lookup"><span data-stu-id="30f10-154">**Listing 2 – `Helpers\LabelHelper.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

<span data-ttu-id="30f10-155">Non ci sono particolari sulla classe nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="30f10-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="30f10-156">Il `Label()` metodo semplicemente restituisce una stringa.</span><span class="sxs-lookup"><span data-stu-id="30f10-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="30f10-157">La visualizzazione dell'indice modificata nel listato 3 Usa il `LabelHelper` per eseguire il rendering HTML `<label>` tag.</span><span class="sxs-lookup"><span data-stu-id="30f10-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="30f10-158">Si noti che la visualizzazione include un' `<%@ imports %>` direttiva che importa il `Application1.Helpers` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="30f10-158">Notice that the view includes an `<%@ imports %>` directive that imports the `Application1.Helpers` namespace.</span></span>

<span data-ttu-id="30f10-159">**Listato 2: `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="30f10-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="30f10-160">Creazione di helper HTML con i metodi di estensione</span><span class="sxs-lookup"><span data-stu-id="30f10-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="30f10-161">Se si desidera creare gli helper HTML che funzionano solo come gli helper HTML standard incluso nel framework di MVC ASP.NET necessario creare i metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="30f10-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="30f10-162">I metodi di estensione consentono di aggiungere nuovi metodi a una classe esistente.</span><span class="sxs-lookup"><span data-stu-id="30f10-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="30f10-163">Quando si crea un metodo HTML Helper, aggiungere nuovi metodi alla classe HtmlHelper rappresentata dalla proprietà Html della visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="30f10-163">When creating an HTML Helper method, you add new methods to the HtmlHelper class represented by a view's Html property.</span></span>

<span data-ttu-id="30f10-164">La classe nel listato 3 viene aggiunto un metodo di estensione per il `HtmlHelper` classe denominata `Label()`.</span><span class="sxs-lookup"><span data-stu-id="30f10-164">The class in Listing 3 adds an extension method to the `HtmlHelper` class named `Label()`.</span></span> <span data-ttu-id="30f10-165">Esistono un paio di aspetti che è possibile notare su questa classe.</span><span class="sxs-lookup"><span data-stu-id="30f10-165">There are a couple of things that you should notice about this class.</span></span> <span data-ttu-id="30f10-166">In primo luogo, si noti che la classe è una classe statica.</span><span class="sxs-lookup"><span data-stu-id="30f10-166">First, notice that the class is a static class.</span></span> <span data-ttu-id="30f10-167">È necessario definire un metodo di estensione con una classe statica.</span><span class="sxs-lookup"><span data-stu-id="30f10-167">You must define an extension method with a static class.</span></span>

<span data-ttu-id="30f10-168">In secondo luogo, si noti che il primo parametro del `Label()` metodo è preceduto dalla parola chiave `this`.</span><span class="sxs-lookup"><span data-stu-id="30f10-168">Second, notice that the first parameter of the `Label()` method is preceded by the keyword `this`.</span></span> <span data-ttu-id="30f10-169">Il primo parametro di un metodo di estensione indica la classe che estende il metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="30f10-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="30f10-170">**Listato 3: `Helpers\LabelExtensions.cs`**</span><span class="sxs-lookup"><span data-stu-id="30f10-170">**Listing 3 – `Helpers\LabelExtensions.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

<span data-ttu-id="30f10-171">Dopo aver creato un metodo di estensione e compilare correttamente l'applicazione, il metodo di estensione viene visualizzata in Intellisense di Visual Studio, ad esempio tutti gli altri metodi di una classe (vedere la figura 2).</span><span class="sxs-lookup"><span data-stu-id="30f10-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="30f10-172">L'unica differenza è che estensione vengono visualizzati i metodi con un simbolo speciale accanto agli (un'icona di freccia verso il basso).</span><span class="sxs-lookup"><span data-stu-id="30f10-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>


<span data-ttu-id="30f10-173">[![Usando il metodo di estensione Html.Label()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="30f10-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span></span>

<span data-ttu-id="30f10-174">**Figura 02**: tramite il metodo di estensione Html.Label() ([fare clic per visualizzare l'immagine con dimensioni normali](creating-custom-html-helpers-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="30f10-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image6.png))</span></span>


<span data-ttu-id="30f10-175">La visualizzazione dell'indice modificata nel listato 4 utilizza il metodo di estensione Html.Label() per eseguire il rendering di tutti i relativi `<label>` tag.</span><span class="sxs-lookup"><span data-stu-id="30f10-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its `<label>` tags.</span></span>

<span data-ttu-id="30f10-176">**Listato 4: `Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="30f10-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="30f10-177">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="30f10-177">Summary</span></span>

<span data-ttu-id="30f10-178">In questa esercitazione si è appreso due metodi di creazione di helper HTML personalizzati.</span><span class="sxs-lookup"><span data-stu-id="30f10-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="30f10-179">In primo luogo, si è appreso come creare un oggetto personalizzato `Label()` HTML Helper mediante la creazione di un metodo statico che restituisce una stringa.</span><span class="sxs-lookup"><span data-stu-id="30f10-179">First, you learned how to create a custom `Label()` HTML Helper by creating a static method that returns a string.</span></span> <span data-ttu-id="30f10-180">Successivamente, si è appreso come creare una classe personalizzata `Label()` metodo di HTML Helper mediante la creazione di un metodo di estensione sul `HtmlHelper` classe.</span><span class="sxs-lookup"><span data-stu-id="30f10-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="30f10-181">In questa esercitazione, mi sono concentrato sulla creazione di un metodo HTML Helper estremamente semplice.</span><span class="sxs-lookup"><span data-stu-id="30f10-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="30f10-182">Tenere presente che un HTML Helper può essere complesso desiderato.</span><span class="sxs-lookup"><span data-stu-id="30f10-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="30f10-183">È possibile compilare gli helper HTML che eseguono il rendering di contenuto avanzato, ad esempio visualizzazioni dell'albero, i menu o tabelle di dati del database.</span><span class="sxs-lookup"><span data-stu-id="30f10-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="30f10-184">[Precedente](asp-net-mvc-views-overview-cs.md)
> [Successivo](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span><span class="sxs-lookup"><span data-stu-id="30f10-184">[Previous](asp-net-mvc-views-overview-cs.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span></span>
