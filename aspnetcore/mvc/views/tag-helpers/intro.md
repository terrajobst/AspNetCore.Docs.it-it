---
title: Helper di tag in ASP.NET Core
author: rick-anderson
description: Informazioni su cosa sono gli helper di tag e sul loro utilizzo in ASP.NET Core.
ms.author: riande
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/intro
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3c198ccc3e3e2c11f3e2b9379bc63bd6428dbf69
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-tag-helpers-in-aspnet-core"></a><span data-ttu-id="8cdce-103">Introduzione agli helper di Tag in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8cdce-103">Introduction to Tag Helpers in ASP.NET Core</span></span> 

<span data-ttu-id="8cdce-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8cdce-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-are-tag-helpers"></a><span data-ttu-id="8cdce-105">Quali sono gli helper di Tag?</span><span class="sxs-lookup"><span data-stu-id="8cdce-105">What are Tag Helpers?</span></span>

<span data-ttu-id="8cdce-106">Gli helper di tag consentono al codice lato server partecipare alla creazione e il rendering di elementi HTML in file Razor.</span><span class="sxs-lookup"><span data-stu-id="8cdce-106">Tag Helpers enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="8cdce-107">Ad esempio, l'elemento predefinito `ImageTagHelper` possibile aggiungere un numero di versione per il nome dell'immagine.</span><span class="sxs-lookup"><span data-stu-id="8cdce-107">For example, the built-in `ImageTagHelper` can append a version number to the image name.</span></span> <span data-ttu-id="8cdce-108">Ogni volta che cambia l'immagine, il server genera una nuova versione univoca per l'immagine, in modo i client sono garantiti per ottenere l'immagine corrente (anziché un'immagine memorizzati nella cache non aggiornata).</span><span class="sxs-lookup"><span data-stu-id="8cdce-108">Whenever the image changes, the server generates a new unique version for the image, so clients are guaranteed to get the current image (instead of a stale cached image).</span></span> <span data-ttu-id="8cdce-109">Vi sono molti gli helper di Tag predefiniti per le attività comuni, ad esempio la creazione di moduli, collegamenti, asset durante il caricamento e più - e ancora più disponibili nel repository GitHub pubblici e come NuGet pacchetti.</span><span class="sxs-lookup"><span data-stu-id="8cdce-109">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="8cdce-110">Vengono creati gli helper di tag in c# e di destinazione gli elementi HTML in base al nome di elemento, il nome dell'attributo o tag padre.</span><span class="sxs-lookup"><span data-stu-id="8cdce-110">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="8cdce-111">Ad esempio, l'elemento predefinito `LabelTagHelper` possono avere come destinazione l'HTML `<label>` elemento quando il `LabelTagHelper` vengono applicati gli attributi.</span><span class="sxs-lookup"><span data-stu-id="8cdce-111">For example, the built-in `LabelTagHelper` can target the HTML `<label>` element when the `LabelTagHelper` attributes are applied.</span></span> <span data-ttu-id="8cdce-112">Se si ha familiarità con [helper HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), gli helper di Tag ridurre le esplicita transizioni tra HTML e c# nelle visualizzazioni Razor.</span><span class="sxs-lookup"><span data-stu-id="8cdce-112">If you're familiar with [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), Tag Helpers reduce the explicit transitions between HTML and C# in Razor views.</span></span> <span data-ttu-id="8cdce-113">In molti casi, l'helper HTML forniscono un'alternativa a un Helper Tag specifici, ma è importante tenere presente che gli helper di Tag non sostituisce l'helper HTML e non esiste un Helper di Tag per ogni HTML Helper.</span><span class="sxs-lookup"><span data-stu-id="8cdce-113">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers don't replace HTML Helpers and there's not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="8cdce-114">[Gli helper confrontati con l'helper HTML tag](#tag-helpers-compared-to-html-helpers) vengono illustrate le differenze nel modo più dettagliato.</span><span class="sxs-lookup"><span data-stu-id="8cdce-114">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span>

## <a name="what-tag-helpers-provide"></a><span data-ttu-id="8cdce-115">Cosa fornisce gli helper di Tag</span><span class="sxs-lookup"><span data-stu-id="8cdce-115">What Tag Helpers provide</span></span>

<span data-ttu-id="8cdce-116">**Un'esperienza di sviluppo orientato HTML** per nella maggior parte, markup Razor utilizzando l'helper di Tag è simile a HTML standard.</span><span class="sxs-lookup"><span data-stu-id="8cdce-116">**An HTML-friendly development experience** For the most part, Razor markup using Tag Helpers looks like standard HTML.</span></span> <span data-ttu-id="8cdce-117">Finestre di progettazione front-end familiarità con HTML/CSS/JavaScript è possibile modificare Razor senza conoscere la sintassi di c# Razor.</span><span class="sxs-lookup"><span data-stu-id="8cdce-117">Front-end designers conversant with HTML/CSS/JavaScript can edit Razor without learning C# Razor syntax.</span></span>

<span data-ttu-id="8cdce-118">**Un ambiente completo di IntelliSense per la creazione di markup HTML e Razor** in contrasto per l'helper HTML, l'approccio precedente per la creazione di markup in visualizzazioni Razor sul lato server.</span><span class="sxs-lookup"><span data-stu-id="8cdce-118">**A rich IntelliSense environment for creating HTML and Razor markup** This is in sharp contrast to HTML Helpers, the previous approach to server-side creation of markup in Razor views.</span></span> <span data-ttu-id="8cdce-119">[Gli helper confrontati con l'helper HTML tag](#tag-helpers-compared-to-html-helpers) vengono illustrate le differenze nel modo più dettagliato.</span><span class="sxs-lookup"><span data-stu-id="8cdce-119">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span> <span data-ttu-id="8cdce-120">[Il supporto IntelliSense per gli helper di Tag](#intellisense-support-for-tag-helpers) viene illustrato l'ambiente di IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="8cdce-120">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) explains the IntelliSense environment.</span></span> <span data-ttu-id="8cdce-121">Anche gli sviluppatori esperti con sintassi Razor c# sono più produttivi usando gli helper di Tag rispetto alla scrittura di codice c# Razor.</span><span class="sxs-lookup"><span data-stu-id="8cdce-121">Even developers experienced with Razor C# syntax are more productive using Tag Helpers than writing C# Razor markup.</span></span>

