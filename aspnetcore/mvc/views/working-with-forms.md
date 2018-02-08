---
title: Helper tag nei moduli in ASP.NET Core
author: rick-anderson
description: Descrive gli helper tag predefiniti usati con i moduli.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/working-with-forms
ms.openlocfilehash: 805c2ba5b3a9669d5547e1c595883436eea0d11a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-using-tag-helpers-in-forms-in-aspnet-core"></a><span data-ttu-id="ed65f-103">Introduzione all'uso di helper tag nei moduli in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ed65f-103">Introduction to using tag helpers in forms in ASP.NET Core</span></span>

<span data-ttu-id="ed65f-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette) e [Jerrie Pelser](https://github.com/jerriep)</span><span class="sxs-lookup"><span data-stu-id="ed65f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), and [Jerrie Pelser](https://github.com/jerriep)</span></span>

<span data-ttu-id="ed65f-105">Questo documento illustra l'uso di helper tag Form e gli elementi HTML comunemente usati all'interno di questi.</span><span class="sxs-lookup"><span data-stu-id="ed65f-105">This document demonstrates working with Forms and the HTML elements commonly used on a Form.</span></span> <span data-ttu-id="ed65f-106">L'elemento [Form](https://www.w3.org/TR/html401/interact/forms.html) del linguaggio HTML rappresenta il meccanismo principale usato dalle app Web per eseguire il postback di dati nel server.</span><span class="sxs-lookup"><span data-stu-id="ed65f-106">The HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) element provides the primary mechanism web apps use to post back data to the server.</span></span> <span data-ttu-id="ed65f-107">La maggior parte di questo documento descrive gli [helper tag](tag-helpers/intro.md) e spiega come questi consentono di creare moduli HTML solidi in modo produttivo.</span><span class="sxs-lookup"><span data-stu-id="ed65f-107">Most of this document describes [Tag Helpers](tag-helpers/intro.md) and how they can help you productively create robust HTML forms.</span></span> <span data-ttu-id="ed65f-108">Prima di leggere questo documento, è consigliabile leggere [Introduzione agli helper tag](tag-helpers/intro.md).</span><span class="sxs-lookup"><span data-stu-id="ed65f-108">We recommend you read [Introduction to Tag Helpers](tag-helpers/intro.md) before you read this document.</span></span>

<span data-ttu-id="ed65f-109">In molti casi gli helper HTML offrono un approccio alternativo a un helper tag specifico, ma è importante tenere presente che gli helper tag non sostituiscono gli helper HTML e che non esiste un helper tag per ogni helper HTML.</span><span class="sxs-lookup"><span data-stu-id="ed65f-109">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers don't replace HTML Helpers and there's not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="ed65f-110">Se un'alternativa sotto forma di helper HTML esiste, viene citata.</span><span class="sxs-lookup"><span data-stu-id="ed65f-110">When an HTML Helper alternative exists, it's mentioned.</span></span>

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a><span data-ttu-id="ed65f-111">Helper tag Form</span><span class="sxs-lookup"><span data-stu-id="ed65f-111">The Form Tag Helper</span></span>

<span data-ttu-id="ed65f-112">L'helper tag [Form](https://www.w3.org/TR/html401/interact/forms.html):</span><span class="sxs-lookup"><span data-stu-id="ed65f-112">The [Form](https://www.w3.org/TR/html401/interact/forms.html) Tag Helper:</span></span>

* <span data-ttu-id="ed65f-113">Genera il valore dell'attributo `action` di [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) HTML per un'azione del controller MVC o una route denominata</span><span class="sxs-lookup"><span data-stu-id="ed65f-113">Generates the HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) `action` attribute value for a MVC controller action or named route</span></span>

* <span data-ttu-id="ed65f-114">Genera un [token di verifica della richiesta](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) nascosto per impedire richieste intersito false, in caso di uso con l'attributo `[ValidateAntiForgeryToken]` nel metodo azione HTTP Post</span><span class="sxs-lookup"><span data-stu-id="ed65f-114">Generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method)</span></span>

* <span data-ttu-id="ed65f-115">Fornisce l'attributo `asp-route-<Parameter Name>`, dove `<Parameter Name>` viene aggiunto ai valori della route.</span><span class="sxs-lookup"><span data-stu-id="ed65f-115">Provides the `asp-route-<Parameter Name>` attribute, where `<Parameter Name>` is added to the route values.</span></span> <span data-ttu-id="ed65f-116">I parametri `routeValues` in `Html.BeginForm` e `Html.BeginRouteForm` forniscono una funzionalità simile.</span><span class="sxs-lookup"><span data-stu-id="ed65f-116">The  `routeValues` parameters to `Html.BeginForm` and `Html.BeginRouteForm` provide similar functionality.</span></span>

* <span data-ttu-id="ed65f-117">Ha come helper HTML alternativi `Html.BeginForm` e `Html.BeginRouteForm`</span><span class="sxs-lookup"><span data-stu-id="ed65f-117">Has an HTML Helper alternative `Html.BeginForm` and `Html.BeginRouteForm`</span></span>

<span data-ttu-id="ed65f-118">Esempio:</span><span class="sxs-lookup"><span data-stu-id="ed65f-118">Sample:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

<span data-ttu-id="ed65f-119">L'helper tag Form precedente genera il codice HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="ed65f-119">The Form Tag Helper above generates the following HTML:</span></span>

