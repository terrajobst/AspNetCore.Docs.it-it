---
title: Helper di tag nei form in ASP.NET Core
author: rick-anderson
description: Descrive gli helper di Tag utilizzati con i moduli predefiniti.
keywords: Helper di tag ASP.NET Core, l'Helper di Tag, Form HTML nel form
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 25595059-4fac-4785-8152-f88590e3169b
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/working-with-forms
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: da36985206521798d3bfe71f6372dc5cc4fca09a
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2017
---
# <a name="introduction-to-using-tag-helpers-in-forms-in-aspnet-core"></a><span data-ttu-id="dcd71-104">Introduzione all'utilizzo di helper di tag nei form in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dcd71-104">Introduction to using tag helpers in forms in ASP.NET Core</span></span>

<span data-ttu-id="dcd71-105">Da [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), e [Jerrie Pelser](https://github.com/jerriep)</span><span class="sxs-lookup"><span data-stu-id="dcd71-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), and [Jerrie Pelser](https://github.com/jerriep)</span></span>

<span data-ttu-id="dcd71-106">Questo documento viene illustrato l'utilizzo di moduli e gli elementi HTML comunemente usati in un Form.</span><span class="sxs-lookup"><span data-stu-id="dcd71-106">This document demonstrates working with Forms and the HTML elements commonly used on a Form.</span></span> <span data-ttu-id="dcd71-107">Il codice HTML [modulo](https://www.w3.org/TR/html401/interact/forms.html) elemento fornisce l'uso di App web meccanismo principale per inviare dati al server.</span><span class="sxs-lookup"><span data-stu-id="dcd71-107">The HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) element provides the primary mechanism web apps use to post back data to the server.</span></span> <span data-ttu-id="dcd71-108">La maggior parte di questo documento descrive [gli helper di Tag](tag-helpers/intro.md) e come consentono in modo produttivo creare form HTML affidabile.</span><span class="sxs-lookup"><span data-stu-id="dcd71-108">Most of this document describes [Tag Helpers](tag-helpers/intro.md) and how they can help you productively create robust HTML forms.</span></span> <span data-ttu-id="dcd71-109">È consigliabile leggere [introduzione per gli helper di Tag](tag-helpers/intro.md) prima di leggere questo documento.</span><span class="sxs-lookup"><span data-stu-id="dcd71-109">We recommend you read [Introduction to Tag Helpers](tag-helpers/intro.md) before you read this document.</span></span>

<span data-ttu-id="dcd71-110">In molti casi, l'helper HTML forniscono un'alternativa a un Helper Tag specifici, ma è importante tenere presente che gli helper di Tag non sostituiscono l'helper HTML e non esiste un Helper di Tag per ogni HTML Helper.</span><span class="sxs-lookup"><span data-stu-id="dcd71-110">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers do not replace HTML Helpers and there is not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="dcd71-111">Se esiste un'alternativa di HTML Helper, è già indicato.</span><span class="sxs-lookup"><span data-stu-id="dcd71-111">When an HTML Helper alternative exists, it is mentioned.</span></span>

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a><span data-ttu-id="dcd71-112">L'Helper di Tag di Form</span><span class="sxs-lookup"><span data-stu-id="dcd71-112">The Form Tag Helper</span></span>