<span data-ttu-id="8cdce-122">**Un modo per verificare più produttivo e in grado di produrre più efficiente, affidabile e codice di facile manutenibilità con informazioni è disponibile solo nel server** , ad esempio, in passato è stato il motto sull'aggiornamento di immagini per modificare il nome dell'immagine quando si modifica l'immagine.</span><span class="sxs-lookup"><span data-stu-id="8cdce-122">**A way to make you more productive and able to produce more robust, reliable, and maintainable code using information only available on the server** For example, historically the mantra on updating images was to change the name of the image when you change the image.</span></span> <span data-ttu-id="8cdce-123">Le immagini devono essere in modo aggressivo memorizzati nella cache per motivi di prestazioni e, a meno che non si modifica il nome di un'immagine, si rischia di client di ottenere una copia non aggiornata.</span><span class="sxs-lookup"><span data-stu-id="8cdce-123">Images should be aggressively cached for performance reasons, and unless you change the name of an image, you risk clients getting a stale copy.</span></span> <span data-ttu-id="8cdce-124">In passato, dopo la modifica di un'immagine, il nome deve essere modificato e ogni riferimento a un'immagine nell'app web deve essere aggiornato.</span><span class="sxs-lookup"><span data-stu-id="8cdce-124">Historically, after an image was edited, the name had to be changed and each reference to the image in the web app needed to be updated.</span></span> <span data-ttu-id="8cdce-125">Non solo è molto lavoro che con utilizzo intensivo, è anche errori (potrebbe mancare un riferimento, accidentalmente immettere la stringa non corretto e così via.) L'elemento predefinito `ImageTagHelper` per questo scopo è automaticamente.</span><span class="sxs-lookup"><span data-stu-id="8cdce-125">Not only is this very labor intensive, it's also error prone (you could miss a reference, accidentally enter the wrong string, etc.) The built-in `ImageTagHelper` can do this for you automatically.</span></span> <span data-ttu-id="8cdce-126">Il `ImageTagHelper` possibile aggiungere una versione numero per il nome dell'immagine, pertanto ogni volta che cambia l'immagine, il server genera automaticamente una nuova versione univoca per l'immagine.</span><span class="sxs-lookup"><span data-stu-id="8cdce-126">The `ImageTagHelper` can append a version number to the image name, so whenever the image changes, the server automatically generates a new unique version for the image.</span></span> <span data-ttu-id="8cdce-127">I client sono garantiti per ottenere l'immagine corrente.</span><span class="sxs-lookup"><span data-stu-id="8cdce-127">Clients are guaranteed to get the current image.</span></span> <span data-ttu-id="8cdce-128">Il risparmio di affidabilità e manodopera proviene essenzialmente disponibile tramite il `ImageTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="8cdce-128">This robustness and labor savings comes essentially free by using the `ImageTagHelper`.</span></span>

<span data-ttu-id="8cdce-129">La maggior parte degli helper Tag predefiniti come destinazione gli elementi HTML esistenti e fornire attributi sul lato server per l'elemento.</span><span class="sxs-lookup"><span data-stu-id="8cdce-129">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span> <span data-ttu-id="8cdce-130">Ad esempio, il `<input>` elemento utilizzato in molte delle visualizzazioni del *viste/Account* cartella contiene il `asp-for` attributo, che estrae il nome della proprietà del modello specificato in codice HTML visualizzabile.</span><span class="sxs-lookup"><span data-stu-id="8cdce-130">For example, the `<input>` element used in many of the views in the *Views/Account* folder contains the `asp-for` attribute, which extracts the name of the specified model property into the rendered HTML.</span></span> <span data-ttu-id="8cdce-131">Il markup Razor seguente:</span><span class="sxs-lookup"><span data-stu-id="8cdce-131">The following Razor markup:</span></span>

```cshtml
<label asp-for="Email"></label>
```

<span data-ttu-id="8cdce-132">Genera il codice HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="8cdce-132">Generates the following HTML:</span></span>

```cshtml
<label for="Email">Email</label>
```