```HTML
<form method="post" action="/Demo/Register">
     <!-- Input and Submit elements -->
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

<span data-ttu-id="ed65f-120">Il runtime MVC genera il valore dell'attributo `action` dagli attributi `asp-controller` e `asp-action` dell'helper tag Form.</span><span class="sxs-lookup"><span data-stu-id="ed65f-120">The MVC runtime generates the `action` attribute value from the Form Tag Helper attributes `asp-controller` and `asp-action`.</span></span> <span data-ttu-id="ed65f-121">L'helper tag Form genera anche un [token di verifica della richiesta](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) nascosto per impedire richieste intersito false, in caso di uso con l'attributo `[ValidateAntiForgeryToken]` nel metodo azione HTTP Post.</span><span class="sxs-lookup"><span data-stu-id="ed65f-121">The Form Tag Helper also generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method).</span></span> <span data-ttu-id="ed65f-122">La protezione di un Form HTML puro da richieste intersito false è difficile, e l'helper tag Form offre questo servizio.</span><span class="sxs-lookup"><span data-stu-id="ed65f-122">Protecting a pure HTML Form from cross-site request forgery is difficult, the Form Tag Helper provides this service for you.</span></span>

### <a name="using-a-named-route"></a><span data-ttu-id="ed65f-123">Uso di una route denominata</span><span class="sxs-lookup"><span data-stu-id="ed65f-123">Using a named route</span></span>

<span data-ttu-id="ed65f-124">L'attributo `asp-route` degli helper tag può anche generare un markup per l'attributo `action` HTML.</span><span class="sxs-lookup"><span data-stu-id="ed65f-124">The `asp-route` Tag Helper attribute can also generate markup for the HTML `action` attribute.</span></span> <span data-ttu-id="ed65f-125">Un'app con una [route](../../fundamentals/routing.md) denominata `register` può usare il markup seguente per la pagina di registrazione:</span><span class="sxs-lookup"><span data-stu-id="ed65f-125">An app with a [route](../../fundamentals/routing.md)  named `register` could use the following markup for the registration page:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

<span data-ttu-id="ed65f-126">Molte delle visualizzazioni nella cartella *Views/Account* (generata quando si crea una nuova app Web con *account utente singoli*) contengono l'attributo [asp-route-returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms):</span><span class="sxs-lookup"><span data-stu-id="ed65f-126">Many of the views in the *Views/Account* folder (generated when you create a new web app with *Individual User Accounts*) contain the [asp-route-returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) attribute:</span></span>

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
><span data-ttu-id="ed65f-127">Con i modelli predefiniti, `returnUrl` viene popolato automaticamente solo quando si tenta di accedere a una risorsa autorizzata senza aver effettuato l'autenticazione o l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="ed65f-127">With the built in templates, `returnUrl` is only populated automatically when you try to access an authorized resource but are not authenticated or authorized.</span></span> <span data-ttu-id="ed65f-128">Se l'utente tenta un accesso non autorizzato, il middleware di sicurezza lo reindirizza alla pagina di accesso con `returnUrl` impostato.</span><span class="sxs-lookup"><span data-stu-id="ed65f-128">When you attempt an unauthorized access, the security middleware redirects you to the login page with the `returnUrl` set.</span></span>

## <a name="the-input-tag-helper"></a><span data-ttu-id="ed65f-129">Helper tag Input</span><span class="sxs-lookup"><span data-stu-id="ed65f-129">The Input Tag Helper</span></span>

<span data-ttu-id="ed65f-130">L'helper tag Input associa un elemento HTML [ \<input>](https://www.w3.org/wiki/HTML/Elements/input) a un'espressione di modello nella visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="ed65f-130">The Input Tag Helper binds an HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) element to a model expression in your razor view.</span></span>

<span data-ttu-id="ed65f-131">Sintassi:</span><span class="sxs-lookup"><span data-stu-id="ed65f-131">Syntax:</span></span>

```HTML
<input asp-for="<Expression Name>" />
```

<span data-ttu-id="ed65f-132">L'helper tag Input:</span><span class="sxs-lookup"><span data-stu-id="ed65f-132">The Input Tag Helper:</span></span>

* <span data-ttu-id="ed65f-133">Genera gli attributi HTML `id` e `name` per il nome dell'espressione specificata nell'attributo `asp-for`.</span><span class="sxs-lookup"><span data-stu-id="ed65f-133">Generates the `id` and `name` HTML attributes for the expression name specified in the `asp-for` attribute.</span></span> <span data-ttu-id="ed65f-134">`asp-for="Property1.Property2"` è equivalente a `m => m.Property1.Property2`.</span><span class="sxs-lookup"><span data-stu-id="ed65f-134">`asp-for="Property1.Property2"` is equivalent to `m => m.Property1.Property2`.</span></span> <span data-ttu-id="ed65f-135">Il nome dell'espressione viene usato come valore dell'attributo `asp-for`.</span><span class="sxs-lookup"><span data-stu-id="ed65f-135">The name of the expression is what is used for the `asp-for` attribute value.</span></span> <span data-ttu-id="ed65f-136">Per altre informazioni, vedere la sezione [Nomi delle espressioni](#expression-names).</span><span class="sxs-lookup"><span data-stu-id="ed65f-136">See the [Expression names](#expression-names) section for additional information.</span></span>

* <span data-ttu-id="ed65f-137">Imposta il valore dell'attributo HTML `type` in base agli attributi relativi al tipo di modello e all'[annotazione dei dati](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) applicati alla proprietà del modello</span><span class="sxs-lookup"><span data-stu-id="ed65f-137">Sets the HTML `type` attribute value based on the model type and  [data annotation](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to the model property</span></span>

* <span data-ttu-id="ed65f-138">Non sovrascrive il valore dell'attributo HTML `type` se è già stato specificato</span><span class="sxs-lookup"><span data-stu-id="ed65f-138">Won't overwrite the HTML `type` attribute value when one is specified</span></span>

* <span data-ttu-id="ed65f-139">Genera attributi di convalida [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) da attributi di [annotazione dei dati](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) applicati alla proprietà del modello</span><span class="sxs-lookup"><span data-stu-id="ed65f-139">Generates [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  validation attributes from [data annotation](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to model properties</span></span>

* <span data-ttu-id="ed65f-140">Sovrappone una funzionalità di helper HTML con `Html.TextBoxFor` e `Html.EditorFor`.</span><span class="sxs-lookup"><span data-stu-id="ed65f-140">Has an HTML Helper feature overlap with `Html.TextBoxFor` and `Html.EditorFor`.</span></span> <span data-ttu-id="ed65f-141">Per i dettagli, vedere la sezione **Alternative helper HTML per l'helper tag Input**.</span><span class="sxs-lookup"><span data-stu-id="ed65f-141">See the **HTML Helper alternatives to Input Tag Helper** section for details.</span></span>

* <span data-ttu-id="ed65f-142">Consente una tipizzazione forte.</span><span class="sxs-lookup"><span data-stu-id="ed65f-142">Provides strong typing.</span></span> <span data-ttu-id="ed65f-143">Se il nome di proprietà viene modificato e non si aggiorna l'helper tag, si ottiene un errore simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="ed65f-143">If the name of the property changes and you don't update the Tag Helper you'll get an error similar to the following:</span></span>

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

<span data-ttu-id="ed65f-144">L'helper tag `Input` imposta l'attributo HTML `type` in base al tipo .NET.</span><span class="sxs-lookup"><span data-stu-id="ed65f-144">The `Input` Tag Helper sets the HTML `type` attribute based on the .NET type.</span></span> <span data-ttu-id="ed65f-145">La tabella seguente elenca alcuni tipi .NET comuni e il tipo HTML generato. Non sono elencati tutti i tipi .NET.</span><span class="sxs-lookup"><span data-stu-id="ed65f-145">The following table lists some common .NET types and generated HTML type (not every .NET type is listed).</span></span>

|<span data-ttu-id="ed65f-146">Tipo .NET</span><span class="sxs-lookup"><span data-stu-id="ed65f-146">.NET type</span></span>|<span data-ttu-id="ed65f-147">Tipo Input</span><span class="sxs-lookup"><span data-stu-id="ed65f-147">Input Type</span></span>|
|---|---|
|<span data-ttu-id="ed65f-148">Bool</span><span class="sxs-lookup"><span data-stu-id="ed65f-148">Bool</span></span>|<span data-ttu-id="ed65f-149">type="checkbox"</span><span class="sxs-lookup"><span data-stu-id="ed65f-149">type=”checkbox”</span></span>|
|<span data-ttu-id="ed65f-150">Stringa</span><span class="sxs-lookup"><span data-stu-id="ed65f-150">String</span></span>|<span data-ttu-id="ed65f-151">type="text"</span><span class="sxs-lookup"><span data-stu-id="ed65f-151">type=”text”</span></span>|
|<span data-ttu-id="ed65f-152">DateTime</span><span class="sxs-lookup"><span data-stu-id="ed65f-152">DateTime</span></span>|<span data-ttu-id="ed65f-153">type="datetime"</span><span class="sxs-lookup"><span data-stu-id="ed65f-153">type=”datetime”</span></span>|
|<span data-ttu-id="ed65f-154">Byte</span><span class="sxs-lookup"><span data-stu-id="ed65f-154">Byte</span></span>|<span data-ttu-id="ed65f-155">type="number"</span><span class="sxs-lookup"><span data-stu-id="ed65f-155">type=”number”</span></span>|
|<span data-ttu-id="ed65f-156">Int</span><span class="sxs-lookup"><span data-stu-id="ed65f-156">Int</span></span>|<span data-ttu-id="ed65f-157">type="number"</span><span class="sxs-lookup"><span data-stu-id="ed65f-157">type=”number”</span></span>|
|<span data-ttu-id="ed65f-158">Single, Double</span><span class="sxs-lookup"><span data-stu-id="ed65f-158">Single, Double</span></span>|<span data-ttu-id="ed65f-159">type="number"</span><span class="sxs-lookup"><span data-stu-id="ed65f-159">type=”number”</span></span>|


<span data-ttu-id="ed65f-160">La tabella seguente illustra alcuni attributi di [annotazioni dei dati](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) comuni di cui l'helper tag Input esegue il mapping a tipi di input specifici (non tutti gli attributi di convalida sono elencati):</span><span class="sxs-lookup"><span data-stu-id="ed65f-160">The following table shows some common [data annotations](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes that the input tag helper will map to specific input types (not every validation attribute is listed):</span></span>


|<span data-ttu-id="ed65f-161">Attributo</span><span class="sxs-lookup"><span data-stu-id="ed65f-161">Attribute</span></span>|<span data-ttu-id="ed65f-162">Tipo Input</span><span class="sxs-lookup"><span data-stu-id="ed65f-162">Input Type</span></span>|
|---|---|
|<span data-ttu-id="ed65f-163">[EmailAddress]</span><span class="sxs-lookup"><span data-stu-id="ed65f-163">[EmailAddress]</span></span>|<span data-ttu-id="ed65f-164">type="email"</span><span class="sxs-lookup"><span data-stu-id="ed65f-164">type=”email”</span></span>|
|<span data-ttu-id="ed65f-165">[Url]</span><span class="sxs-lookup"><span data-stu-id="ed65f-165">[Url]</span></span>|<span data-ttu-id="ed65f-166">type="url"</span><span class="sxs-lookup"><span data-stu-id="ed65f-166">type=”url”</span></span>|
|<span data-ttu-id="ed65f-167">[HiddenInput]</span><span class="sxs-lookup"><span data-stu-id="ed65f-167">[HiddenInput]</span></span>|<span data-ttu-id="ed65f-168">type="hidden"</span><span class="sxs-lookup"><span data-stu-id="ed65f-168">type=”hidden”</span></span>|
|<span data-ttu-id="ed65f-169">[Phone]</span><span class="sxs-lookup"><span data-stu-id="ed65f-169">[Phone]</span></span>|<span data-ttu-id="ed65f-170">type="tel"</span><span class="sxs-lookup"><span data-stu-id="ed65f-170">type=”tel”</span></span>|
|<span data-ttu-id="ed65f-171">[DataType(DataType.Password)]</span><span class="sxs-lookup"><span data-stu-id="ed65f-171">[DataType(DataType.Password)]</span></span>| <span data-ttu-id="ed65f-172">type="password"</span><span class="sxs-lookup"><span data-stu-id="ed65f-172">type=”password”</span></span>|
|<span data-ttu-id="ed65f-173">[DataType(DataType.Date)]</span><span class="sxs-lookup"><span data-stu-id="ed65f-173">[DataType(DataType.Date)]</span></span>| <span data-ttu-id="ed65f-174">type="date"</span><span class="sxs-lookup"><span data-stu-id="ed65f-174">type=”date”</span></span>|
|<span data-ttu-id="ed65f-175">[DataType(DataType.Time)]</span><span class="sxs-lookup"><span data-stu-id="ed65f-175">[DataType(DataType.Time)]</span></span>| <span data-ttu-id="ed65f-176">type="time"</span><span class="sxs-lookup"><span data-stu-id="ed65f-176">type=”time”</span></span>|


<span data-ttu-id="ed65f-177">Esempio:</span><span class="sxs-lookup"><span data-stu-id="ed65f-177">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

<span data-ttu-id="ed65f-178">Il codice precedente genera il codice HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="ed65f-178">The code above generates the following HTML:</span></span>

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid email address."
              data-val-required="The Email Address field is required."
              id="Email" name="Email" value="" /> <br>
       Password:
       <input type="password" data-val="true"
              data-val-required="The Password field is required."
              id="Password" name="Password" /><br>
       <button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

<span data-ttu-id="ed65f-179">Le annotazioni dei dati applicate alle proprietà `Email` e `Password` generano metadati per il modello.</span><span class="sxs-lookup"><span data-stu-id="ed65f-179">The data annotations applied to the `Email` and `Password` properties generate metadata on the model.</span></span> <span data-ttu-id="ed65f-180">L'helper tag Input usa i metadati del modello e genere attributi [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` (vedere [Convalida del modello](../models/validation.md)).</span><span class="sxs-lookup"><span data-stu-id="ed65f-180">The Input Tag Helper consumes the model metadata and produces [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` attributes (see [Model Validation](../models/validation.md)).</span></span> <span data-ttu-id="ed65f-181">Questi attributi descrivono i validator da collegare ai campi di input.</span><span class="sxs-lookup"><span data-stu-id="ed65f-181">These attributes describe the validators to attach to the input fields.</span></span> <span data-ttu-id="ed65f-182">Ciò consente una convalida HTML5 e [jQuery](https://jquery.com/) discreta.</span><span class="sxs-lookup"><span data-stu-id="ed65f-182">This provides unobtrusive HTML5 and [jQuery](https://jquery.com/) validation.</span></span> <span data-ttu-id="ed65f-183">Gli attributi discreti hanno il formato `data-val-rule="Error Message"`, in cui rule è il nome della regola di convalida (ad esempio `data-val-required`, `data-val-email`, `data-val-maxlength` e così via). Se l'attributo specifica un messaggio di errore, quest'ultimo costituisce il valore dell'attributo `data-val-rule`.</span><span class="sxs-lookup"><span data-stu-id="ed65f-183">The unobtrusive attributes have the format `data-val-rule="Error Message"`, where rule is the name of the validation rule (such as `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) If an error message is provided in the attribute, it's displayed as the value for the `data-val-rule` attribute.</span></span> <span data-ttu-id="ed65f-184">Esistono anche attributi di `data-val-ruleName-argumentName="argumentValue"` di Form che offrono dettagli aggiuntivi sulla regola, ad esempio, `data-val-maxlength-max="1024"` .</span><span class="sxs-lookup"><span data-stu-id="ed65f-184">There are also attributes of the form `data-val-ruleName-argumentName="argumentValue"` that provide additional details about the rule, for example, `data-val-maxlength-max="1024"` .</span></span>