<span data-ttu-id="dcd71-113">Il [modulo](https://www.w3.org/TR/html401/interact/forms.html) Helper di Tag:</span><span class="sxs-lookup"><span data-stu-id="dcd71-113">The [Form](https://www.w3.org/TR/html401/interact/forms.html) Tag Helper:</span></span>

* <span data-ttu-id="dcd71-114">Genera il codice HTML [ \<FORM >](https://www.w3.org/TR/html401/interact/forms.html) `action` valore di attributo per un'azione del controller MVC o una route denominata</span><span class="sxs-lookup"><span data-stu-id="dcd71-114">Generates the HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) `action` attribute value for a MVC controller action or named route</span></span>

* <span data-ttu-id="dcd71-115">Genera l'errore nascosta [richiesta verifica Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) per impedire richieste intersito false (se usato con il `[ValidateAntiForgeryToken]` attributo nel metodo di azione HTTP Post)</span><span class="sxs-lookup"><span data-stu-id="dcd71-115">Generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method)</span></span>

* <span data-ttu-id="dcd71-116">Fornisce il `asp-route-<Parameter Name>` attributo, in cui `<Parameter Name>` viene aggiunto per i valori della route.</span><span class="sxs-lookup"><span data-stu-id="dcd71-116">Provides the `asp-route-<Parameter Name>` attribute, where `<Parameter Name>` is added to the route values.</span></span> <span data-ttu-id="dcd71-117">Il `routeValues` parametri `Html.BeginForm` e `Html.BeginRouteForm` offrono funzionalità simili.</span><span class="sxs-lookup"><span data-stu-id="dcd71-117">The  `routeValues` parameters to `Html.BeginForm` and `Html.BeginRouteForm` provide similar functionality.</span></span>

* <span data-ttu-id="dcd71-118">È un'alternativa di HTML Helper `Html.BeginForm` e`Html.BeginRouteForm`</span><span class="sxs-lookup"><span data-stu-id="dcd71-118">Has an HTML Helper alternative `Html.BeginForm` and `Html.BeginRouteForm`</span></span>

<span data-ttu-id="dcd71-119">Esempio:</span><span class="sxs-lookup"><span data-stu-id="dcd71-119">Sample:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

<span data-ttu-id="dcd71-120">L'Helper di Tag Form precedente genera il codice HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="dcd71-120">The Form Tag Helper above generates the following HTML:</span></span>

```HTML
<form method="post" action="/Demo/Register">
     <!-- Input and Submit elements -->
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

<span data-ttu-id="dcd71-121">Il runtime MVC viene generato il `action` valore dell'attributo dagli attributi, gli Helper di Tag Form `asp-controller` e `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="dcd71-121">The MVC runtime generates the `action` attribute value from the Form Tag Helper attributes `asp-controller` and `asp-action`.</span></span> <span data-ttu-id="dcd71-122">L'Helper di Tag Form genera inoltre nascosta [richiesta verifica Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) per impedire richieste intersito false (se usato con il `[ValidateAntiForgeryToken]` attributo nel metodo di azione HTTP Post).</span><span class="sxs-lookup"><span data-stu-id="dcd71-122">The Form Tag Helper also generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method).</span></span> <span data-ttu-id="dcd71-123">La protezione di un HTML Form puro da richieste intersito false è difficile, l'Helper di Tag Form offre questo servizio per l'utente.</span><span class="sxs-lookup"><span data-stu-id="dcd71-123">Protecting a pure HTML Form from cross-site request forgery is difficult, the Form Tag Helper provides this service for you.</span></span>

### <a name="using-a-named-route"></a><span data-ttu-id="dcd71-124">Utilizzo di una route denominata</span><span class="sxs-lookup"><span data-stu-id="dcd71-124">Using a named route</span></span>

<span data-ttu-id="dcd71-125">Il `asp-route` attributo Helper di Tag possa anche generare codice per l'HTML `action` attributo.</span><span class="sxs-lookup"><span data-stu-id="dcd71-125">The `asp-route` Tag Helper attribute can also generate markup for the HTML `action` attribute.</span></span> <span data-ttu-id="dcd71-126">Un'app con un [route](../../fundamentals/routing.md) denominato `register` Impossibile utilizzare il markup seguente per la pagina di registrazione:</span><span class="sxs-lookup"><span data-stu-id="dcd71-126">An app with a [route](../../fundamentals/routing.md)  named `register` could use the following markup for the registration page:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

<span data-ttu-id="dcd71-127">Molte delle visualizzazioni di *viste/Account* cartella (generato quando si crea una nuova app web con *singoli account utente di*) contengono il [asp-route-returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) attributo:</span><span class="sxs-lookup"><span data-stu-id="dcd71-127">Many of the views in the *Views/Account* folder (generated when you create a new web app with *Individual User Accounts*) contain the [asp-route-returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) attribute:</span></span>

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
><span data-ttu-id="dcd71-128">Con i modelli predefiniti, `returnUrl` solo viene popolato automaticamente quando si tenta di accedere a una risorsa autorizzata ma non autenticate o autorizzate.</span><span class="sxs-lookup"><span data-stu-id="dcd71-128">With the built in templates, `returnUrl` is only populated automatically when you try to access an authorized resource but are not authenticated or authorized.</span></span> <span data-ttu-id="dcd71-129">Quando si tenta l'accesso non autorizzato, il middleware di sicurezza si viene reindirizzati alla pagina di accesso con il `returnUrl` impostato.</span><span class="sxs-lookup"><span data-stu-id="dcd71-129">When you attempt an unauthorized access, the security middleware redirects you to the login page with the `returnUrl` set.</span></span>

## <a name="the-input-tag-helper"></a><span data-ttu-id="dcd71-130">L'Helper di Tag di Input</span><span class="sxs-lookup"><span data-stu-id="dcd71-130">The Input Tag Helper</span></span>

<span data-ttu-id="dcd71-131">L'Helper di Tag di Input viene associata a un elemento HTML [ \<input >](https://www.w3.org/wiki/HTML/Elements/input) elemento da un'espressione di modello nella propria visualizzazione razor.</span><span class="sxs-lookup"><span data-stu-id="dcd71-131">The Input Tag Helper binds an HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) element to a model expression in your razor view.</span></span>

<span data-ttu-id="dcd71-132">Sintassi:</span><span class="sxs-lookup"><span data-stu-id="dcd71-132">Syntax:</span></span>

```HTML
<input asp-for="<Expression Name>" />
```

<span data-ttu-id="dcd71-133">L'Helper di Tag di Input:</span><span class="sxs-lookup"><span data-stu-id="dcd71-133">The Input Tag Helper:</span></span>

* <span data-ttu-id="dcd71-134">Genera il `id` e `name` attributi HTML per il nome dell'espressione specificata nel `asp-for` attributo.</span><span class="sxs-lookup"><span data-stu-id="dcd71-134">Generates the `id` and `name` HTML attributes for the expression name specified in the `asp-for` attribute.</span></span> <span data-ttu-id="dcd71-135">`asp-for="Property1.Property2"` è equivalente a `m => m.Property1.Property2`.</span><span class="sxs-lookup"><span data-stu-id="dcd71-135">`asp-for="Property1.Property2"` is equivalent to `m => m.Property1.Property2`.</span></span> <span data-ttu-id="dcd71-136">Il nome dell'espressione viene utilizzato per il `asp-for` valore dell'attributo.</span><span class="sxs-lookup"><span data-stu-id="dcd71-136">The name of the expression is what is used for the `asp-for` attribute value.</span></span> <span data-ttu-id="dcd71-137">Vedere il [i nomi delle espressioni](#expression-names) sezione per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="dcd71-137">See the [Expression names](#expression-names) section for additional information.</span></span>

* <span data-ttu-id="dcd71-138">Imposta il codice HTML `type` valore in base al tipo di modello dell'attributo e [annotazione dei dati](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) gli attributi applicati per la proprietà del modello</span><span class="sxs-lookup"><span data-stu-id="dcd71-138">Sets the HTML `type` attribute value based on the model type and  [data annotation](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to the model property</span></span>

* <span data-ttu-id="dcd71-139">Il codice HTML non comporta la sovrascrittura `type` quando è specificato un valore dell'attributo</span><span class="sxs-lookup"><span data-stu-id="dcd71-139">Will not overwrite the HTML `type` attribute value when one is specified</span></span>

* <span data-ttu-id="dcd71-140">Genera l'errore [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) gli attributi di convalida da [annotazione dei dati](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) gli attributi applicati alle proprietà del modello</span><span class="sxs-lookup"><span data-stu-id="dcd71-140">Generates [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  validation attributes from [data annotation](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to model properties</span></span>

* <span data-ttu-id="dcd71-141">Dispone di una funzione HTML Helper si sovrappongono con `Html.TextBoxFor` e `Html.EditorFor`.</span><span class="sxs-lookup"><span data-stu-id="dcd71-141">Has an HTML Helper feature overlap with `Html.TextBoxFor` and `Html.EditorFor`.</span></span> <span data-ttu-id="dcd71-142">Vedere il **alternative HTML Helper per l'Helper di Tag di Input** sezione per informazioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="dcd71-142">See the **HTML Helper alternatives to Input Tag Helper** section for details.</span></span>

* <span data-ttu-id="dcd71-143">Fornisce la tipizzazione forte.</span><span class="sxs-lookup"><span data-stu-id="dcd71-143">Provides strong typing.</span></span> <span data-ttu-id="dcd71-144">Se il nome di proprietà viene modificato e non aggiorna l'Helper di Tag si otterrà un errore simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="dcd71-144">If the name of the property changes and you don't update the Tag Helper you'll get an error similar to the following:</span></span>

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

<span data-ttu-id="dcd71-145">Il `Input` Helper di Tag imposta il codice HTML `type` attributo in base al tipo di .NET.</span><span class="sxs-lookup"><span data-stu-id="dcd71-145">The `Input` Tag Helper sets the HTML `type` attribute based on the .NET type.</span></span> <span data-ttu-id="dcd71-146">Nella tabella seguente sono elencati alcuni tipi .NET comuni e un tipo HTML generato (non tutti i tipi .NET sono elencato).</span><span class="sxs-lookup"><span data-stu-id="dcd71-146">The following table lists some common .NET types and generated HTML type (not every .NET type is listed).</span></span>

|<span data-ttu-id="dcd71-147">Tipo .NET</span><span class="sxs-lookup"><span data-stu-id="dcd71-147">.NET type</span></span>|<span data-ttu-id="dcd71-148">Tipo di input</span><span class="sxs-lookup"><span data-stu-id="dcd71-148">Input Type</span></span>|
|---|---|
|<span data-ttu-id="dcd71-149">Bool</span><span class="sxs-lookup"><span data-stu-id="dcd71-149">Bool</span></span>|<span data-ttu-id="dcd71-150">tipo = "checkbox"</span><span class="sxs-lookup"><span data-stu-id="dcd71-150">type=”checkbox”</span></span>|
|<span data-ttu-id="dcd71-151">Stringa</span><span class="sxs-lookup"><span data-stu-id="dcd71-151">String</span></span>|<span data-ttu-id="dcd71-152">tipo = "text"</span><span class="sxs-lookup"><span data-stu-id="dcd71-152">type=”text”</span></span>|
|<span data-ttu-id="dcd71-153">DateTime</span><span class="sxs-lookup"><span data-stu-id="dcd71-153">DateTime</span></span>|<span data-ttu-id="dcd71-154">tipo = "datetime"</span><span class="sxs-lookup"><span data-stu-id="dcd71-154">type=”datetime”</span></span>|
|<span data-ttu-id="dcd71-155">Byte</span><span class="sxs-lookup"><span data-stu-id="dcd71-155">Byte</span></span>|<span data-ttu-id="dcd71-156">tipo = "number"</span><span class="sxs-lookup"><span data-stu-id="dcd71-156">type=”number”</span></span>|
|<span data-ttu-id="dcd71-157">Int</span><span class="sxs-lookup"><span data-stu-id="dcd71-157">Int</span></span>|<span data-ttu-id="dcd71-158">tipo = "number"</span><span class="sxs-lookup"><span data-stu-id="dcd71-158">type=”number”</span></span>|
|<span data-ttu-id="dcd71-159">Single, Double</span><span class="sxs-lookup"><span data-stu-id="dcd71-159">Single, Double</span></span>|<span data-ttu-id="dcd71-160">tipo = "number"</span><span class="sxs-lookup"><span data-stu-id="dcd71-160">type=”number”</span></span>|


<span data-ttu-id="dcd71-161">La tabella seguente illustra alcuni comuni [le annotazioni dei dati](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) gli attributi che l'helper di tag di input verrà eseguito il mapping a tipi specifici di input (non tutti gli attributi di convalida sono elencato):</span><span class="sxs-lookup"><span data-stu-id="dcd71-161">The following table shows some common [data annotations](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes that the input tag helper will map to specific input types (not every validation attribute is listed):</span></span>


|<span data-ttu-id="dcd71-162">Attributo</span><span class="sxs-lookup"><span data-stu-id="dcd71-162">Attribute</span></span>|<span data-ttu-id="dcd71-163">Tipo di input</span><span class="sxs-lookup"><span data-stu-id="dcd71-163">Input Type</span></span>|
|---|---|
|<span data-ttu-id="dcd71-164">[EmailAddress]</span><span class="sxs-lookup"><span data-stu-id="dcd71-164">[EmailAddress]</span></span>|<span data-ttu-id="dcd71-165">tipo = "email"</span><span class="sxs-lookup"><span data-stu-id="dcd71-165">type=”email”</span></span>|
|<span data-ttu-id="dcd71-166">[Url]</span><span class="sxs-lookup"><span data-stu-id="dcd71-166">[Url]</span></span>|<span data-ttu-id="dcd71-167">tipo = "url"</span><span class="sxs-lookup"><span data-stu-id="dcd71-167">type=”url”</span></span>|
|<span data-ttu-id="dcd71-168">[HiddenInput]</span><span class="sxs-lookup"><span data-stu-id="dcd71-168">[HiddenInput]</span></span>|<span data-ttu-id="dcd71-169">tipo = "hidden"</span><span class="sxs-lookup"><span data-stu-id="dcd71-169">type=”hidden”</span></span>|
|<span data-ttu-id="dcd71-170">[Phone]</span><span class="sxs-lookup"><span data-stu-id="dcd71-170">[Phone]</span></span>|<span data-ttu-id="dcd71-171">tipo = "tel"</span><span class="sxs-lookup"><span data-stu-id="dcd71-171">type=”tel”</span></span>|
|<span data-ttu-id="dcd71-172">[DataType(DataType.Password)]</span><span class="sxs-lookup"><span data-stu-id="dcd71-172">[DataType(DataType.Password)]</span></span>| <span data-ttu-id="dcd71-173">tipo = "password"</span><span class="sxs-lookup"><span data-stu-id="dcd71-173">type=”password”</span></span>|
|<span data-ttu-id="dcd71-174">[DataType(DataType.Date)]</span><span class="sxs-lookup"><span data-stu-id="dcd71-174">[DataType(DataType.Date)]</span></span>| <span data-ttu-id="dcd71-175">tipo = "Data"</span><span class="sxs-lookup"><span data-stu-id="dcd71-175">type=”date”</span></span>|
|<span data-ttu-id="dcd71-176">[DataType(DataType.Time)]</span><span class="sxs-lookup"><span data-stu-id="dcd71-176">[DataType(DataType.Time)]</span></span>| <span data-ttu-id="dcd71-177">tipo = "ora"</span><span class="sxs-lookup"><span data-stu-id="dcd71-177">type=”time”</span></span>|


<span data-ttu-id="dcd71-178">Esempio:</span><span class="sxs-lookup"><span data-stu-id="dcd71-178">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

<span data-ttu-id="dcd71-179">Il codice precedente genera il codice HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="dcd71-179">The code above generates the following HTML:</span></span>

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid e-mail address."
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

<span data-ttu-id="dcd71-180">Le annotazioni dei dati applicate al `Email` e `Password` proprietà generano metadati sul modello.</span><span class="sxs-lookup"><span data-stu-id="dcd71-180">The data annotations applied to the `Email` and `Password` properties generate metadata on the model.</span></span> <span data-ttu-id="dcd71-181">L'Helper di Tag di Input utilizza i metadati del modello e produce [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` attributi (vedere [la convalida del modello](../models/validation.md)).</span><span class="sxs-lookup"><span data-stu-id="dcd71-181">The Input Tag Helper consumes the model metadata and produces [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` attributes (see [Model Validation](../models/validation.md)).</span></span> <span data-ttu-id="dcd71-182">Tali attributi descrivono i validator da associare ai campi di input.</span><span class="sxs-lookup"><span data-stu-id="dcd71-182">These attributes describe the validators to attach to the input fields.</span></span> <span data-ttu-id="dcd71-183">In questo modo HTML5 non intrusivi e [jQuery](https://jquery.com/) convalida.</span><span class="sxs-lookup"><span data-stu-id="dcd71-183">This provides unobtrusive HTML5 and [jQuery](https://jquery.com/) validation.</span></span> <span data-ttu-id="dcd71-184">Gli attributi non intrusivi sono del formato `data-val-rule="Error Message"`, in cui regola è il nome della regola di convalida (ad esempio `data-val-required`, `data-val-email`, `data-val-maxlength`, ecc.) Se un messaggio di errore viene fornito nell'attributo, viene visualizzato come valore per il `data-val-rule` attributo.</span><span class="sxs-lookup"><span data-stu-id="dcd71-184">The unobtrusive attributes have the format `data-val-rule="Error Message"`, where rule is the name of the validation rule (such as `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) If an error message is provided in the attribute, it is displayed as the value for the `data-val-rule` attribute.</span></span> <span data-ttu-id="dcd71-185">Esistono anche gli attributi della maschera `data-val-ruleName-argumentName="argumentValue"` che forniscono dettagli aggiuntivi sulla regola, ad esempio, `data-val-maxlength-max="1024"` .</span><span class="sxs-lookup"><span data-stu-id="dcd71-185">There are also attributes of the form `data-val-ruleName-argumentName="argumentValue"` that provide additional details about the rule, for example, `data-val-maxlength-max="1024"` .</span></span>

### <a name="html-helper-alternatives-to-input-tag-helper"></a><span data-ttu-id="dcd71-186">Alternative di HTML Helper per l'Helper di Tag di Input</span><span class="sxs-lookup"><span data-stu-id="dcd71-186">HTML Helper alternatives to Input Tag Helper</span></span>

<span data-ttu-id="dcd71-187">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` e `Html.EditorFor` sovrapposti funzionalità con l'Helper di Tag di Input.</span><span class="sxs-lookup"><span data-stu-id="dcd71-187">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` and `Html.EditorFor` have overlapping features with the Input Tag Helper.</span></span> <span data-ttu-id="dcd71-188">L'Helper di Tag di Input verrà impostato automaticamente il `type` attributo; `Html.TextBox` e `Html.TextBoxFor` No.</span><span class="sxs-lookup"><span data-stu-id="dcd71-188">The Input Tag Helper will automatically set the `type` attribute; `Html.TextBox` and `Html.TextBoxFor` will not.</span></span> <span data-ttu-id="dcd71-189">`Html.Editor`e `Html.EditorFor` gestire raccolte, gli oggetti complessi e i modelli; non l'Helper di Tag di Input.</span><span class="sxs-lookup"><span data-stu-id="dcd71-189">`Html.Editor` and `Html.EditorFor` handle collections, complex objects and templates; the Input Tag Helper does not.</span></span> <span data-ttu-id="dcd71-190">L'Helper di Tag di Input, `Html.EditorFor` e `Html.TextBoxFor` sono fortemente tipizzati (usano le espressioni lambda); `Html.TextBox` e `Html.Editor` non (utilizzano i nomi delle espressioni).</span><span class="sxs-lookup"><span data-stu-id="dcd71-190">The Input Tag Helper, `Html.EditorFor`  and  `Html.TextBoxFor` are strongly typed (they use lambda expressions); `Html.TextBox` and `Html.Editor` are not (they use expression names).</span></span>

### <a name="htmlattributes"></a><span data-ttu-id="dcd71-191">HtmlAttributes</span><span class="sxs-lookup"><span data-stu-id="dcd71-191">HtmlAttributes</span></span>

<span data-ttu-id="dcd71-192">`@Html.Editor()`e `@Html.EditorFor()` utilizzare una speciale `ViewDataDictionary` voce denominata `htmlAttributes` durante l'esecuzione dei modelli predefiniti.</span><span class="sxs-lookup"><span data-stu-id="dcd71-192">`@Html.Editor()` and `@Html.EditorFor()` use a special `ViewDataDictionary` entry named `htmlAttributes` when executing their default templates.</span></span> <span data-ttu-id="dcd71-193">Questo comportamento è facoltativamente ampliato usando `additionalViewData` parametri.</span><span class="sxs-lookup"><span data-stu-id="dcd71-193">This behavior is optionally augmented using `additionalViewData` parameters.</span></span> <span data-ttu-id="dcd71-194">La chiave "htmlAttributes" è tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="dcd71-194">The key "htmlAttributes" is case-insensitive.</span></span> <span data-ttu-id="dcd71-195">La chiave "htmlAttributes" viene gestita in modo analogo al `htmlAttributes` oggetto passato a input helper come `@Html.TextBox()`.</span><span class="sxs-lookup"><span data-stu-id="dcd71-195">The key "htmlAttributes" is handled similarly to the `htmlAttributes` object passed to input helpers like `@Html.TextBox()`.</span></span>

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a><span data-ttu-id="dcd71-196">Nomi delle espressioni</span><span class="sxs-lookup"><span data-stu-id="dcd71-196">Expression names</span></span>

<span data-ttu-id="dcd71-197">Il `asp-for` valore dell'attributo è un `ModelExpression` e il lato destro di un'espressione lambda.</span><span class="sxs-lookup"><span data-stu-id="dcd71-197">The `asp-for` attribute value is a `ModelExpression` and the right hand side of a lambda expression.</span></span> <span data-ttu-id="dcd71-198">Pertanto, `asp-for="Property1"` diventa `m => m.Property1` nel codice generato motivo per cui non è necessario prefisso `Model`.</span><span class="sxs-lookup"><span data-stu-id="dcd71-198">Therefore, `asp-for="Property1"` becomes `m => m.Property1` in the generated code which is why you don't need to prefix with `Model`.</span></span> <span data-ttu-id="dcd71-199">È possibile utilizzare il "@" caratteri per avviare un'espressione inline e spostare prima la `m.`:</span><span class="sxs-lookup"><span data-stu-id="dcd71-199">You can use the "@" character to start an inline expression and move before the `m.`:</span></span>

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

<span data-ttu-id="dcd71-200">Genera quanto segue:</span><span class="sxs-lookup"><span data-stu-id="dcd71-200">Generates the following:</span></span>

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

<span data-ttu-id="dcd71-201">Con le proprietà di raccolta, `asp-for="CollectionProperty[23].Member"` genera lo stesso nome `asp-for="CollectionProperty[i].Member"` quando `i` ha il valore `23`.</span><span class="sxs-lookup"><span data-stu-id="dcd71-201">With collection properties, `asp-for="CollectionProperty[23].Member"` generates the same name as `asp-for="CollectionProperty[i].Member"` when `i` has the value `23`.</span></span>

### <a name="navigating-child-properties"></a><span data-ttu-id="dcd71-202">Proprietà di navigazione figlio</span><span class="sxs-lookup"><span data-stu-id="dcd71-202">Navigating child properties</span></span>

<span data-ttu-id="dcd71-203">È anche possibile accedere alle proprietà figlio utilizzando il percorso della proprietà del modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="dcd71-203">You can also navigate to child properties using the property path of the view model.</span></span> <span data-ttu-id="dcd71-204">Si consideri una classe modello più complessa che contiene un elemento figlio `Address` proprietà.</span><span class="sxs-lookup"><span data-stu-id="dcd71-204">Consider a more complex model class that contains a child `Address` property.</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

<span data-ttu-id="dcd71-205">Nella visualizzazione, è associare `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="dcd71-205">In the view, we bind to `Address.AddressLine1`:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

<span data-ttu-id="dcd71-206">Il codice HTML seguente viene generato per `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="dcd71-206">The following HTML is generated for `Address.AddressLine1`:</span></span>

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a><span data-ttu-id="dcd71-207">I nomi delle espressioni e raccolte</span><span class="sxs-lookup"><span data-stu-id="dcd71-207">Expression names and Collections</span></span>

<span data-ttu-id="dcd71-208">Esempio, un modello contenente una matrice di `Colors`:</span><span class="sxs-lookup"><span data-stu-id="dcd71-208">Sample, a model containing an array of `Colors`:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

<span data-ttu-id="dcd71-209">Il metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="dcd71-209">The action method:</span></span>

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

<span data-ttu-id="dcd71-210">Razor seguente viene illustrata la modalità di accesso di un oggetto specifico `Color` elemento:</span><span class="sxs-lookup"><span data-stu-id="dcd71-210">The following Razor shows how you access a specific `Color` element:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

<span data-ttu-id="dcd71-211">Il *Views/Shared/EditorTemplates/String.cshtml* modello:</span><span class="sxs-lookup"><span data-stu-id="dcd71-211">The *Views/Shared/EditorTemplates/String.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

<span data-ttu-id="dcd71-212">Utilizzo di esempio `List<T>`:</span><span class="sxs-lookup"><span data-stu-id="dcd71-212">Sample using `List<T>`:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

<span data-ttu-id="dcd71-213">Razor seguente viene illustrato come scorrere una raccolta:</span><span class="sxs-lookup"><span data-stu-id="dcd71-213">The following Razor shows how to iterate over a collection:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

<span data-ttu-id="dcd71-214">Il *Views/Shared/EditorTemplates/ToDoItem.cshtml* modello:</span><span class="sxs-lookup"><span data-stu-id="dcd71-214">The *Views/Shared/EditorTemplates/ToDoItem.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
><span data-ttu-id="dcd71-215">Utilizzare sempre `for` (e *non* `foreach`) per eseguire l'iterazione su un elenco.</span><span class="sxs-lookup"><span data-stu-id="dcd71-215">Always use `for` (and *not* `foreach`) to iterate over a list.</span></span> <span data-ttu-id="dcd71-216">La valutazione di un indicizzatore in LINQ espressione può essere costoso e deve essere ridotta a icona.</span><span class="sxs-lookup"><span data-stu-id="dcd71-216">Evaluating an indexer in a LINQ expression can be expensive and should be minimized.</span></span>

&nbsp;

>[!NOTE]
><span data-ttu-id="dcd71-217">Il codice di esempio commentato precedente viene illustrato come sostituire l'espressione lambda con la `@` operatore per accedere a ogni `ToDoItem` nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="dcd71-217">The commented sample code above shows how you would replace the lambda expression with the `@` operator to access each `ToDoItem` in the list.</span></span>

## <a name="the-textarea-tag-helper"></a><span data-ttu-id="dcd71-218">L'Helper di Tag Textarea</span><span class="sxs-lookup"><span data-stu-id="dcd71-218">The Textarea Tag Helper</span></span>

<span data-ttu-id="dcd71-219">Il `Textarea Tag Helper` helper di tag è simile all'Helper di Tag di Input.</span><span class="sxs-lookup"><span data-stu-id="dcd71-219">The `Textarea Tag Helper` tag helper is  similar to the Input Tag Helper.</span></span>

* <span data-ttu-id="dcd71-220">Genera il `id` e `name` gli attributi e gli attributi di convalida dei dati dal modello per un [ \<textarea >](https://www.w3.org/wiki/HTML/Elements/textarea) elemento.</span><span class="sxs-lookup"><span data-stu-id="dcd71-220">Generates the `id` and `name` attributes, and the data validation attributes from the model for a [\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea) element.</span></span>

* <span data-ttu-id="dcd71-221">Fornisce la tipizzazione forte.</span><span class="sxs-lookup"><span data-stu-id="dcd71-221">Provides strong typing.</span></span>

* <span data-ttu-id="dcd71-222">In alternativa HTML Helper:`Html.TextAreaFor`</span><span class="sxs-lookup"><span data-stu-id="dcd71-222">HTML Helper alternative: `Html.TextAreaFor`</span></span>

<span data-ttu-id="dcd71-223">Esempio:</span><span class="sxs-lookup"><span data-stu-id="dcd71-223">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

<span data-ttu-id="dcd71-224">Viene generato il codice HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="dcd71-224">The following HTML is generated:</span></span>

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

## <a name="the-label-tag-helper"></a><span data-ttu-id="dcd71-225">L'Helper di Tag di etichetta</span><span class="sxs-lookup"><span data-stu-id="dcd71-225">The Label Tag Helper</span></span>

* <span data-ttu-id="dcd71-226">Genera la didascalia dell'etichetta e `for` dell'attributo su un [ <label> ](https://www.w3.org/wiki/HTML/Elements/label) elemento per un nome di espressione</span><span class="sxs-lookup"><span data-stu-id="dcd71-226">Generates the label caption and `for` attribute on a [<label>](https://www.w3.org/wiki/HTML/Elements/label) element for an expression name</span></span>

* <span data-ttu-id="dcd71-227">In alternativa HTML Helper: `Html.LabelFor`.</span><span class="sxs-lookup"><span data-stu-id="dcd71-227">HTML Helper alternative: `Html.LabelFor`.</span></span>

<span data-ttu-id="dcd71-228">Il `Label Tag Helper` offre i vantaggi seguenti su un elemento HTML label puro:</span><span class="sxs-lookup"><span data-stu-id="dcd71-228">The `Label Tag Helper`  provides the following benefits over a pure HTML label element:</span></span>

* <span data-ttu-id="dcd71-229">Si ottiene automaticamente il valore di etichetta descrittiva dal `Display` attributo.</span><span class="sxs-lookup"><span data-stu-id="dcd71-229">You automatically get the descriptive label value from the `Display` attribute.</span></span> <span data-ttu-id="dcd71-230">Il nome visualizzato desiderato potrebbe cambiare nel tempo e la combinazione di `Display` attributo e Helper di Tag di etichetta verrà applicata la `Display` ovunque viene utilizzato.</span><span class="sxs-lookup"><span data-stu-id="dcd71-230">The intended display name might change over time, and the combination of `Display` attribute and Label Tag Helper will apply the `Display` everywhere it's used.</span></span>

* <span data-ttu-id="dcd71-231">Meno markup nel codice sorgente</span><span class="sxs-lookup"><span data-stu-id="dcd71-231">Less markup in source code</span></span>

* <span data-ttu-id="dcd71-232">Tipizzazione forte con la proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="dcd71-232">Strong typing with the model property.</span></span>

<span data-ttu-id="dcd71-233">Esempio:</span><span class="sxs-lookup"><span data-stu-id="dcd71-233">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

<span data-ttu-id="dcd71-234">Il codice HTML seguente viene generato per il `<label>` elemento:</span><span class="sxs-lookup"><span data-stu-id="dcd71-234">The following HTML is generated for the `<label>` element:</span></span>

```HTML
<label for="Email">Email Address</label>
```

<span data-ttu-id="dcd71-235">L'Helper di Tag etichetta generato il `for` valore di "Email", che è l'ID dell'attributo è associato il `<input>` elemento.</span><span class="sxs-lookup"><span data-stu-id="dcd71-235">The Label Tag Helper generated the `for` attribute value of "Email", which is the ID associated with the `<input>` element.</span></span> <span data-ttu-id="dcd71-236">Gli helper di Tag generare coerente `id` e `for` elementi affinché possano essere associati correttamente.</span><span class="sxs-lookup"><span data-stu-id="dcd71-236">The Tag Helpers generate consistent `id` and `for` elements so they can be correctly associated.</span></span> <span data-ttu-id="dcd71-237">La didascalia in questo esempio proviene dal `Display` attributo.</span><span class="sxs-lookup"><span data-stu-id="dcd71-237">The caption in this sample comes from the `Display` attribute.</span></span> <span data-ttu-id="dcd71-238">Se il modello non contiene un `Display` attributo, la didascalia sarà il nome della proprietà dell'espressione.</span><span class="sxs-lookup"><span data-stu-id="dcd71-238">If the model didn't contain a `Display` attribute, the caption would be the property name of the expression.</span></span>

## <a name="the-validation-tag-helpers"></a><span data-ttu-id="dcd71-239">Gli helper di Tag di convalida</span><span class="sxs-lookup"><span data-stu-id="dcd71-239">The Validation Tag Helpers</span></span>

<span data-ttu-id="dcd71-240">Sono disponibili due helper di Tag di convalida.</span><span class="sxs-lookup"><span data-stu-id="dcd71-240">There are two Validation Tag Helpers.</span></span> <span data-ttu-id="dcd71-241">Il `Validation Message Tag Helper` (che viene visualizzato un messaggio di convalida per una singola proprietà nel modello) e `Validation Summary Tag Helper` (Visualizza un riepilogo degli errori di convalida).</span><span class="sxs-lookup"><span data-stu-id="dcd71-241">The `Validation Message Tag Helper` (which displays a validation message for a single property on your model), and the `Validation Summary Tag Helper` (which displays a summary of validation errors).</span></span> <span data-ttu-id="dcd71-242">Il `Input Tag Helper` aggiunge attributi di convalida sul lato client HTML5 per gli elementi in base ai dati gli attributi di annotazione sulle classi del modello di input.</span><span class="sxs-lookup"><span data-stu-id="dcd71-242">The `Input Tag Helper` adds HTML5 client side validation attributes to input elements based on data annotation attributes on your model classes.</span></span> <span data-ttu-id="dcd71-243">Viene inoltre eseguita la convalida sul server.</span><span class="sxs-lookup"><span data-stu-id="dcd71-243">Validation is also performed on the server.</span></span> <span data-ttu-id="dcd71-244">L'Helper di Tag di convalida vengono visualizzati questi messaggi di errore quando si verifica un errore di convalida.</span><span class="sxs-lookup"><span data-stu-id="dcd71-244">The Validation Tag Helper displays these error messages when a validation error occurs.</span></span>

### <a name="the-validation-message-tag-helper"></a><span data-ttu-id="dcd71-245">L'Helper di Tag di messaggio di convalida</span><span class="sxs-lookup"><span data-stu-id="dcd71-245">The Validation Message Tag Helper</span></span>

* <span data-ttu-id="dcd71-246">Aggiunge il [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` attributo la [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) elemento, che collega i messaggi di errore di convalida nel campo di input della proprietà del modello specificato.</span><span class="sxs-lookup"><span data-stu-id="dcd71-246">Adds the [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  `data-valmsg-for="property"` attribute to the [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, which attaches the validation error messages on the input field of the specified model property.</span></span> <span data-ttu-id="dcd71-247">Quando si verifica un errore di convalida sul lato client, [jQuery](https://jquery.com/) viene visualizzato il messaggio di errore nel `<span>` elemento.</span><span class="sxs-lookup"><span data-stu-id="dcd71-247">When a client side validation error occurs, [jQuery](https://jquery.com/) displays the error message in the `<span>` element.</span></span>

* <span data-ttu-id="dcd71-248">La convalida avviene anche nel server.</span><span class="sxs-lookup"><span data-stu-id="dcd71-248">Validation also takes place on the server.</span></span> <span data-ttu-id="dcd71-249">I client debba JavaScript disabilitato e una convalida può essere eseguita solo sul lato server.</span><span class="sxs-lookup"><span data-stu-id="dcd71-249">Clients may have JavaScript disabled and some validation can only be done on the server side.</span></span>

* <span data-ttu-id="dcd71-250">In alternativa HTML Helper:`Html.ValidationMessageFor`</span><span class="sxs-lookup"><span data-stu-id="dcd71-250">HTML Helper alternative: `Html.ValidationMessageFor`</span></span>

<span data-ttu-id="dcd71-251">Il `Validation Message Tag Helper` viene utilizzato con il `asp-validation-for` attributo HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) elemento.</span><span class="sxs-lookup"><span data-stu-id="dcd71-251">The `Validation Message Tag Helper`  is used with the `asp-validation-for` attribute on a HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element.</span></span>

```HTML
<span asp-validation-for="Email"></span>
```

<span data-ttu-id="dcd71-252">L'Helper di Tag di messaggio di convalida viene generato il codice HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="dcd71-252">The Validation Message Tag Helper will generate the following HTML:</span></span>

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

<span data-ttu-id="dcd71-253">In genere viene utilizzata la `Validation Message Tag Helper` dopo un `Input` Helper di Tag per la stessa proprietà.</span><span class="sxs-lookup"><span data-stu-id="dcd71-253">You generally use the `Validation Message Tag Helper`  after an `Input` Tag Helper for the same property.</span></span> <span data-ttu-id="dcd71-254">In questo modo consente di visualizzare eventuali messaggi di errore di convalida in prossimità di input che ha causato l'errore.</span><span class="sxs-lookup"><span data-stu-id="dcd71-254">Doing so displays any validation error messages near the input that caused the error.</span></span>

> [!NOTE]
> <span data-ttu-id="dcd71-255">È necessario disporre di una vista con il codice JavaScript corretto e [jQuery](https://jquery.com/) riferimenti agli script sul posto per la convalida lato client.</span><span class="sxs-lookup"><span data-stu-id="dcd71-255">You must have a view with the correct JavaScript and [jQuery](https://jquery.com/) script references in place for client side validation.</span></span> <span data-ttu-id="dcd71-256">Vedere [la convalida del modello](../models/validation.md) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="dcd71-256">See [Model Validation](../models/validation.md) for more information.</span></span>

<span data-ttu-id="dcd71-257">Se (ad esempio quando si utilizza una convalida sul lato server personalizzato o viene disabilitata la convalida lato client), si verifica un errore di convalida sul lato server, MVC inserisce il messaggio di errore come corpo del metodo di `<span>` elemento.</span><span class="sxs-lookup"><span data-stu-id="dcd71-257">When a server side validation error occurs (for example when you have custom server side validation or client-side validation is disabled), MVC places that error message as the body of the `<span>` element.</span></span>

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a><span data-ttu-id="dcd71-258">L'Helper di convalida Summary (tag)</span><span class="sxs-lookup"><span data-stu-id="dcd71-258">The Validation Summary Tag Helper</span></span>

* <span data-ttu-id="dcd71-259">Destinazioni `<div>` gli elementi con il `asp-validation-summary` attributo</span><span class="sxs-lookup"><span data-stu-id="dcd71-259">Targets `<div>` elements with the `asp-validation-summary` attribute</span></span>

* <span data-ttu-id="dcd71-260">In alternativa HTML Helper:`@Html.ValidationSummary`</span><span class="sxs-lookup"><span data-stu-id="dcd71-260">HTML Helper alternative: `@Html.ValidationSummary`</span></span>

<span data-ttu-id="dcd71-261">Il `Validation Summary Tag Helper` viene utilizzato per visualizzare un riepilogo dei messaggi di convalida.</span><span class="sxs-lookup"><span data-stu-id="dcd71-261">The `Validation Summary Tag Helper`  is used to display a summary of validation messages.</span></span> <span data-ttu-id="dcd71-262">Il `asp-validation-summary` valore dell'attributo può essere uno dei seguenti:</span><span class="sxs-lookup"><span data-stu-id="dcd71-262">The `asp-validation-summary` attribute value can be any of the following:</span></span>

|<span data-ttu-id="dcd71-263">riepilogo di convalida ASP</span><span class="sxs-lookup"><span data-stu-id="dcd71-263">asp-validation-summary</span></span>|<span data-ttu-id="dcd71-264">Messaggi di convalida visualizzati</span><span class="sxs-lookup"><span data-stu-id="dcd71-264">Validation messages displayed</span></span>|
|--- |--- |
|<span data-ttu-id="dcd71-265">ValidationSummary.All</span><span class="sxs-lookup"><span data-stu-id="dcd71-265">ValidationSummary.All</span></span>|<span data-ttu-id="dcd71-266">Livello di proprietà e il modello</span><span class="sxs-lookup"><span data-stu-id="dcd71-266">Property and model level</span></span>|
|<span data-ttu-id="dcd71-267">ValidationSummary.ModelOnly</span><span class="sxs-lookup"><span data-stu-id="dcd71-267">ValidationSummary.ModelOnly</span></span>|<span data-ttu-id="dcd71-268">Modello</span><span class="sxs-lookup"><span data-stu-id="dcd71-268">Model</span></span>|
|<span data-ttu-id="dcd71-269">ValidationSummary.None</span><span class="sxs-lookup"><span data-stu-id="dcd71-269">ValidationSummary.None</span></span>|<span data-ttu-id="dcd71-270">Nessuno</span><span class="sxs-lookup"><span data-stu-id="dcd71-270">None</span></span>|

### <a name="sample"></a><span data-ttu-id="dcd71-271">Esempio</span><span class="sxs-lookup"><span data-stu-id="dcd71-271">Sample</span></span>

<span data-ttu-id="dcd71-272">Nell'esempio seguente, il modello di dati è decorato con `DataAnnotation` attributi, che genera i messaggi di errore di convalida per il `<input>` elemento.</span><span class="sxs-lookup"><span data-stu-id="dcd71-272">In the following example, the data model is decorated with `DataAnnotation` attributes, which generates validation error messages on the `<input>` element.</span></span>  <span data-ttu-id="dcd71-273">Quando si verifica un errore di convalida, l'Helper di Tag di convalida viene visualizzato il messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="dcd71-273">When a validation error occurs, the Validation Tag Helper displays the error message:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

<span data-ttu-id="dcd71-274">Il codice HTML generato (quando il modello è valido):</span><span class="sxs-lookup"><span data-stu-id="dcd71-274">The generated HTML (when the model is valid):</span></span>

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid e-mail address."
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

## <a name="the-select-tag-helper"></a><span data-ttu-id="dcd71-275">L'Helper di Tag di selezione</span><span class="sxs-lookup"><span data-stu-id="dcd71-275">The Select Tag Helper</span></span>

* <span data-ttu-id="dcd71-276">Genera l'errore [selezionare](https://www.w3.org/wiki/HTML/Elements/select) e [opzione](https://www.w3.org/wiki/HTML/Elements/option) elementi per le proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="dcd71-276">Generates [select](https://www.w3.org/wiki/HTML/Elements/select) and associated [option](https://www.w3.org/wiki/HTML/Elements/option) elements for properties of your model.</span></span>

* <span data-ttu-id="dcd71-277">È un'alternativa di HTML Helper `Html.DropDownListFor` e`Html.ListBoxFor`</span><span class="sxs-lookup"><span data-stu-id="dcd71-277">Has an HTML Helper alternative `Html.DropDownListFor` and `Html.ListBoxFor`</span></span>

<span data-ttu-id="dcd71-278">Il `Select Tag Helper` `asp-for` specifica il nome della proprietà di modello per il [selezionare](https://www.w3.org/wiki/HTML/Elements/select) elemento e `asp-items` specifica il [opzione](https://www.w3.org/wiki/HTML/Elements/option) elementi.</span><span class="sxs-lookup"><span data-stu-id="dcd71-278">The `Select Tag Helper` `asp-for` specifies the model property  name for the [select](https://www.w3.org/wiki/HTML/Elements/select) element  and `asp-items` specifies the [option](https://www.w3.org/wiki/HTML/Elements/option) elements.</span></span>  <span data-ttu-id="dcd71-279">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="dcd71-279">For example:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

<span data-ttu-id="dcd71-280">Esempio:</span><span class="sxs-lookup"><span data-stu-id="dcd71-280">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

<span data-ttu-id="dcd71-281">Il `Index` metodo inizializza la `CountryViewModel`, imposta il paese selezionato e lo passa al `Index` visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="dcd71-281">The `Index` method initializes the `CountryViewModel`, sets the selected country and passes it to the `Index` view.</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

<span data-ttu-id="dcd71-282">HTTP POST `Index` metodo visualizza la selezione:</span><span class="sxs-lookup"><span data-stu-id="dcd71-282">The HTTP POST `Index` method displays the selection:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

<span data-ttu-id="dcd71-283">Il `Index` Vista:</span><span class="sxs-lookup"><span data-stu-id="dcd71-283">The `Index` view:</span></span>

[!code-cshtml[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

<span data-ttu-id="dcd71-284">Che genera il codice HTML seguente (con "CA" selezionata):</span><span class="sxs-lookup"><span data-stu-id="dcd71-284">Which generates the following HTML (with "CA" selected):</span></span>

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
> <span data-ttu-id="dcd71-285">Non è consigliabile utilizzare `ViewBag` o `ViewData` con l'Helper di Tag selezionare.</span><span class="sxs-lookup"><span data-stu-id="dcd71-285">We do not recommend using `ViewBag` or `ViewData` with the Select Tag Helper.</span></span> <span data-ttu-id="dcd71-286">Un modello di visualizzazione è più affidabile di fornire metadati MVC e in genere meno problematici.</span><span class="sxs-lookup"><span data-stu-id="dcd71-286">A view model is more robust at providing MVC metadata and generally less problematic.</span></span>

<span data-ttu-id="dcd71-287">Il `asp-for` valore dell'attributo è un caso speciale e non richiede un `Model` prefisso, non altri attributi Helper di Tag (ad esempio `asp-items`)</span><span class="sxs-lookup"><span data-stu-id="dcd71-287">The `asp-for` attribute value is a special case and doesn't require a `Model` prefix, the other Tag Helper attributes do (such as `asp-items`)</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a><span data-ttu-id="dcd71-288">Associazione di enum</span><span class="sxs-lookup"><span data-stu-id="dcd71-288">Enum binding</span></span>

<span data-ttu-id="dcd71-289">Spesso è consigliabile usare `<select>` con un `enum` proprietà e generare il `SelectListItem` gli elementi dal `enum` valori.</span><span class="sxs-lookup"><span data-stu-id="dcd71-289">It's often convenient to use `<select>` with an `enum` property and generate the `SelectListItem` elements from the `enum` values.</span></span>

<span data-ttu-id="dcd71-290">Esempio:</span><span class="sxs-lookup"><span data-stu-id="dcd71-290">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

<span data-ttu-id="dcd71-291">Il `GetEnumSelectList` metodo genera un `SelectList` per un enum.</span><span class="sxs-lookup"><span data-stu-id="dcd71-291">The `GetEnumSelectList` method generates a `SelectList` object for an enum.</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

<span data-ttu-id="dcd71-292">È possibile decorare all'elenco di enumeratore con il `Display` attributo per ottenere un'interfaccia utente di più dettagliata:</span><span class="sxs-lookup"><span data-stu-id="dcd71-292">You can decorate your enumerator list with the `Display` attribute to get a richer UI:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

<span data-ttu-id="dcd71-293">Viene generato il codice HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="dcd71-293">The following HTML is generated:</span></span>

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

### <a name="option-group"></a><span data-ttu-id="dcd71-294">Gruppo di opzioni</span><span class="sxs-lookup"><span data-stu-id="dcd71-294">Option Group</span></span>

<span data-ttu-id="dcd71-295">Il codice HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup) elemento viene generato quando il modello di visualizzazione contiene uno o più `SelectListGroup` oggetti.</span><span class="sxs-lookup"><span data-stu-id="dcd71-295">The HTML  [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) element is generated when the view model contains one or more `SelectListGroup` objects.</span></span>

<span data-ttu-id="dcd71-296">Il `CountryViewModelGroup` gruppi di `SelectListItem` elementi nei gruppi "North America" e "Europe":</span><span class="sxs-lookup"><span data-stu-id="dcd71-296">The `CountryViewModelGroup` groups the `SelectListItem` elements into the "North America" and "Europe" groups:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

<span data-ttu-id="dcd71-297">I due gruppi sono illustrati di seguito:</span><span class="sxs-lookup"><span data-stu-id="dcd71-297">The two groups are shown below:</span></span>

![esempio di gruppo di opzione](working-with-forms/_static/grp.png)

<span data-ttu-id="dcd71-299">Il codice HTML generato:</span><span class="sxs-lookup"><span data-stu-id="dcd71-299">The generated HTML:</span></span>

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

### <a name="multiple-select"></a><span data-ttu-id="dcd71-300">Selezione multipla</span><span class="sxs-lookup"><span data-stu-id="dcd71-300">Multiple select</span></span>

<span data-ttu-id="dcd71-301">L'Helper di Tag selezionare genererà automaticamente le [più = "più"](http://w3c.github.io/html-reference/select.html) attributo se la proprietà specificata nel `asp-for` attributo è un `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="dcd71-301">The Select Tag Helper  will automatically generate the [multiple = "multiple"](http://w3c.github.io/html-reference/select.html)  attribute if the property specified in the `asp-for` attribute is an `IEnumerable`.</span></span> <span data-ttu-id="dcd71-302">Ad esempio, dato il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="dcd71-302">For example, given the following model:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

<span data-ttu-id="dcd71-303">Con la vista seguente:</span><span class="sxs-lookup"><span data-stu-id="dcd71-303">With the following view:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

<span data-ttu-id="dcd71-304">Genera il codice HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="dcd71-304">Generates the following HTML:</span></span>

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

### <a name="no-selection"></a><span data-ttu-id="dcd71-305">Nessuna selezione</span><span class="sxs-lookup"><span data-stu-id="dcd71-305">No selection</span></span>

<span data-ttu-id="dcd71-306">Se è necessario utilizzare l'opzione "non è specificato" in più pagine, è possibile creare un modello per eliminare il codice HTML di ripetizione:</span><span class="sxs-lookup"><span data-stu-id="dcd71-306">If you find yourself using the "not specified" option in multiple pages, you can create a template to eliminate repeating the HTML:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

<span data-ttu-id="dcd71-307">Il *Views/Shared/EditorTemplates/CountryViewModel.cshtml* modello:</span><span class="sxs-lookup"><span data-stu-id="dcd71-307">The *Views/Shared/EditorTemplates/CountryViewModel.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

<span data-ttu-id="dcd71-308">Aggiunta di HTML [ \<opzione >](https://www.w3.org/wiki/HTML/Elements/option) elementi non è limitato al *Nessuna selezione* case.</span><span class="sxs-lookup"><span data-stu-id="dcd71-308">Adding HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) elements is not limited to the *No selection* case.</span></span> <span data-ttu-id="dcd71-309">Ad esempio, il metodo di visualizzazione e l'azione seguente genererà HTML simile al codice precedente:</span><span class="sxs-lookup"><span data-stu-id="dcd71-309">For example, the following view and action method will generate HTML similar to the code above:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

<span data-ttu-id="dcd71-310">Il corretto `<option>` verrà selezionato l'elemento (contengono il `selected="selected"` attributo) a seconda di corrente `Country` valore.</span><span class="sxs-lookup"><span data-stu-id="dcd71-310">The correct `<option>` element will be selected ( contain the `selected="selected"` attribute) depending on the current `Country` value.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="dcd71-311">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="dcd71-311">Additional Resources</span></span>

* [<span data-ttu-id="dcd71-312">Helper tag</span><span class="sxs-lookup"><span data-stu-id="dcd71-312">Tag Helpers</span></span>](tag-helpers/intro.md)

* [<span data-ttu-id="dcd71-313">Elemento di HTML Form</span><span class="sxs-lookup"><span data-stu-id="dcd71-313">HTML Form element</span></span>](https://www.w3.org/TR/html401/interact/forms.html)

* [<span data-ttu-id="dcd71-314">Token di richiesta di verifica</span><span class="sxs-lookup"><span data-stu-id="dcd71-314">Request Verification Token</span></span>](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)

* [<span data-ttu-id="dcd71-315">Associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="dcd71-315">Model Binding</span></span>](../models/model-binding.md)

* [<span data-ttu-id="dcd71-316">Convalida del modello</span><span class="sxs-lookup"><span data-stu-id="dcd71-316">Model Validation</span></span>](../models/validation.md)

* [<span data-ttu-id="dcd71-317">annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="dcd71-317">data annotations</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)

* <span data-ttu-id="dcd71-318">[Frammenti per il documento](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample).</span><span class="sxs-lookup"><span data-stu-id="dcd71-318">[Code snippets for this document](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample).</span></span>