<span data-ttu-id="8cdce-133">Il `asp-for` attributo è reso disponibile per il `For` proprietà il `LabelTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="8cdce-133">The `asp-for` attribute is made available by the `For` property in the `LabelTagHelper`.</span></span> <span data-ttu-id="8cdce-134">Vedere [creazione gli helper di Tag](authoring.md) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="8cdce-134">See [Authoring Tag Helpers](authoring.md) for more information.</span></span>

## <a name="managing-tag-helper-scope"></a><span data-ttu-id="8cdce-135">La gestione di ambito Helper Tag</span><span class="sxs-lookup"><span data-stu-id="8cdce-135">Managing Tag Helper scope</span></span>

<span data-ttu-id="8cdce-136">Ambito di helper di tag è controllata da una combinazione di `@addTagHelper`, `@removeTagHelper`e "!" rinuncia carattere.</span><span class="sxs-lookup"><span data-stu-id="8cdce-136">Tag Helpers scope is controlled by a combination of `@addTagHelper`, `@removeTagHelper`, and the "!" opt-out character.</span></span>

<a name="add-helper-label"></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a><span data-ttu-id="8cdce-137">`@addTagHelper`rende disponibili gli helper di Tag</span><span class="sxs-lookup"><span data-stu-id="8cdce-137">`@addTagHelper` makes Tag Helpers available</span></span>

<span data-ttu-id="8cdce-138">Se si crea una nuova app web ASP.NET Core denominata *AuthoringTagHelpers* (con nessuna autenticazione), le operazioni seguenti *Views/_ViewImports.cshtml* file verrà aggiunto al progetto:</span><span class="sxs-lookup"><span data-stu-id="8cdce-138">If you create a new ASP.NET Core web app named *AuthoringTagHelpers* (with no authentication), the following *Views/_ViewImports.cshtml* file will be added to your project:</span></span>

[!code-cshtml[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

<span data-ttu-id="8cdce-139">Il `@addTagHelper` direttiva rende disponibile per la visualizzazione gli helper di Tag.</span><span class="sxs-lookup"><span data-stu-id="8cdce-139">The `@addTagHelper` directive makes Tag Helpers available to the view.</span></span> <span data-ttu-id="8cdce-140">In questo caso, il file della visualizzazione è *Views/_ViewImports.cshtml*, che per impostazione predefinita viene ereditata da tutti i file di visualizzazione di *viste* cartella e le sottodirectory; rendere disponibili gli helper di Tag.</span><span class="sxs-lookup"><span data-stu-id="8cdce-140">In this case, the view file is *Views/_ViewImports.cshtml*, which by default is inherited by all view files in the *Views* folder and sub-directories; making Tag Helpers available.</span></span> <span data-ttu-id="8cdce-141">Il codice precedente utilizza la sintassi con caratteri jolly ("\*") per specificare che tutti gli helper di Tag nell'assembly specificato (*Microsoft.AspNetCore.Mvc.TagHelpers*) sarà disponibile per tutti i file nella visualizzazione di *viste* directory o sottodirectory.</span><span class="sxs-lookup"><span data-stu-id="8cdce-141">The code above uses the wildcard syntax ("\*") to specify that all Tag Helpers in the specified assembly (*Microsoft.AspNetCore.Mvc.TagHelpers*) will be available to every view file in the *Views* directory or sub-directory.</span></span> <span data-ttu-id="8cdce-142">Il primo parametro dopo `@addTagHelper` specifica gli helper di Tag da caricare (usiamo "\*" per tutti gli helper di Tag), e il secondo parametro "Microsoft.AspNetCore.Mvc.TagHelpers" Specifica l'assembly contenente gli helper di Tag.</span><span class="sxs-lookup"><span data-stu-id="8cdce-142">The first parameter after `@addTagHelper` specifies the Tag Helpers to load (we are using "\*" for all Tag Helpers), and the second parameter "Microsoft.AspNetCore.Mvc.TagHelpers" specifies the assembly containing the Tag Helpers.</span></span> <span data-ttu-id="8cdce-143">*Microsoft.AspNetCore.Mvc.TagHelpers* è l'assembly per gli helper di Tag predefiniti di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8cdce-143">*Microsoft.AspNetCore.Mvc.TagHelpers* is the assembly for the built-in ASP.NET Core Tag Helpers.</span></span>

<span data-ttu-id="8cdce-144">Per esporre tutti gli helper di Tag in questo progetto (che consente di creare un assembly denominato *AuthoringTagHelpers*), è necessario utilizzare la seguente:</span><span class="sxs-lookup"><span data-stu-id="8cdce-144">To expose all of the Tag Helpers in this project (which creates an assembly named *AuthoringTagHelpers*), you would use the following:</span></span>

[!code-cshtml[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

<span data-ttu-id="8cdce-145">Se il progetto contiene un `EmailTagHelper` con spazio dei nomi predefinito (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), è possibile fornire il nome completo (FQN) dell'Helper di Tag:</span><span class="sxs-lookup"><span data-stu-id="8cdce-145">If your project contains an `EmailTagHelper` with the default namespace (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), you can provide the fully qualified name (FQN) of the Tag Helper:</span></span>

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<span data-ttu-id="8cdce-146">Per aggiungere un Helper Tag a una vista utilizzando un FQN, aggiungere innanzitutto i FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) e quindi il nome dell'assembly (*AuthoringTagHelpers*).</span><span class="sxs-lookup"><span data-stu-id="8cdce-146">To add a Tag Helper to a view using an FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="8cdce-147">La maggior parte degli sviluppatori preferiscono utilizzare il "\*" sintassi dei caratteri jolly.</span><span class="sxs-lookup"><span data-stu-id="8cdce-147">Most developers prefer to use the  "\*" wildcard syntax.</span></span> <span data-ttu-id="8cdce-148">La sintassi con caratteri jolly consente di inserire il carattere jolly "\*" come suffisso in un FQN.</span><span class="sxs-lookup"><span data-stu-id="8cdce-148">The wildcard syntax allows you to insert the wildcard character "\*" as the suffix in an FQN.</span></span> <span data-ttu-id="8cdce-149">Ad esempio, una delle seguenti direttive fornirà `EmailTagHelper`:</span><span class="sxs-lookup"><span data-stu-id="8cdce-149">For example, any of the following directives will bring in the `EmailTagHelper`:</span></span>

```cshtml
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

<span data-ttu-id="8cdce-150">Come indicato in precedenza, l'aggiunta di `@addTagHelper` direttiva per il *Views/_ViewImports.cshtml* file rende disponibili a tutti i file di visualizzazione dell'Helper di Tag di *viste* directory e sottodirectory.</span><span class="sxs-lookup"><span data-stu-id="8cdce-150">As mentioned previously, adding the `@addTagHelper` directive to the *Views/_ViewImports.cshtml* file makes the Tag Helper available to all view files in the *Views* directory and sub-directories.</span></span> <span data-ttu-id="8cdce-151">È possibile utilizzare il `@addTagHelper` direttiva nei file di visualizzazione specifico se si desidera registrarsi per esporre l'Helper di Tag solo tali viste.</span><span class="sxs-lookup"><span data-stu-id="8cdce-151">You can use the `@addTagHelper` directive in specific view files if you want to opt-in to exposing the Tag Helper to only those views.</span></span>

<a name="remove-razor-directives-label"></a>

### <a name="removetaghelper-removes-tag-helpers"></a><span data-ttu-id="8cdce-152">`@removeTagHelper`Rimuove gli helper di Tag</span><span class="sxs-lookup"><span data-stu-id="8cdce-152">`@removeTagHelper` removes Tag Helpers</span></span>