### <a name="html-helper-alternatives-to-input-tag-helper"></a><span data-ttu-id="ed65f-185">Alternative helper HTML per l'helper tag Input</span><span class="sxs-lookup"><span data-stu-id="ed65f-185">HTML Helper alternatives to Input Tag Helper</span></span>

<span data-ttu-id="ed65f-186">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` e `Html.EditorFor` hanno funzionalità che si sovrappongono a quelle dell'helper tag Input.</span><span class="sxs-lookup"><span data-stu-id="ed65f-186">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` and `Html.EditorFor` have overlapping features with the Input Tag Helper.</span></span> <span data-ttu-id="ed65f-187">L'helper tag Input imposta automaticamente l'attributo `type`, mentre `Html.TextBox` e `Html.TextBoxFor` non lo fanno.</span><span class="sxs-lookup"><span data-stu-id="ed65f-187">The Input Tag Helper will automatically set the `type` attribute; `Html.TextBox` and `Html.TextBoxFor` won't.</span></span> <span data-ttu-id="ed65f-188">`Html.Editor` e `Html.EditorFor` gestiscono raccolte, oggetti complessi e modelli, mentre l'helper tag Input non lo fa.</span><span class="sxs-lookup"><span data-stu-id="ed65f-188">`Html.Editor` and `Html.EditorFor` handle collections, complex objects and templates; the Input Tag Helper doesn't.</span></span> <span data-ttu-id="ed65f-189">L'helper tag Input, `Html.EditorFor` e `Html.TextBoxFor` sono fortemente tipizzati (usano espressioni lambda), mentre `Html.TextBox` e `Html.Editor` non lo sono (usano nomi di espressioni).</span><span class="sxs-lookup"><span data-stu-id="ed65f-189">The Input Tag Helper, `Html.EditorFor`  and  `Html.TextBoxFor` are strongly typed (they use lambda expressions); `Html.TextBox` and `Html.Editor` are not (they use expression names).</span></span>