<span data-ttu-id="8cdce-153">Il `@removeTagHelper` ha gli stessi due parametri di `@addTagHelper`, e viene rimosso un Helper Tag che è stato aggiunto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="8cdce-153">The `@removeTagHelper` has the same two parameters as `@addTagHelper`, and it removes a Tag Helper that was previously added.</span></span> <span data-ttu-id="8cdce-154">Ad esempio, `@removeTagHelper` applicati per rimuovere una visualizzazione specifica l'Helper di Tag specificato dalla vista.</span><span class="sxs-lookup"><span data-stu-id="8cdce-154">For example, `@removeTagHelper` applied to a specific view removes the specified Tag Helper from the view.</span></span> <span data-ttu-id="8cdce-155">Utilizzando `@removeTagHelper` in un *Views/Folder/_ViewImports.cshtml* file rimuove l'Helper di Tag da tutte le viste in *cartella*.</span><span class="sxs-lookup"><span data-stu-id="8cdce-155">Using `@removeTagHelper` in a *Views/Folder/_ViewImports.cshtml* file removes the specified Tag Helper from all of the views in *Folder*.</span></span>

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a><span data-ttu-id="8cdce-156">Controllo dell'ambito di Helper di Tag con il *viewimports. cshtml* file</span><span class="sxs-lookup"><span data-stu-id="8cdce-156">Controlling Tag Helper scope with the *_ViewImports.cshtml* file</span></span>

<span data-ttu-id="8cdce-157">È possibile aggiungere un *viewimports. cshtml* per qualsiasi cartella di visualizzazione e la visualizzazione motore applica le direttive da tale file di entrambi e *Views/_ViewImports.cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="8cdce-157">You can add a *_ViewImports.cshtml* to any view folder, and the view engine applies the directives from both that file and the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="8cdce-158">Se è stato aggiunto un oggetto vuoto *Views/Home/_ViewImports.cshtml* file per il *Home* visualizzazioni, non vi sarà alcuna modifica perché il *viewimports. cshtml* file additivo.</span><span class="sxs-lookup"><span data-stu-id="8cdce-158">If you added an empty *Views/Home/_ViewImports.cshtml* file for the *Home* views, there would be no change because the *_ViewImports.cshtml* file is additive.</span></span> <span data-ttu-id="8cdce-159">Qualsiasi `@addTagHelper` aggiungere le direttive di *Views/Home/_ViewImports.cshtml* file (che non sono presenti il valore predefinito *Views/_ViewImports.cshtml* file) consente di esporre tali gli helper di Tag alle viste solo nel *Home* cartella.</span><span class="sxs-lookup"><span data-stu-id="8cdce-159">Any `@addTagHelper` directives you add to the *Views/Home/_ViewImports.cshtml* file (that are not in the default *Views/_ViewImports.cshtml* file) would expose those Tag Helpers to views only in the *Home* folder.</span></span>

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a><span data-ttu-id="8cdce-160">Rifiuto esplicito singoli elementi</span><span class="sxs-lookup"><span data-stu-id="8cdce-160">Opting out of individual elements</span></span>

<span data-ttu-id="8cdce-161">È possibile disabilitare un Helper Tag a livello di elemento con il carattere di rinuncia Helper di Tag ("!").</span><span class="sxs-lookup"><span data-stu-id="8cdce-161">You can disable a Tag Helper at the element level with the Tag Helper opt-out character ("!").</span></span> <span data-ttu-id="8cdce-162">Ad esempio, `Email` convalida è disabilitata nel `<span>` con il carattere di rinuncia Helper di Tag:</span><span class="sxs-lookup"><span data-stu-id="8cdce-162">For example, `Email` validation is disabled in the `<span>` with the Tag Helper opt-out character:</span></span>

```cshtml
<!span asp-validation-for="Email" class="text-danger"></!span>
```

<span data-ttu-id="8cdce-163">È necessario applicare il carattere di rinuncia Helper di Tag per il tag di apertura e chiusura.</span><span class="sxs-lookup"><span data-stu-id="8cdce-163">You must apply the Tag Helper opt-out character to the opening and closing tag.</span></span> <span data-ttu-id="8cdce-164">(Editor di Visual Studio aggiunge automaticamente il carattere di aderire al tag di chiusura quando si aggiunge uno al tag di apertura).</span><span class="sxs-lookup"><span data-stu-id="8cdce-164">(The Visual Studio editor automatically adds the opt-out character to the closing tag when you add one to the opening tag).</span></span> <span data-ttu-id="8cdce-165">Dopo aver aggiunto il carattere di rifiuto esplicito, l'elemento e gli attributi di Helper di Tag non sono più visualizzati in un unico tipo di carattere.</span><span class="sxs-lookup"><span data-stu-id="8cdce-165">After you add the opt-out character, the element and Tag Helper attributes are no longer displayed in a distinctive font.</span></span>

<a name="prefix-razor-directives-label"></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a><span data-ttu-id="8cdce-166">Utilizzando `@tagHelperPrefix` rendere esplicito l'utilizzo di Helper Tag</span><span class="sxs-lookup"><span data-stu-id="8cdce-166">Using `@tagHelperPrefix` to make Tag Helper usage explicit</span></span>

<span data-ttu-id="8cdce-167">Il `@tagHelperPrefix` direttiva consente di specificare una stringa di prefisso di tag per abilitare il supporto di Helper di Tag e rendere esplicito l'utilizzo di Helper di Tag.</span><span class="sxs-lookup"><span data-stu-id="8cdce-167">The `@tagHelperPrefix` directive allows you to specify a tag prefix string to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> <span data-ttu-id="8cdce-168">Ad esempio, è possibile aggiungere il markup seguente per il *Views/_ViewImports.cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="8cdce-168">For example, you could add the following markup to the *Views/_ViewImports.cshtml* file:</span></span>

```cshtml
@tagHelperPrefix th:
```
<span data-ttu-id="8cdce-169">Nell'immagine di codice seguente, il prefisso di Tag Helper è impostato su `th:`, in modo che solo gli elementi utilizzando il prefisso `th:` supportano gli helper di Tag (abilitato Helper di Tag di elementi di un carattere distintivo).</span><span class="sxs-lookup"><span data-stu-id="8cdce-169">In the code image below, the Tag Helper prefix is set to `th:`, so only those elements using the prefix `th:` support Tag Helpers (Tag Helper-enabled elements have a distinctive font).</span></span> <span data-ttu-id="8cdce-170">Il `<label>` e `<input>` elementi dispongono del prefisso dell'Helper di Tag e sono abilitati per il supporto di Tag, durante il `<span>` non elemento.</span><span class="sxs-lookup"><span data-stu-id="8cdce-170">The `<label>` and `<input>` elements have the Tag Helper prefix and are Tag Helper-enabled, while the `<span>` element doesn't.</span></span>

![immagine](intro/_static/thp.png)

<span data-ttu-id="8cdce-172">Le stesse regole di gerarchia che si applicano a `@addTagHelper` si applicano anche alle `@tagHelperPrefix`.</span><span class="sxs-lookup"><span data-stu-id="8cdce-172">The same hierarchy rules that apply to `@addTagHelper` also apply to `@tagHelperPrefix`.</span></span>

## <a name="intellisense-support-for-tag-helpers"></a><span data-ttu-id="8cdce-173">Supporto IntelliSense per gli helper di Tag</span><span class="sxs-lookup"><span data-stu-id="8cdce-173">IntelliSense support for Tag Helpers</span></span>

<span data-ttu-id="8cdce-174">Quando si crea una nuova app web ASP.NET in Visual Studio, aggiunge il pacchetto NuGet "Microsoft.AspNetCore.Razor.Tools".</span><span class="sxs-lookup"><span data-stu-id="8cdce-174">When you create a new ASP.NET web app in Visual Studio, it adds the NuGet package "Microsoft.AspNetCore.Razor.Tools".</span></span> <span data-ttu-id="8cdce-175">Questo è il pacchetto che aggiunge gli strumenti di supporto di Tag.</span><span class="sxs-lookup"><span data-stu-id="8cdce-175">This is the package that adds Tag Helper tooling.</span></span>

<span data-ttu-id="8cdce-176">Si consiglia di scrivere l'HTML `<label>` elemento.</span><span class="sxs-lookup"><span data-stu-id="8cdce-176">Consider writing an HTML `<label>` element.</span></span> <span data-ttu-id="8cdce-177">Non appena immette `<l` nell'editor di Visual Studio, IntelliSense visualizza gli elementi corrispondenti:</span><span class="sxs-lookup"><span data-stu-id="8cdce-177">As soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![immagine](intro/_static/label.png)

<span data-ttu-id="8cdce-179">Non solo ottenere della Guida HTML, ma l'icona (il "@" symbol with "<>" sotto di esso).</span><span class="sxs-lookup"><span data-stu-id="8cdce-179">Not only do you get HTML help, but the icon (the "@" symbol with "<>" under it).</span></span>

![immagine](intro/_static/tagSym.png)

<span data-ttu-id="8cdce-181">Identifica l'elemento come destinazione gli helper di Tag.</span><span class="sxs-lookup"><span data-stu-id="8cdce-181">identifies the element as targeted by Tag Helpers.</span></span> <span data-ttu-id="8cdce-182">Gli elementi HTML puri (ad esempio il `fieldset`) l'icona di "<>".</span><span class="sxs-lookup"><span data-stu-id="8cdce-182">Pure HTML elements (such as the `fieldset`) display the "<>" icon.</span></span>

<span data-ttu-id="8cdce-183">Un HTML puro `<label>` tag viene visualizzato il tag HTML (con il tema colori di Visual Studio predefinito) in un tipo di carattere marrone, gli attributi in rosso, e i valori di attributo in blu.</span><span class="sxs-lookup"><span data-stu-id="8cdce-183">A pure HTML `<label>` tag displays the HTML tag (with the default Visual Studio color theme) in a brown font, the attributes in red, and the attribute values in blue.</span></span>

![immagine](intro/_static/LableHtmlTag.png)

<span data-ttu-id="8cdce-185">Dopo avere immesso `<label`, IntelliSense Elenca gli attributi HTML/CSS disponibili e gli attributi di Helper di Tag di destinazione:</span><span class="sxs-lookup"><span data-stu-id="8cdce-185">After you enter `<label`, IntelliSense lists the available HTML/CSS attributes and the Tag Helper-targeted attributes:</span></span>

![immagine](intro/_static/labelattr.png)

<span data-ttu-id="8cdce-187">Completamento delle istruzioni IntelliSense consente di immettere il tasto tab per completare l'istruzione con il valore selezionato:</span><span class="sxs-lookup"><span data-stu-id="8cdce-187">IntelliSense statement completion allows you to enter the tab key to complete the statement with the selected value:</span></span>

![immagine](intro/_static/stmtcomplete.png)

<span data-ttu-id="8cdce-189">Non appena viene immesso un attributo Helper di Tag, modificare i tipi di carattere tag e attributi.</span><span class="sxs-lookup"><span data-stu-id="8cdce-189">As soon as a Tag Helper attribute is entered, the tag and attribute fonts change.</span></span> <span data-ttu-id="8cdce-190">Usa il tema colori "Light" o predefinito Visual Studio "Blue", il carattere è in grassetto viola.</span><span class="sxs-lookup"><span data-stu-id="8cdce-190">Using the default Visual Studio "Blue" or "Light" color theme, the font is bold purple.</span></span> <span data-ttu-id="8cdce-191">Se si usa il tema "Scuro" il carattere è in grassetto verde acqua.</span><span class="sxs-lookup"><span data-stu-id="8cdce-191">If you're using the "Dark" theme the font is bold teal.</span></span> <span data-ttu-id="8cdce-192">Le immagini in questo documento sono state eseguite utilizzando il tema predefinito.</span><span class="sxs-lookup"><span data-stu-id="8cdce-192">The images in this document were taken using the default theme.</span></span>

![immagine](intro/_static/labelaspfor2.png)