### <a name="htmlattributes"></a><span data-ttu-id="ed65f-190">HtmlAttributes</span><span class="sxs-lookup"><span data-stu-id="ed65f-190">HtmlAttributes</span></span>

<span data-ttu-id="ed65f-191">Quando eseguono i modelli predefiniti, `@Html.Editor()` e `@Html.EditorFor()` usano una voce `ViewDataDictionary` speciale denominata `htmlAttributes`.</span><span class="sxs-lookup"><span data-stu-id="ed65f-191">`@Html.Editor()` and `@Html.EditorFor()` use a special `ViewDataDictionary` entry named `htmlAttributes` when executing their default templates.</span></span> <span data-ttu-id="ed65f-192">È possibile aumentare questo comportamento usando i parametri `additionalViewData`.</span><span class="sxs-lookup"><span data-stu-id="ed65f-192">This behavior is optionally augmented using `additionalViewData` parameters.</span></span> <span data-ttu-id="ed65f-193">La chiave "htmlAttributes" non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="ed65f-193">The key "htmlAttributes" is case-insensitive.</span></span> <span data-ttu-id="ed65f-194">La chiave "htmlAttributes" viene gestita in modo analogo all'oggetto `htmlAttributes` passato agli helper di input come `@Html.TextBox()`.</span><span class="sxs-lookup"><span data-stu-id="ed65f-194">The key "htmlAttributes" is handled similarly to the `htmlAttributes` object passed to input helpers like `@Html.TextBox()`.</span></span>

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a><span data-ttu-id="ed65f-195">Nomi delle espressioni</span><span class="sxs-lookup"><span data-stu-id="ed65f-195">Expression names</span></span>

<span data-ttu-id="ed65f-196">Il valore dell'attributo `asp-for` è un `ModelExpression` e il lato destro di un'espressione lambda.</span><span class="sxs-lookup"><span data-stu-id="ed65f-196">The `asp-for` attribute value is a `ModelExpression` and the right hand side of a lambda expression.</span></span> <span data-ttu-id="ed65f-197">Pertanto, `asp-for="Property1"` diventa `m => m.Property1` nel codice generato. Per questo motivo il prefisso `Model` non è necessario.</span><span class="sxs-lookup"><span data-stu-id="ed65f-197">Therefore, `asp-for="Property1"` becomes `m => m.Property1` in the generated code which is why you don't need to prefix with `Model`.</span></span> <span data-ttu-id="ed65f-198">È possibile usare il carattere "@" per iniziare un'espressione inline, spostandolo prima di `m.`:</span><span class="sxs-lookup"><span data-stu-id="ed65f-198">You can use the "@" character to start an inline expression and move before the `m.`:</span></span>

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

<span data-ttu-id="ed65f-199">Genera quanto segue:</span><span class="sxs-lookup"><span data-stu-id="ed65f-199">Generates the following:</span></span>

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

<span data-ttu-id="ed65f-200">Con le proprietà delle raccolte, `asp-for="CollectionProperty[23].Member"` genera lo stesso nome di `asp-for="CollectionProperty[i].Member"` se il valore di `i` è `23`.</span><span class="sxs-lookup"><span data-stu-id="ed65f-200">With collection properties, `asp-for="CollectionProperty[23].Member"` generates the same name as `asp-for="CollectionProperty[i].Member"` when `i` has the value `23`.</span></span>

### <a name="navigating-child-properties"></a><span data-ttu-id="ed65f-201">Esplorazione delle proprietà figlio</span><span class="sxs-lookup"><span data-stu-id="ed65f-201">Navigating child properties</span></span>

<span data-ttu-id="ed65f-202">È anche possibile passare alle proprietà figlio tramite il percorso delle proprietà del modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ed65f-202">You can also navigate to child properties using the property path of the view model.</span></span> <span data-ttu-id="ed65f-203">Si consideri una classe modello più complessa contenente una proprietà `Address` figlio.</span><span class="sxs-lookup"><span data-stu-id="ed65f-203">Consider a more complex model class that contains a child `Address` property.</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

<span data-ttu-id="ed65f-204">Nella visualizzazione, viene eseguita l'associazione a `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="ed65f-204">In the view, we bind to `Address.AddressLine1`:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

<span data-ttu-id="ed65f-205">Per `Address.AddressLine1` viene generato il codice HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="ed65f-205">The following HTML is generated for `Address.AddressLine1`:</span></span>

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a><span data-ttu-id="ed65f-206">Nomi delle espressioni e raccolte</span><span class="sxs-lookup"><span data-stu-id="ed65f-206">Expression names and Collections</span></span>

<span data-ttu-id="ed65f-207">Esempio di modello contenente una matrice di `Colors`:</span><span class="sxs-lookup"><span data-stu-id="ed65f-207">Sample, a model containing an array of `Colors`:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

<span data-ttu-id="ed65f-208">Metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="ed65f-208">The action method:</span></span>

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

<span data-ttu-id="ed65f-209">Il codice Razor seguente illustra come accedere a un elemento `Color` specifico:</span><span class="sxs-lookup"><span data-stu-id="ed65f-209">The following Razor shows how you access a specific `Color` element:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

<span data-ttu-id="ed65f-210">Modello *Views/Shared/EditorTemplates/String.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ed65f-210">The *Views/Shared/EditorTemplates/String.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

<span data-ttu-id="ed65f-211">Esempio di uso di `List<T>`:</span><span class="sxs-lookup"><span data-stu-id="ed65f-211">Sample using `List<T>`:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