<span data-ttu-id="8cdce-194">È possibile immettere Visual Studio *CompleteWord* scelta rapida (Ctrl + barra spaziatrice è il [predefinito](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) all'interno delle virgolette doppie (""), ed è ora in c#, esattamente come sarebbe in una classe c#.</span><span class="sxs-lookup"><span data-stu-id="8cdce-194">You can enter the Visual Studio *CompleteWord* shortcut (Ctrl +spacebar is the [default](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) inside the double quotes (""), and you are now in C#, just like you would be in a C# class.</span></span> <span data-ttu-id="8cdce-195">IntelliSense consente di visualizzare tutti i metodi e proprietà sul modello di pagina.</span><span class="sxs-lookup"><span data-stu-id="8cdce-195">IntelliSense displays all the methods and properties on the page model.</span></span> <span data-ttu-id="8cdce-196">I metodi e proprietà sono disponibili perché il tipo della proprietà è `ModelExpression`.</span><span class="sxs-lookup"><span data-stu-id="8cdce-196">The methods and properties are available because the property type is `ModelExpression`.</span></span> <span data-ttu-id="8cdce-197">Nell'immagine seguente, si modifica il `Register` vista, pertanto la `RegisterViewModel` è disponibile.</span><span class="sxs-lookup"><span data-stu-id="8cdce-197">In the image below, I'm editing the `Register` view, so the `RegisterViewModel` is available.</span></span>

![immagine](intro/_static/intellemail.png)

<span data-ttu-id="8cdce-199">IntelliSense Elenca le proprietà e i metodi disponibili per il modello nella pagina.</span><span class="sxs-lookup"><span data-stu-id="8cdce-199">IntelliSense lists the properties and methods available to the model on the page.</span></span> <span data-ttu-id="8cdce-200">L'ambiente completo di IntelliSense consente di selezionare la classe CSS:</span><span class="sxs-lookup"><span data-stu-id="8cdce-200">The rich IntelliSense environment helps you select the CSS class:</span></span>

![immagine](intro/_static/iclass.png)

![immagine](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a><span data-ttu-id="8cdce-203">Helper di tag rispetto a helper HTML</span><span class="sxs-lookup"><span data-stu-id="8cdce-203">Tag Helpers compared to HTML Helpers</span></span>

<span data-ttu-id="8cdce-204">Collegare gli helper di tag agli elementi HTML in visualizzazioni Razor, mentre [helper HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) vengono chiamati come metodi frammisto HTML in visualizzazioni Razor.</span><span class="sxs-lookup"><span data-stu-id="8cdce-204">Tag Helpers attach to HTML elements in Razor views, while [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) are invoked as methods interspersed with HTML in Razor views.</span></span> <span data-ttu-id="8cdce-205">Si consideri il seguente markup Razor, che consente di creare un'etichetta HTML con la classe CSS "titolo":</span><span class="sxs-lookup"><span data-stu-id="8cdce-205">Consider the following Razor markup, which creates an HTML label with the CSS class "caption":</span></span>

```cshtml
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

<span data-ttu-id="8cdce-206">L'in (`@`) simbolo indica Razor, si tratta dell'inizio del codice.</span><span class="sxs-lookup"><span data-stu-id="8cdce-206">The at (`@`) symbol tells Razor this is the start of code.</span></span> <span data-ttu-id="8cdce-207">I due parametri successivi ("FirstName" e "nome:") sono stringhe, pertanto [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense) non consente di.</span><span class="sxs-lookup"><span data-stu-id="8cdce-207">The next two parameters ("FirstName" and "First Name:") are strings, so [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense) can't help.</span></span> <span data-ttu-id="8cdce-208">L'ultimo argomento:</span><span class="sxs-lookup"><span data-stu-id="8cdce-208">The last argument:</span></span>

```cshtml
new {@class="caption"}
```

<span data-ttu-id="8cdce-209">Un oggetto anonimo viene utilizzato per rappresentare gli attributi.</span><span class="sxs-lookup"><span data-stu-id="8cdce-209">Is an anonymous object used to represent attributes.</span></span> <span data-ttu-id="8cdce-210">Poiché **classe** è una parola chiave riservata in c#, utilizzare il `@` simbolo per forzare in c# per interpretare "@class=" come simbolo (nome della proprietà).</span><span class="sxs-lookup"><span data-stu-id="8cdce-210">Because **class** is a reserved keyword in C#, you use the `@` symbol to force C# to interpret "@class=" as a symbol (property name).</span></span> <span data-ttu-id="8cdce-211">Per una finestra di progettazione front-end (un utente hanno dimestichezza con HTML/CSS/JavaScript e altre tecnologie client, ma non hanno familiarità con c# e Razor), la maggior parte della riga è esterno.</span><span class="sxs-lookup"><span data-stu-id="8cdce-211">To a front-end designer (someone familiar with HTML/CSS/JavaScript and other client technologies but not familiar with C# and Razor), most of the line is foreign.</span></span> <span data-ttu-id="8cdce-212">L'intera riga deve essere creata con alcuna informazione della Guida da IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="8cdce-212">The entire line must be authored with no help from IntelliSense.</span></span>

<span data-ttu-id="8cdce-213">Utilizzo di `LabelTagHelper`, gli stessi tag possono essere scritti come:</span><span class="sxs-lookup"><span data-stu-id="8cdce-213">Using the `LabelTagHelper`, the same markup can be written as:</span></span>

![immagine](intro/_static/label2.png)

<span data-ttu-id="8cdce-215">Con la versione dell'Helper di Tag, non appena immette `<l` nell'editor di Visual Studio, IntelliSense visualizza gli elementi corrispondenti:</span><span class="sxs-lookup"><span data-stu-id="8cdce-215">With the Tag Helper version, as soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![immagine](intro/_static/label.png)

<span data-ttu-id="8cdce-217">IntelliSense consente di scrivere l'intera riga.</span><span class="sxs-lookup"><span data-stu-id="8cdce-217">IntelliSense helps you write the entire line.</span></span> <span data-ttu-id="8cdce-218">Il `LabelTagHelper` anche per impostazione predefinita il contenuto dell'impostazione di `asp-for` valore ("FirstName") dell'attributo "Nome"; Proprietà maiuscole/minuscole camel converte in una frase composta dal nome di proprietà con uno spazio in cui si verifica ogni nuova lettera maiuscola.</span><span class="sxs-lookup"><span data-stu-id="8cdce-218">The `LabelTagHelper` also defaults to setting the content of the `asp-for` attribute value ("FirstName") to "First Name"; It converts camel-cased properties to a sentence composed of the property name with a space where each new upper-case letter occurs.</span></span> <span data-ttu-id="8cdce-219">Nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="8cdce-219">In the following markup:</span></span>

![immagine](intro/_static/label2.png)

<span data-ttu-id="8cdce-221">Genera l'errore:</span><span class="sxs-lookup"><span data-stu-id="8cdce-221">generates:</span></span>

```cshtml
<label class="caption" for="FirstName">First Name</label>
```

<span data-ttu-id="8cdce-222">Le maiuscole/minuscole camel al contenuto in base alla frase non viene usata se si aggiunge contenuto per il `<label>`.</span><span class="sxs-lookup"><span data-stu-id="8cdce-222">The camel-cased to sentence-cased content isn't used if you add content to the `<label>`.</span></span> <span data-ttu-id="8cdce-223">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="8cdce-223">For example:</span></span>

![immagine](intro/_static/1stName.png)

<span data-ttu-id="8cdce-225">Genera l'errore:</span><span class="sxs-lookup"><span data-stu-id="8cdce-225">generates:</span></span>

```cshtml
<label class="caption" for="FirstName">Name First</label>
```

<span data-ttu-id="8cdce-226">L'immagine di codice seguente mostra la parte di *Views/Account/Register.cshtml* visualizzazione Razor generata dal legacy ASP.NET 4.5.x MVC modello incluso con Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="8cdce-226">The following code image shows the Form portion of the *Views/Account/Register.cshtml* Razor view generated from the legacy ASP.NET 4.5.x MVC template included with Visual Studio 2015.</span></span>

![immagine](intro/_static/regCS.png)

<span data-ttu-id="8cdce-228">Editor di Visual Studio consente di visualizzare il codice c# con uno sfondo grigio.</span><span class="sxs-lookup"><span data-stu-id="8cdce-228">The Visual Studio editor displays C# code with a grey background.</span></span> <span data-ttu-id="8cdce-229">Ad esempio, il `AntiForgeryToken` HTML Helper:</span><span class="sxs-lookup"><span data-stu-id="8cdce-229">For example, the `AntiForgeryToken` HTML Helper:</span></span>

```cshtml
@Html.AntiForgeryToken()
```

<span data-ttu-id="8cdce-230">viene visualizzato con uno sfondo grigio.</span><span class="sxs-lookup"><span data-stu-id="8cdce-230">is displayed with a grey background.</span></span> <span data-ttu-id="8cdce-231">La maggior parte del markup nella visualizzazione registro è c#.</span><span class="sxs-lookup"><span data-stu-id="8cdce-231">Most of the markup in the Register view is C#.</span></span> <span data-ttu-id="8cdce-232">Stabilire un confronto con l'approccio equivalente utilizzando l'helper di Tag:</span><span class="sxs-lookup"><span data-stu-id="8cdce-232">Compare that to the equivalent approach using Tag Helpers:</span></span>

![immagine](intro/_static/regTH.png)

<span data-ttu-id="8cdce-234">Il markup è molto più semplice e più facile da leggere, modificare e gestire rispetto a quella di helper HTML.</span><span class="sxs-lookup"><span data-stu-id="8cdce-234">The markup is much cleaner and easier to read, edit, and maintain than the HTML Helpers approach.</span></span> <span data-ttu-id="8cdce-235">Il codice c# è ridotto al minimo, che il server deve conoscere.</span><span class="sxs-lookup"><span data-stu-id="8cdce-235">The C# code is reduced to the minimum that the server needs to know about.</span></span> <span data-ttu-id="8cdce-236">L'editor di Visual Studio Visualizza markup di destinazione di un Helper di Tag in un unico tipo di carattere.</span><span class="sxs-lookup"><span data-stu-id="8cdce-236">The Visual Studio editor displays markup targeted by a Tag Helper in a distinctive font.</span></span>

<span data-ttu-id="8cdce-237">Si consideri il *posta elettronica* gruppo:</span><span class="sxs-lookup"><span data-stu-id="8cdce-237">Consider the *Email* group:</span></span>

[!code-csharp[Main](intro/sample/Register.cshtml?range=12-18)]

<span data-ttu-id="8cdce-238">Gli attributi "asp-" ha un valore di "Email" e "Email" non è una stringa.</span><span class="sxs-lookup"><span data-stu-id="8cdce-238">Each of the "asp-" attributes has a value of "Email", but "Email" isn't a string.</span></span> <span data-ttu-id="8cdce-239">In questo contesto, "Email" è la proprietà expression di modello c# per il `RegisterViewModel`.</span><span class="sxs-lookup"><span data-stu-id="8cdce-239">In this context, "Email" is the C# model expression property for the `RegisterViewModel`.</span></span>

<span data-ttu-id="8cdce-240">Editor di Visual Studio consente di scrivere **tutti** del markup nell'approccio Helper di Tag del modulo di registrazione, mentre Visual Studio non fornisce alcuna informazione della Guida per la maggior parte del codice nell'approccio helper HTML.</span><span class="sxs-lookup"><span data-stu-id="8cdce-240">The Visual Studio editor helps you write **all** of the markup in the Tag Helper approach of the register form, while Visual Studio provides no help for most of the code in the HTML Helpers approach.</span></span> <span data-ttu-id="8cdce-241">[Il supporto IntelliSense per gli helper di Tag](#intellisense-support-for-tag-helpers) entra dettagli sull'utilizzo di helper di Tag nell'editor di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8cdce-241">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) goes into detail on working with Tag Helpers in the Visual Studio editor.</span></span>

## <a name="tag-helpers-compared-to-web-server-controls"></a><span data-ttu-id="8cdce-242">Helper di tag rispetto ai controlli Server Web</span><span class="sxs-lookup"><span data-stu-id="8cdce-242">Tag Helpers compared to Web Server Controls</span></span>

* <span data-ttu-id="8cdce-243">Gli helper di tag non proprietari dell'elemento che sono associate; fanno parte semplicemente il rendering dell'elemento e il contenuto.</span><span class="sxs-lookup"><span data-stu-id="8cdce-243">Tag Helpers don't own the element they're associated with; they simply participate in the rendering of the element and content.</span></span> <span data-ttu-id="8cdce-244">ASP.NET [controlli Server Web](https://msdn.microsoft.com/library/7698y1f0.aspx) vengono dichiarati e richiamato in una pagina.</span><span class="sxs-lookup"><span data-stu-id="8cdce-244">ASP.NET [Web Server controls](https://msdn.microsoft.com/library/7698y1f0.aspx) are declared and invoked on a page.</span></span>

* <span data-ttu-id="8cdce-245">[Controlli Server Web](https://msdn.microsoft.com/library/zsyt68f1.aspx) hanno un ciclo di vita non semplice che può rendere difficile lo sviluppo e debug.</span><span class="sxs-lookup"><span data-stu-id="8cdce-245">[Web Server controls](https://msdn.microsoft.com/library/zsyt68f1.aspx) have a non-trivial lifecycle that can make developing and debugging difficult.</span></span>

* <span data-ttu-id="8cdce-246">I controlli Server Web consentono di aggiungere funzionalità per gli elementi del modello DOM (Document Object) client tramite un controllo client.</span><span class="sxs-lookup"><span data-stu-id="8cdce-246">Web Server controls allow you to add functionality to the client Document Object Model (DOM) elements by using a client control.</span></span> <span data-ttu-id="8cdce-247">Gli helper di tag non dispone di alcun DOM.</span><span class="sxs-lookup"><span data-stu-id="8cdce-247">Tag Helpers have no DOM.</span></span>

* <span data-ttu-id="8cdce-248">Controlli Server Web includono rilevamento automatico del browser.</span><span class="sxs-lookup"><span data-stu-id="8cdce-248">Web Server controls include automatic browser detection.</span></span> <span data-ttu-id="8cdce-249">Gli helper di tag non sono a conoscenza del browser.</span><span class="sxs-lookup"><span data-stu-id="8cdce-249">Tag Helpers have no knowledge of the browser.</span></span>

* <span data-ttu-id="8cdce-250">Gli helper di Tag più può fungere da sullo stesso elemento (vedere [conflitti evitando Helper Tag](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts) ) mentre è in genere non è possibile comporre i controlli Server Web.</span><span class="sxs-lookup"><span data-stu-id="8cdce-250">Multiple Tag Helpers can act on the same element (see [Avoiding Tag Helper conflicts](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts) ) while you typically can't compose Web Server controls.</span></span>

* <span data-ttu-id="8cdce-251">Gli helper di tag è possono modificare il tag e il contenuto degli elementi HTML che sta ambiti, ma non modificare direttamente qualsiasi altra in una pagina.</span><span class="sxs-lookup"><span data-stu-id="8cdce-251">Tag Helpers can modify the tag and content of HTML elements that they're scoped to, but don't directly modify anything else on a page.</span></span> <span data-ttu-id="8cdce-252">I controlli Server Web hanno un ambito specifico meno e possono eseguire azioni che influiscono su altre parti della pagina; abilitazione di effetti collaterali imprevisti.</span><span class="sxs-lookup"><span data-stu-id="8cdce-252">Web Server controls have a less specific scope and can perform actions that affect other parts of your page; enabling unintended side effects.</span></span>

* <span data-ttu-id="8cdce-253">Controlli Server Web utilizzano i convertitori di tipi per convertire le stringhe in oggetti.</span><span class="sxs-lookup"><span data-stu-id="8cdce-253">Web Server controls use type converters to convert strings into objects.</span></span> <span data-ttu-id="8cdce-254">Gli helper di Tag, si lavora in modo nativo in c#, pertanto non è necessario al tipo di conversione.</span><span class="sxs-lookup"><span data-stu-id="8cdce-254">With Tag Helpers, you work natively in C#, so you don't need to do type conversion.</span></span>

* <span data-ttu-id="8cdce-255">Utilizzare i controlli Server Web [System. ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel) per implementare il comportamento in fase di esecuzione e in fase di progettazione di componenti e controlli.</span><span class="sxs-lookup"><span data-stu-id="8cdce-255">Web Server controls use [System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel) to implement the run-time and design-time behavior of components and controls.</span></span> <span data-ttu-id="8cdce-256">`System.ComponentModel`include le classi base e interfacce per l'implementazione di convertitori di tipi e gli attributi, associazioni a origini dati e componenti della licenza.</span><span class="sxs-lookup"><span data-stu-id="8cdce-256">`System.ComponentModel` includes the base classes and interfaces for implementing attributes and type converters, binding to data sources, and licensing components.</span></span> <span data-ttu-id="8cdce-257">Invece che per gli helper di Tag, che in genere derivano da `TagHelper`e `TagHelper` classe di base espone due metodi, `Process` e `ProcessAsync`.</span><span class="sxs-lookup"><span data-stu-id="8cdce-257">Contrast that to Tag Helpers, which typically derive from `TagHelper`, and the `TagHelper` base class exposes only two methods, `Process` and `ProcessAsync`.</span></span>

## <a name="customizing-the-tag-helper-element-font"></a><span data-ttu-id="8cdce-258">Il tipo di carattere elemento Helper di Tag di personalizzazione</span><span class="sxs-lookup"><span data-stu-id="8cdce-258">Customizing the Tag Helper element font</span></span>

<span data-ttu-id="8cdce-259">È possibile personalizzare il tipo di carattere e la colorazione da **strumenti** > **opzioni** > **ambiente** > **tipi di carattere e i colori**:</span><span class="sxs-lookup"><span data-stu-id="8cdce-259">You can customize the font and colorization from **Tools** > **Options** > **Environment** > **Fonts and Colors**:</span></span>

![immagine](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a><span data-ttu-id="8cdce-261">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="8cdce-261">Additional Resources</span></span>

* [<span data-ttu-id="8cdce-262">Creazione e modifica di helper tag</span><span class="sxs-lookup"><span data-stu-id="8cdce-262">Authoring Tag Helpers</span></span>](authoring.md)
* [<span data-ttu-id="8cdce-263">Utilizzo dei moduli</span><span class="sxs-lookup"><span data-stu-id="8cdce-263">Working with Forms </span></span>](../working-with-forms.md)
* <span data-ttu-id="8cdce-264">[TagHelperSamples su GitHub](https://github.com/dpaquette/TagHelperSamples) sono inclusi esempi di Helper di Tag per l'utilizzo di [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="8cdce-264">[TagHelperSamples on GitHub](https://github.com/dpaquette/TagHelperSamples) contains Tag Helper samples for working with [Bootstrap](http://getbootstrap.com/).</span></span>

<!--
* [Working with Forms ](xref:mvc/views/working-with-forms)
-->