<span data-ttu-id="ed65f-212">Il codice Razor seguente illustra come eseguire l'iterazione in una raccolta:</span><span class="sxs-lookup"><span data-stu-id="ed65f-212">The following Razor shows how to iterate over a collection:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

<span data-ttu-id="ed65f-213">Modello *Views/Shared/EditorTemplates/ToDoItem.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ed65f-213">The *Views/Shared/EditorTemplates/ToDoItem.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
><span data-ttu-id="ed65f-214">Usare sempre `for` (e *non* `foreach`) per eseguire l'iterazione in un elenco.</span><span class="sxs-lookup"><span data-stu-id="ed65f-214">Always use `for` (and *not* `foreach`) to iterate over a list.</span></span> <span data-ttu-id="ed65f-215">La valutazione di un indicizzatore in un'espressione LINQ può essere dispendiosa e deve essere ridotta a al minimo.</span><span class="sxs-lookup"><span data-stu-id="ed65f-215">Evaluating an indexer in a LINQ expression can be expensive and should be minimized.</span></span>

&nbsp;

>[!NOTE]
><span data-ttu-id="ed65f-216">Il codice di esempio commentato precedente illustra come sostituire l'espressione lambda con l'operatore `@` per accedere a ogni `ToDoItem` nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="ed65f-216">The commented sample code above shows how you would replace the lambda expression with the `@` operator to access each `ToDoItem` in the list.</span></span>

## <a name="the-textarea-tag-helper"></a><span data-ttu-id="ed65f-217">Helper tag Textarea</span><span class="sxs-lookup"><span data-stu-id="ed65f-217">The Textarea Tag Helper</span></span>

<span data-ttu-id="ed65f-218">`Textarea Tag Helper` è simile all'helper Tag Input.</span><span class="sxs-lookup"><span data-stu-id="ed65f-218">The `Textarea Tag Helper` tag helper is  similar to the Input Tag Helper.</span></span>

* <span data-ttu-id="ed65f-219">Genera gli attributi `id` e `name`, nonché gli attributi di convalida dei dati dal modello per un elemento [ \<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea).</span><span class="sxs-lookup"><span data-stu-id="ed65f-219">Generates the `id` and `name` attributes, and the data validation attributes from the model for a [\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea) element.</span></span>

* <span data-ttu-id="ed65f-220">Consente una tipizzazione forte.</span><span class="sxs-lookup"><span data-stu-id="ed65f-220">Provides strong typing.</span></span>

* <span data-ttu-id="ed65f-221">Helper HTML alternativo: `Html.TextAreaFor`</span><span class="sxs-lookup"><span data-stu-id="ed65f-221">HTML Helper alternative: `Html.TextAreaFor`</span></span>

<span data-ttu-id="ed65f-222">Esempio:</span><span class="sxs-lookup"><span data-stu-id="ed65f-222">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

<span data-ttu-id="ed65f-223">Viene generato il codice HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="ed65f-223">The following HTML is generated:</span></span>

```HTML
<form method="post" action="/Demo/RegisterTextArea">
  <textarea data-val="true"
   data-val-maxlength="The field Description must be a string or array type with a maximum length of &#x27;1024&#x27;."
   data-val-maxlength-max="1024"
   data-val-minlength="The field Description must be a string or array type with a minimum length of &#x27;5&#x27;."
   data-val-minlength-min="5"
   id="Description" name="Description">
  </textarea>
  <button type="submit">Test</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-label-tag-helper"></a><span data-ttu-id="ed65f-224">Helper tag Label</span><span class="sxs-lookup"><span data-stu-id="ed65f-224">The Label Tag Helper</span></span>

* <span data-ttu-id="ed65f-225">Genera la didascalia dell'etichetta e l'attributo `for` in un elemento [ <label> ](https://www.w3.org/wiki/HTML/Elements/label) per un nome di espressione</span><span class="sxs-lookup"><span data-stu-id="ed65f-225">Generates the label caption and `for` attribute on a [<label>](https://www.w3.org/wiki/HTML/Elements/label) element for an expression name</span></span>

* <span data-ttu-id="ed65f-226">Helper HTML alternativo: `Html.LabelFor`.</span><span class="sxs-lookup"><span data-stu-id="ed65f-226">HTML Helper alternative: `Html.LabelFor`.</span></span>

<span data-ttu-id="ed65f-227">Rispetto a un elemento etichetta HTML puro, `Label Tag Helper` offre i vantaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="ed65f-227">The `Label Tag Helper`  provides the following benefits over a pure HTML label element:</span></span>

* <span data-ttu-id="ed65f-228">Si ottiene automaticamente il valore descrittivo dell'etichetta dall'attributo `Display`.</span><span class="sxs-lookup"><span data-stu-id="ed65f-228">You automatically get the descriptive label value from the `Display` attribute.</span></span> <span data-ttu-id="ed65f-229">Il nome visualizzato desiderato può cambiare nel tempo e la combinazione dell'attributo `Display` e dell'helper tag Label applica l'attributo `Display` ovunque venga usato.</span><span class="sxs-lookup"><span data-stu-id="ed65f-229">The intended display name might change over time, and the combination of `Display` attribute and Label Tag Helper will apply the `Display` everywhere it's used.</span></span>

* <span data-ttu-id="ed65f-230">Meno markup nel codice sorgente</span><span class="sxs-lookup"><span data-stu-id="ed65f-230">Less markup in source code</span></span>

* <span data-ttu-id="ed65f-231">Tipizzazione forte con la proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="ed65f-231">Strong typing with the model property.</span></span>

<span data-ttu-id="ed65f-232">Esempio:</span><span class="sxs-lookup"><span data-stu-id="ed65f-232">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

<span data-ttu-id="ed65f-233">Per l'elemento `<label>` viene generato il codice HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="ed65f-233">The following HTML is generated for the `<label>` element:</span></span>

```HTML
<label for="Email">Email Address</label>
```

<span data-ttu-id="ed65f-234">L'helper tag Label genera il valore dell'attributo `for` di "Email", che è l'ID associato all'elemento `<input>`.</span><span class="sxs-lookup"><span data-stu-id="ed65f-234">The Label Tag Helper generated the `for` attribute value of "Email", which is the ID associated with the `<input>` element.</span></span> <span data-ttu-id="ed65f-235">Gli helper tag generano elementi `id` e `for` coerenti. In questo modo possono essere associati correttamente.</span><span class="sxs-lookup"><span data-stu-id="ed65f-235">The Tag Helpers generate consistent `id` and `for` elements so they can be correctly associated.</span></span> <span data-ttu-id="ed65f-236">La didascalia di questo esempio deriva dall'attributo `Display`.</span><span class="sxs-lookup"><span data-stu-id="ed65f-236">The caption in this sample comes from the `Display` attribute.</span></span> <span data-ttu-id="ed65f-237">Se il modello non contiene un attributo `Display`, la didascalia corrisponde al nome della proprietà dell'espressione.</span><span class="sxs-lookup"><span data-stu-id="ed65f-237">If the model didn't contain a `Display` attribute, the caption would be the property name of the expression.</span></span>

## <a name="the-validation-tag-helpers"></a><span data-ttu-id="ed65f-238">Helper tag di convalida</span><span class="sxs-lookup"><span data-stu-id="ed65f-238">The Validation Tag Helpers</span></span>

<span data-ttu-id="ed65f-239">Ci sono due helper tag di convalida.</span><span class="sxs-lookup"><span data-stu-id="ed65f-239">There are two Validation Tag Helpers.</span></span> <span data-ttu-id="ed65f-240">`Validation Message Tag Helper`, che visualizza un messaggio di convalida per una singola proprietà del modello, e `Validation Summary Tag Helper`, che visualizza un riepilogo degli errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="ed65f-240">The `Validation Message Tag Helper` (which displays a validation message for a single property on your model), and the `Validation Summary Tag Helper` (which displays a summary of validation errors).</span></span> <span data-ttu-id="ed65f-241">`Input Tag Helper` aggiunge attributi di convalida sul lato client HTML5 agli elementi di input in base agli attributi di annotazione dei dati per le classi del modello.</span><span class="sxs-lookup"><span data-stu-id="ed65f-241">The `Input Tag Helper` adds HTML5 client side validation attributes to input elements based on data annotation attributes on your model classes.</span></span> <span data-ttu-id="ed65f-242">La convalida viene eseguita anche per il server.</span><span class="sxs-lookup"><span data-stu-id="ed65f-242">Validation is also performed on the server.</span></span> <span data-ttu-id="ed65f-243">Quando si verifica un errore di convalida, l'helper tag di convalida visualizza questi messaggi di errore.</span><span class="sxs-lookup"><span data-stu-id="ed65f-243">The Validation Tag Helper displays these error messages when a validation error occurs.</span></span>

### <a name="the-validation-message-tag-helper"></a><span data-ttu-id="ed65f-244">Helper tag Validation Message</span><span class="sxs-lookup"><span data-stu-id="ed65f-244">The Validation Message Tag Helper</span></span>

* <span data-ttu-id="ed65f-245">Aggiunge l'attributo [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` all'elemento [span](https://developer.mozilla.org/docs/Web/HTML/Element/span), che collega i messaggi di errore di convalida nel campo di input della proprietà del modello specificato.</span><span class="sxs-lookup"><span data-stu-id="ed65f-245">Adds the [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  `data-valmsg-for="property"` attribute to the [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, which attaches the validation error messages on the input field of the specified model property.</span></span> <span data-ttu-id="ed65f-246">Quando si verifica un errore di convalida sul lato client, [jQuery](https://jquery.com/) visualizza il messaggio di errore nell'elemento `<span>`.</span><span class="sxs-lookup"><span data-stu-id="ed65f-246">When a client side validation error occurs, [jQuery](https://jquery.com/) displays the error message in the `<span>` element.</span></span>

* <span data-ttu-id="ed65f-247">La convalida viene eseguita anche nel server.</span><span class="sxs-lookup"><span data-stu-id="ed65f-247">Validation also takes place on the server.</span></span> <span data-ttu-id="ed65f-248">Nei client JavaScript può essere disabilitato e una parte della convalida può essere eseguita solo sul lato server.</span><span class="sxs-lookup"><span data-stu-id="ed65f-248">Clients may have JavaScript disabled and some validation can only be done on the server side.</span></span>

* <span data-ttu-id="ed65f-249">Helper HTML alternativo: `Html.ValidationMessageFor`</span><span class="sxs-lookup"><span data-stu-id="ed65f-249">HTML Helper alternative: `Html.ValidationMessageFor`</span></span>

<span data-ttu-id="ed65f-250">`Validation Message Tag Helper` viene usato con l'attributo `asp-validation-for` in un elemento HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span).</span><span class="sxs-lookup"><span data-stu-id="ed65f-250">The `Validation Message Tag Helper`  is used with the `asp-validation-for` attribute on a HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element.</span></span>

```HTML
<span asp-validation-for="Email"></span>
```

<span data-ttu-id="ed65f-251">L'helper tag Validation Message genera il codice HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="ed65f-251">The Validation Message Tag Helper will generate the following HTML:</span></span>

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

<span data-ttu-id="ed65f-252">In genere si usa `Validation Message Tag Helper` dopo un helper tag `Input` per la stessa proprietà.</span><span class="sxs-lookup"><span data-stu-id="ed65f-252">You generally use the `Validation Message Tag Helper`  after an `Input` Tag Helper for the same property.</span></span> <span data-ttu-id="ed65f-253">In questo modo è possibile visualizzare i messaggi di errore di convalida vicino all'input che ha causato l'errore.</span><span class="sxs-lookup"><span data-stu-id="ed65f-253">Doing so displays any validation error messages near the input that caused the error.</span></span>

> [!NOTE]
> <span data-ttu-id="ed65f-254">È necessario avere una visualizzazione con i riferimenti agli script JavaScript e [jQuery](https://jquery.com/) corretti per la convalida lato client.</span><span class="sxs-lookup"><span data-stu-id="ed65f-254">You must have a view with the correct JavaScript and [jQuery](https://jquery.com/) script references in place for client side validation.</span></span> <span data-ttu-id="ed65f-255">Per altre informazioni, vedere [Convalida del modello](../models/validation.md).</span><span class="sxs-lookup"><span data-stu-id="ed65f-255">See [Model Validation](../models/validation.md) for more information.</span></span>

<span data-ttu-id="ed65f-256">Se si verifica un errore di convalida sul lato server, ad esempio quando la convalida sul lato server è personalizzata o quando la convalida sul lato client è disabilitata, MVC inserisce il messaggio di errore nel corpo dell'elemento `<span>`.</span><span class="sxs-lookup"><span data-stu-id="ed65f-256">When a server side validation error occurs (for example when you have custom server side validation or client-side validation is disabled), MVC places that error message as the body of the `<span>` element.</span></span>

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a><span data-ttu-id="ed65f-257">Helper tag Validation Summary</span><span class="sxs-lookup"><span data-stu-id="ed65f-257">The Validation Summary Tag Helper</span></span>

* <span data-ttu-id="ed65f-258">Considera come destinazione gli elementi `<div>` con l'attributo `asp-validation-summary`</span><span class="sxs-lookup"><span data-stu-id="ed65f-258">Targets `<div>` elements with the `asp-validation-summary` attribute</span></span>

* <span data-ttu-id="ed65f-259">Helper HTML alternativo: `@Html.ValidationSummary`</span><span class="sxs-lookup"><span data-stu-id="ed65f-259">HTML Helper alternative: `@Html.ValidationSummary`</span></span>

<span data-ttu-id="ed65f-260">`Validation Summary Tag Helper` viene usato per visualizzare un riepilogo dei messaggi di convalida.</span><span class="sxs-lookup"><span data-stu-id="ed65f-260">The `Validation Summary Tag Helper`  is used to display a summary of validation messages.</span></span> <span data-ttu-id="ed65f-261">Il valore dell'attributo `asp-validation-summary` può corrispondere a uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="ed65f-261">The `asp-validation-summary` attribute value can be any of the following:</span></span>

|<span data-ttu-id="ed65f-262">asp-validation-summary</span><span class="sxs-lookup"><span data-stu-id="ed65f-262">asp-validation-summary</span></span>|<span data-ttu-id="ed65f-263">Messaggi di convalida visualizzati</span><span class="sxs-lookup"><span data-stu-id="ed65f-263">Validation messages displayed</span></span>|
|--- |--- |
|<span data-ttu-id="ed65f-264">ValidationSummary.All</span><span class="sxs-lookup"><span data-stu-id="ed65f-264">ValidationSummary.All</span></span>|<span data-ttu-id="ed65f-265">Livello di modello e proprietà</span><span class="sxs-lookup"><span data-stu-id="ed65f-265">Property and model level</span></span>|
|<span data-ttu-id="ed65f-266">ValidationSummary.ModelOnly</span><span class="sxs-lookup"><span data-stu-id="ed65f-266">ValidationSummary.ModelOnly</span></span>|<span data-ttu-id="ed65f-267">Modello</span><span class="sxs-lookup"><span data-stu-id="ed65f-267">Model</span></span>|
|<span data-ttu-id="ed65f-268">ValidationSummary.None</span><span class="sxs-lookup"><span data-stu-id="ed65f-268">ValidationSummary.None</span></span>|<span data-ttu-id="ed65f-269">nessuno</span><span class="sxs-lookup"><span data-stu-id="ed65f-269">None</span></span>|

### <a name="sample"></a><span data-ttu-id="ed65f-270">Esempio</span><span class="sxs-lookup"><span data-stu-id="ed65f-270">Sample</span></span>

<span data-ttu-id="ed65f-271">Nell'esempio seguente, il modello di dati viene decorato con attributi `DataAnnotation` e genera messaggi di errore di convalida per l'elemento `<input>`.</span><span class="sxs-lookup"><span data-stu-id="ed65f-271">In the following example, the data model is decorated with `DataAnnotation` attributes, which generates validation error messages on the `<input>` element.</span></span>  <span data-ttu-id="ed65f-272">Quando si verifica un errore di convalida, l'helper tag di convalida visualizza il messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="ed65f-272">When a validation error occurs, the Validation Tag Helper displays the error message:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

<span data-ttu-id="ed65f-273">Codice HTML generato (se il modello è valido):</span><span class="sxs-lookup"><span data-stu-id="ed65f-273">The generated HTML (when the model is valid):</span></span>

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid email address."
   data-val="true"> <br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-select-tag-helper"></a><span data-ttu-id="ed65f-274">Helper tag Select</span><span class="sxs-lookup"><span data-stu-id="ed65f-274">The Select Tag Helper</span></span>

* <span data-ttu-id="ed65f-275">Genera l'elemento [select](https://www.w3.org/wiki/HTML/Elements/select) e gli elementi [option](https://www.w3.org/wiki/HTML/Elements/option) associati per le proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="ed65f-275">Generates [select](https://www.w3.org/wiki/HTML/Elements/select) and associated [option](https://www.w3.org/wiki/HTML/Elements/option) elements for properties of your model.</span></span>

* <span data-ttu-id="ed65f-276">Ha come helper HTML alternativi `Html.DropDownListFor` e `Html.ListBoxFor`</span><span class="sxs-lookup"><span data-stu-id="ed65f-276">Has an HTML Helper alternative `Html.DropDownListFor` and `Html.ListBoxFor`</span></span>

<span data-ttu-id="ed65f-277">`Select Tag Helper` `asp-for` specifica il nome della proprietà di modello per l'elemento [select](https://www.w3.org/wiki/HTML/Elements/select) e `asp-items` specifica gli elementi [option](https://www.w3.org/wiki/HTML/Elements/option).</span><span class="sxs-lookup"><span data-stu-id="ed65f-277">The `Select Tag Helper` `asp-for` specifies the model property  name for the [select](https://www.w3.org/wiki/HTML/Elements/select) element  and `asp-items` specifies the [option](https://www.w3.org/wiki/HTML/Elements/option) elements.</span></span>  <span data-ttu-id="ed65f-278">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="ed65f-278">For example:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

<span data-ttu-id="ed65f-279">Esempio:</span><span class="sxs-lookup"><span data-stu-id="ed65f-279">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

<span data-ttu-id="ed65f-280">Il metodo `Index` inizializza `CountryViewModel`, imposta il paese selezionato e lo passa alla visualizzazione `Index`.</span><span class="sxs-lookup"><span data-stu-id="ed65f-280">The `Index` method initializes the `CountryViewModel`, sets the selected country and passes it to the `Index` view.</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

<span data-ttu-id="ed65f-281">Il metodo `Index` di HTTP POST visualizza la selezione:</span><span class="sxs-lookup"><span data-stu-id="ed65f-281">The HTTP POST `Index` method displays the selection:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

<span data-ttu-id="ed65f-282">Visualizzazione `Index`:</span><span class="sxs-lookup"><span data-stu-id="ed65f-282">The `Index` view:</span></span>

[!code-cshtml[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

<span data-ttu-id="ed65f-283">Che genera il codice HTML seguente (con "CA" selezionato):</span><span class="sxs-lookup"><span data-stu-id="ed65f-283">Which generates the following HTML (with "CA" selected):</span></span>

```html
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

> [!NOTE]
> <span data-ttu-id="ed65f-284">Non è consigliabile usare `ViewBag` o `ViewData` con l'helper tag Select.</span><span class="sxs-lookup"><span data-stu-id="ed65f-284">We don't recommend using `ViewBag` or `ViewData` with the Select Tag Helper.</span></span> <span data-ttu-id="ed65f-285">Un modello di visualizzazione è più solido e in genere presenta meno problemi quando si tratta di fornire metadati MVC.</span><span class="sxs-lookup"><span data-stu-id="ed65f-285">A view model is more robust at providing MVC metadata and generally less problematic.</span></span>

<span data-ttu-id="ed65f-286">Il valore dell'attributo `asp-for` rappresenta un caso speciale e non richiede un prefisso `Model`, che invece è richiesto dagli altri attributi di helper tag, ad esempio `asp-items`</span><span class="sxs-lookup"><span data-stu-id="ed65f-286">The `asp-for` attribute value is a special case and doesn't require a `Model` prefix, the other Tag Helper attributes do (such as `asp-items`)</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a><span data-ttu-id="ed65f-287">Associazione di enum</span><span class="sxs-lookup"><span data-stu-id="ed65f-287">Enum binding</span></span>

<span data-ttu-id="ed65f-288">Spesso è comodo usare `<select>` con una proprietà `enum` e generare gli elementi `SelectListItem` dai valori `enum`.</span><span class="sxs-lookup"><span data-stu-id="ed65f-288">It's often convenient to use `<select>` with an `enum` property and generate the `SelectListItem` elements from the `enum` values.</span></span>

<span data-ttu-id="ed65f-289">Esempio:</span><span class="sxs-lookup"><span data-stu-id="ed65f-289">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

<span data-ttu-id="ed65f-290">Il metodo `GetEnumSelectList` genera un oggetto `SelectList` per un enum.</span><span class="sxs-lookup"><span data-stu-id="ed65f-290">The `GetEnumSelectList` method generates a `SelectList` object for an enum.</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

<span data-ttu-id="ed65f-291">Per ottenere un'interfaccia utente più completa, è possibile decorare l'elenco di enumeratori con l'attributo `Display`:</span><span class="sxs-lookup"><span data-stu-id="ed65f-291">You can decorate your enumerator list with the `Display` attribute to get a richer UI:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

<span data-ttu-id="ed65f-292">Viene generato il codice HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="ed65f-292">The following HTML is generated:</span></span>

```HTML
  <form method="post" action="/Home/IndexEnum">
         <select data-val="true" data-val-required="The EnumCountry field is required."
                 id="EnumCountry" name="EnumCountry">
             <option value="0">United Mexican States</option>
             <option value="1">United States of America</option>
             <option value="2">Canada</option>
             <option value="3">France</option>
             <option value="4">Germany</option>
             <option selected="selected" value="5">Spain</option>
         </select>
         <br /><button type="submit">Register</button>
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

### <a name="option-group"></a><span data-ttu-id="ed65f-293">Gruppo di opzioni</span><span class="sxs-lookup"><span data-stu-id="ed65f-293">Option Group</span></span>

<span data-ttu-id="ed65f-294">L'elemento HTML [ \<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) viene generato quando il modello di visualizzazione contiene uno o più oggetti `SelectListGroup`.</span><span class="sxs-lookup"><span data-stu-id="ed65f-294">The HTML  [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) element is generated when the view model contains one or more `SelectListGroup` objects.</span></span>

<span data-ttu-id="ed65f-295">`CountryViewModelGroup` suddivide gli elementi `SelectListItem` nei gruppi "North America" ed "Europe":</span><span class="sxs-lookup"><span data-stu-id="ed65f-295">The `CountryViewModelGroup` groups the `SelectListItem` elements into the "North America" and "Europe" groups:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

<span data-ttu-id="ed65f-296">I due gruppi sono illustrati di seguito:</span><span class="sxs-lookup"><span data-stu-id="ed65f-296">The two groups are shown below:</span></span>

![esempio di gruppi di opzioni](working-with-forms/_static/grp.png)

<span data-ttu-id="ed65f-298">Codice HTML generato:</span><span class="sxs-lookup"><span data-stu-id="ed65f-298">The generated HTML:</span></span>

```HTML
 <form method="post" action="/Home/IndexGroup">
      <select id="Country" name="Country">
          <optgroup label="North America">
              <option value="MEX">Mexico</option>
              <option value="CAN">Canada</option>
              <option value="US">USA</option>
          </optgroup>
          <optgroup label="Europe">
              <option value="FR">France</option>
              <option value="ES">Spain</option>
              <option value="DE">Germany</option>
          </optgroup>
      </select>
      <br /><button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
```

### <a name="multiple-select"></a><span data-ttu-id="ed65f-299">Selezione multipla</span><span class="sxs-lookup"><span data-stu-id="ed65f-299">Multiple select</span></span>

<span data-ttu-id="ed65f-300">L'helper tag Select genera automaticamente l'attributo [multiple = "multiple"](http://w3c.github.io/html-reference/select.html) se la proprietà specificata nell'attributo `asp-for` è `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="ed65f-300">The Select Tag Helper  will automatically generate the [multiple = "multiple"](http://w3c.github.io/html-reference/select.html)  attribute if the property specified in the `asp-for` attribute is an `IEnumerable`.</span></span> <span data-ttu-id="ed65f-301">Dato, ad esempio, il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="ed65f-301">For example, given the following model:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

<span data-ttu-id="ed65f-302">Con la visualizzazione seguente:</span><span class="sxs-lookup"><span data-stu-id="ed65f-302">With the following view:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

<span data-ttu-id="ed65f-303">Viene generato il codice HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="ed65f-303">Generates the following HTML:</span></span>

```HTML
<form method="post" action="/Home/IndexMultiSelect">
    <select id="CountryCodes"
    multiple="multiple"
    name="CountryCodes"><option value="MX">Mexico</option>
<option value="CA">Canada</option>
<option value="US">USA</option>
<option value="FR">France</option>
<option value="ES">Spain</option>
<option value="DE">Germany</option>
</select>
    <br /><button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

### <a name="no-selection"></a><span data-ttu-id="ed65f-304">Nessuna selezione</span><span class="sxs-lookup"><span data-stu-id="ed65f-304">No selection</span></span>

<span data-ttu-id="ed65f-305">Se si usa l'opzione "not specified" in più pagine, è possibile creare un modello per evitare di ripetere lo stesso codice HTML:</span><span class="sxs-lookup"><span data-stu-id="ed65f-305">If you find yourself using the "not specified" option in multiple pages, you can create a template to eliminate repeating the HTML:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

<span data-ttu-id="ed65f-306">Modello *Views/Shared/EditorTemplates/CountryViewModel.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ed65f-306">The *Views/Shared/EditorTemplates/CountryViewModel.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

<span data-ttu-id="ed65f-307">L'aggiunta di elementi HTML [ \<option>](https://www.w3.org/wiki/HTML/Elements/option) non è limitata al caso relativo a *nessuna selezione*.</span><span class="sxs-lookup"><span data-stu-id="ed65f-307">Adding HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) elements isn't limited to the *No selection* case.</span></span> <span data-ttu-id="ed65f-308">La visualizzazione e il metodo di azione seguenti, ad esempio, generano codice HTML simile al codice precedente:</span><span class="sxs-lookup"><span data-stu-id="ed65f-308">For example, the following view and action method will generate HTML similar to the code above:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

<span data-ttu-id="ed65f-309">L'elemento `<option>` corretto viene selezionato (con l'attributo `selected="selected"`) a seconda del valore `Country` corrente.</span><span class="sxs-lookup"><span data-stu-id="ed65f-309">The correct `<option>` element will be selected ( contain the `selected="selected"` attribute) depending on the current `Country` value.</span></span>

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
 ```

## <a name="additional-resources"></a><span data-ttu-id="ed65f-310">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="ed65f-310">Additional resources</span></span>

* [<span data-ttu-id="ed65f-311">Helper tag</span><span class="sxs-lookup"><span data-stu-id="ed65f-311">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="ed65f-312">Elemento Form HTML</span><span class="sxs-lookup"><span data-stu-id="ed65f-312">HTML Form element</span></span>](https://www.w3.org/TR/html401/interact/forms.html)
* [<span data-ttu-id="ed65f-313">Token di verifica della richiesta</span><span class="sxs-lookup"><span data-stu-id="ed65f-313">Request Verification Token</span></span>](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)
* [<span data-ttu-id="ed65f-314">Associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="ed65f-314">Model Binding</span></span>](xref:mvc/models/model-binding)
* [<span data-ttu-id="ed65f-315">Convalida del modello</span><span class="sxs-lookup"><span data-stu-id="ed65f-315">Model Validation</span></span>](xref:mvc/models/validation)
* [<span data-ttu-id="ed65f-316">Interfaccia IAttributeAdapter</span><span class="sxs-lookup"><span data-stu-id="ed65f-316">IAttributeAdapter Interface</span></span>](/dotnet/api/Microsoft.AspNetCore.Mvc.DataAnnotations.IAttributeAdapter)
* [<span data-ttu-id="ed65f-317">Frammenti di codice per questo documento</span><span class="sxs-lookup"><span data-stu-id="ed65f-317">Code snippets for this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample)
