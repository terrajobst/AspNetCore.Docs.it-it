---
title: Convalida del modello in ASP.NET Core MVC
author: tdykstra
description: Informazioni sulla convalida del modello in ASP.NET Core MVC e in Razor Pages.
ms.author: riande
ms.custom: mvc
ms.date: 04/01/2019
uid: mvc/models/validation
ms.openlocfilehash: b766d47f296745ba4be6ea8cb6335db9c3e2d975
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/02/2019
ms.locfileid: "58809315"
---
# <a name="model-validation-in-aspnet-core-mvc-and-razor-pages"></a><span data-ttu-id="0f538-103">Convalida del modello in ASP.NET Core MVC e in Razor Pages</span><span class="sxs-lookup"><span data-stu-id="0f538-103">Model validation in ASP.NET Core MVC and Razor Pages</span></span>

<span data-ttu-id="0f538-104">Questo articolo illustra come convalidare l'input utente in un'app ASP.NET Core MVC o Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0f538-104">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="0f538-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="0f538-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="0f538-106">Stato del modello</span><span class="sxs-lookup"><span data-stu-id="0f538-106">Model state</span></span>

<span data-ttu-id="0f538-107">Lo stato del modello rappresenta gli errori che provengono da due sottosistemi: associazione di modelli e convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="0f538-107">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="0f538-108">Gli errori provenienti dall'[associazione di modelli](model-binding.md) sono in genere errori di conversione dei dati, ad esempio l'immissione di una "x" in un campo in cui è previsto un intero.</span><span class="sxs-lookup"><span data-stu-id="0f538-108">Errors that originate from [model binding](model-binding.md) are generally data conversion errors (for example, an "x" is entered in a field that expects an integer).</span></span> <span data-ttu-id="0f538-109">La convalida del modello è un processo successivo all'associazione di modelli e segnala gli errori in caso di dati non conformi alle regole di business, ad esempio l'immissione del valore 0 in un campo in cui è previsto un valore compreso tra 1 e 5.</span><span class="sxs-lookup"><span data-stu-id="0f538-109">Model validation occurs after model binding and reports errors where the data doesn't conform to business rules (for example, a 0 is entered in a field that expects a rating between 1 and 5).</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0f538-110">Sia l'associazione che la convalida di modelli avviene prima di eseguire un'azione del controller o un metodo gestore di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0f538-110">Both model binding and validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="0f538-111">È responsabilità dell'app esaminare `ModelState.IsValid` e rispondere nel modo appropriato.</span><span class="sxs-lookup"><span data-stu-id="0f538-111">It's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="0f538-112">Le app Web in genere visualizzare di nuovo la pagina con un messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="0f538-112">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/sample_snapshot/Create.cshtml.cs?name=snippet&highlight=3-6)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0f538-113">Sia l'associazione che la convalida di modelli avviene prima di eseguire un'azione del controller o un metodo gestore di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0f538-113">Both model binding and validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="0f538-114">Per le app Web, è responsabilità dell'app esaminare `ModelState.IsValid` e rispondere nel modo appropriato.</span><span class="sxs-lookup"><span data-stu-id="0f538-114">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="0f538-115">Le app Web in genere visualizzare di nuovo la pagina con un messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="0f538-115">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/sample_snapshot/Create.cshtml.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="0f538-116">I controller API Web non devono controllare `ModelState.IsValid` se hanno l'attributo `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="0f538-116">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="0f538-117">In questo caso, se lo stato del modello non è valido, viene restituita una risposta HTTP 400 automatica contenente i dettagli del problema.</span><span class="sxs-lookup"><span data-stu-id="0f538-117">In that case, an automatic HTTP 400 response containing issue details is returned when model state is invalid.</span></span> <span data-ttu-id="0f538-118">Per altre informazioni, vedere [Risposte HTTP 400 automatiche](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="0f538-118">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

::: moniker-end

## <a name="rerun-validation"></a><span data-ttu-id="0f538-119">Rieseguire la convalida</span><span class="sxs-lookup"><span data-stu-id="0f538-119">Rerun validation</span></span>

<span data-ttu-id="0f538-120">La convalida è automatica, ma potrebbe essere necessario ripeterla manualmente.</span><span class="sxs-lookup"><span data-stu-id="0f538-120">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="0f538-121">Ad esempio, è possibile che si calcoli un valore per una proprietà e che si voglia rieseguire la convalida dopo aver impostato la proprietà sul valore calcolato.</span><span class="sxs-lookup"><span data-stu-id="0f538-121">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="0f538-122">Per rieseguire la convalida, chiamare il metodo `TryValidateModel` come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="0f538-122">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/sample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a><span data-ttu-id="0f538-123">Attributi di convalida</span><span class="sxs-lookup"><span data-stu-id="0f538-123">Validation attributes</span></span>

<span data-ttu-id="0f538-124">Gli attributi di convalida consentono di specificare le regole di convalida per le proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="0f538-124">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="0f538-125">Nell'esempio seguente dell'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample) viene illustrata una classe di modello annotata con attributi di convalida.</span><span class="sxs-lookup"><span data-stu-id="0f538-125">The following example from [the sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample) shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="0f538-126">`[ClassicMovie]` è un attributo di convalida personalizzato, mentre gli altri sono attributi predefiniti.</span><span class="sxs-lookup"><span data-stu-id="0f538-126">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="0f538-127">Non viene visualizzato l'attributo `[ClassicMovie2]`, che rappresenta un modo alternativo di implementare un attributo personalizzato.</span><span class="sxs-lookup"><span data-stu-id="0f538-127">(Not shown is `[ClassicMovie2]`, which shows an alternative way to implement a custom attribute.)</span></span>

[!code-csharp[](validation/sample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a><span data-ttu-id="0f538-128">Attributi predefiniti</span><span class="sxs-lookup"><span data-stu-id="0f538-128">Built-in attributes</span></span>

<span data-ttu-id="0f538-129">Di seguito sono elencati alcuni degli attributi di convalida predefiniti:</span><span class="sxs-lookup"><span data-stu-id="0f538-129">Here are some of the built-in validation attributes:</span></span>

* <span data-ttu-id="0f538-130">`[CreditCard]`: convalida che la proprietà abbia un formato di carta di credito.</span><span class="sxs-lookup"><span data-stu-id="0f538-130">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="0f538-131">`[Compare]`: convalida che due proprietà abbiano corrispondenza in un modello.</span><span class="sxs-lookup"><span data-stu-id="0f538-131">`[Compare]`: Validates that two properties in a model match.</span></span>
* <span data-ttu-id="0f538-132">`[EmailAddress]`: convalida che la proprietà abbia un formato di indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="0f538-132">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="0f538-133">`[Phone]`: convalida che la proprietà abbia un formato di numero di telefono.</span><span class="sxs-lookup"><span data-stu-id="0f538-133">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="0f538-134">`[Range]`: convalida che il valore della proprietà rientri in un intervallo specificato.</span><span class="sxs-lookup"><span data-stu-id="0f538-134">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="0f538-135">`[RegularExpression]`: convalida che il valore della proprietà corrisponda a un'espressione regolare specificata.</span><span class="sxs-lookup"><span data-stu-id="0f538-135">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="0f538-136">`[Required]`: convalida che il campo non sia Null.</span><span class="sxs-lookup"><span data-stu-id="0f538-136">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="0f538-137">Vedere l'[attributo [Required]](#required-attribute) per informazioni dettagliate sul comportamento di questo attributo.</span><span class="sxs-lookup"><span data-stu-id="0f538-137">See [[Required] attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="0f538-138">`[StringLength]`: convalida che un valore della proprietà della stringa non superi un limite di lunghezza specificata.</span><span class="sxs-lookup"><span data-stu-id="0f538-138">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="0f538-139">`[Url]`: convalida che la proprietà abbia un formato di URL.</span><span class="sxs-lookup"><span data-stu-id="0f538-139">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="0f538-140">`[Remote]`: convalida l'input nel client chiamando un metodo di azione nel server.</span><span class="sxs-lookup"><span data-stu-id="0f538-140">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="0f538-141">Vedere l'[attributo [Remote]](#remote-attribute) per informazioni dettagliate sul comportamento di questo attributo.</span><span class="sxs-lookup"><span data-stu-id="0f538-141">See [[Remote] attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="0f538-142">Nello spazio dei nomi [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) è possibile trovare un elenco completo degli attributi di convalida.</span><span class="sxs-lookup"><span data-stu-id="0f538-142">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="0f538-143">Messaggi di errore</span><span class="sxs-lookup"><span data-stu-id="0f538-143">Error messages</span></span>

<span data-ttu-id="0f538-144">Gli attributi di convalida consentono di specificare il messaggio di errore da visualizzare in caso di input non valido.</span><span class="sxs-lookup"><span data-stu-id="0f538-144">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="0f538-145">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="0f538-145">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="0f538-146">Internamente gli attributi chiamano `String.Format` con un segnaposto per il nome campo e talvolta con segnaposto aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="0f538-146">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="0f538-147">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="0f538-147">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="0f538-148">Quando applicato a una proprietà `Name`, il messaggio di errore creato con il codice precedente sarà "La lunghezza del nome utente deve essere compresa tra 6 e 8".</span><span class="sxs-lookup"><span data-stu-id="0f538-148">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="0f538-149">Per scoprire i parametri passati a `String.Format` per un messaggio di errore dell'attributo specifico, vedere il [codice sorgente DataAnnotations](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="0f538-149">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="0f538-150">Attributo [Required]</span><span class="sxs-lookup"><span data-stu-id="0f538-150">[Required] attribute</span></span>

<span data-ttu-id="0f538-151">Per impostazione predefinita, il sistema di convalida considera i parametri non nullable o le proprietà come se avessero un attributo `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="0f538-151">By default, the validation system treats non-nullable parameters or properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="0f538-152">I [tipi di valore](/dotnet/csharp/language-reference/keywords/value-types) `decimal` e `int` sono parametri non nullable.</span><span class="sxs-lookup"><span data-stu-id="0f538-152">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="0f538-153">Convalida dell'attributo [Required] nel server</span><span class="sxs-lookup"><span data-stu-id="0f538-153">[Required] validation on the server</span></span>

<span data-ttu-id="0f538-154">Nel server un valore obbligatorio viene considerato mancante se la proprietà è Null.</span><span class="sxs-lookup"><span data-stu-id="0f538-154">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="0f538-155">Un campo non nullable è sempre valido e il messaggio di errore dell'attributo [Required] non viene mai visualizzato.</span><span class="sxs-lookup"><span data-stu-id="0f538-155">A non-nullable field is always valid, and the [Required] attribute's error message is never displayed.</span></span>

<span data-ttu-id="0f538-156">Può però succedere che l'associazione di modelli per una proprietà non nullable abbia esito negativo, generando un messaggio di errore, ad esempio `The value '' is invalid`.</span><span class="sxs-lookup"><span data-stu-id="0f538-156">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="0f538-157">Per specificare un messaggio di errore personalizzato per la convalida lato server di tipi non nullable, sono disponibili le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="0f538-157">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="0f538-158">Il campo deve ammettere i valori Null, ad esempio `decimal?` anziché `decimal`.</span><span class="sxs-lookup"><span data-stu-id="0f538-158">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="0f538-159">I tipi di valore [Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) vengono considerati tipi nullable standard.</span><span class="sxs-lookup"><span data-stu-id="0f538-159">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="0f538-160">Specificare il messaggio di errore predefinito che l'associazione di modelli deve usare, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="0f538-160">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  <span data-ttu-id="0f538-161">Per altre informazioni sugli errori dell'associazione di modelli che possono essere impostati come messaggi predefiniti, vedere <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span><span class="sxs-lookup"><span data-stu-id="0f538-161">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="0f538-162">Convalida dell'attributo [Required] nel client</span><span class="sxs-lookup"><span data-stu-id="0f538-162">[Required] validation on the client</span></span>

<span data-ttu-id="0f538-163">Le stringhe e i tipi non nullable sono gestiti in modo diverso nel client rispetto a come vengono gestiti nel server.</span><span class="sxs-lookup"><span data-stu-id="0f538-163">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="0f538-164">Nel client:</span><span class="sxs-lookup"><span data-stu-id="0f538-164">On the client:</span></span>

* <span data-ttu-id="0f538-165">Un valore viene considerato presente solo viene immesso un input per tale valore.</span><span class="sxs-lookup"><span data-stu-id="0f538-165">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="0f538-166">La convalida lato client gestisce quindi i tipi non nullable come i tipi nullable.</span><span class="sxs-lookup"><span data-stu-id="0f538-166">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="0f538-167">Lo spazio vuoto in un campo stringa viene considerato un input valido per il metodo [required](https://jqueryvalidation.org/required-method/) della convalida di jQuery.</span><span class="sxs-lookup"><span data-stu-id="0f538-167">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="0f538-168">La convalida lato server considera un campo stringa obbligatorio non valido solo se viene immesso uno spazio vuoto.</span><span class="sxs-lookup"><span data-stu-id="0f538-168">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="0f538-169">Come indicato in precedenza, i tipi non nullable vengono trattati come se avessero un attributo `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="0f538-169">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="0f538-170">Quindi viene eseguita la convalida lato client anche se non si applica l'attributo `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="0f538-170">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="0f538-171">Se invece non si usa l'attributo, viene visualizzato un messaggio di errore predefinito.</span><span class="sxs-lookup"><span data-stu-id="0f538-171">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="0f538-172">Per specificare un messaggio di errore personalizzato, usare l'attributo.</span><span class="sxs-lookup"><span data-stu-id="0f538-172">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="0f538-173">Attributo [Remote]</span><span class="sxs-lookup"><span data-stu-id="0f538-173">[Remote] attribute</span></span>

<span data-ttu-id="0f538-174">L'attributo `[Remote]` implementa la convalida lato client che richiede la chiamata a un metodo nel server per determinare se l'input del campo è valido.</span><span class="sxs-lookup"><span data-stu-id="0f538-174">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="0f538-175">Ad esempio, l'app potrebbe dover verificare se un nome utente è già in uso.</span><span class="sxs-lookup"><span data-stu-id="0f538-175">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="0f538-176">Per implementare la convalida remota:</span><span class="sxs-lookup"><span data-stu-id="0f538-176">To implement remote validation:</span></span>

1. <span data-ttu-id="0f538-177">Creare un metodo di azione per JavaScript da chiamare.</span><span class="sxs-lookup"><span data-stu-id="0f538-177">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="0f538-178">Il metodo [remote](https://jqueryvalidation.org/remote-method/) jQuery Validate prevede una risposta JSON:</span><span class="sxs-lookup"><span data-stu-id="0f538-178">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="0f538-179">`"true"` indica che i dati di input sono validi.</span><span class="sxs-lookup"><span data-stu-id="0f538-179">`"true"` means the input data is valid.</span></span>
   * <span data-ttu-id="0f538-180">`"false"`, `undefined` o `null` indica che l'input non è valido.</span><span class="sxs-lookup"><span data-stu-id="0f538-180">`"false"`, `undefined`, or `null` means the input is invalid.</span></span>  <span data-ttu-id="0f538-181">Visualizzare il messaggio di errore predefinito.</span><span class="sxs-lookup"><span data-stu-id="0f538-181">Display the default error message.</span></span>
   * <span data-ttu-id="0f538-182">Qualsiasi altra stringa indica che l'input non è valido.</span><span class="sxs-lookup"><span data-stu-id="0f538-182">Any other string means the input is invalid.</span></span> <span data-ttu-id="0f538-183">Visualizzare la stringa come messaggio di errore personalizzato.</span><span class="sxs-lookup"><span data-stu-id="0f538-183">Display the string as a custom error message.</span></span>

   <span data-ttu-id="0f538-184">Di seguito è riportato un esempio di un metodo di azione che restituisce un messaggio di errore personalizzato:</span><span class="sxs-lookup"><span data-stu-id="0f538-184">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="0f538-185">Nella classe del modello annotare la proprietà con un attributo `[Remote]` che punta al metodo di azione di convalida, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="0f538-185">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserEmailProperty)]

### <a name="additional-fields"></a><span data-ttu-id="0f538-186">Campi aggiuntivi</span><span class="sxs-lookup"><span data-stu-id="0f538-186">Additional fields</span></span>

<span data-ttu-id="0f538-187">La proprietà `[Remote]` dell'attributo `AdditionalFields` consente di convalidare combinazioni di campi rispetto ai dati nel server.</span><span class="sxs-lookup"><span data-stu-id="0f538-187">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="0f538-188">Ad esempio, se il modello `User` avesse le proprietà `FirstName` e `LastName`, potrebbe essere necessario controllare che non siano già esistenti utenti con la stessa coppia di nomi.</span><span class="sxs-lookup"><span data-stu-id="0f538-188">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="0f538-189">Nell'esempio riportato di seguito viene illustrato come usare `AdditionalFields`:</span><span class="sxs-lookup"><span data-stu-id="0f538-189">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserNameProperties)]

<span data-ttu-id="0f538-190">È stato possibile impostare in modo esplicito `AdditionalFields` sulle stringhe `"FirstName"` e `"LastName"`, ma usando l'operatore [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) il successivo refactoring risulta semplificato.</span><span class="sxs-lookup"><span data-stu-id="0f538-190">`AdditionalFields` could be set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="0f538-191">Il metodo di azione per la convalida deve accettare gli argomenti sia per il nome e sia per il cognome:</span><span class="sxs-lookup"><span data-stu-id="0f538-191">The action method for this validation must accept both first name and last name arguments:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="0f538-192">Quando si immette un nome o un cognome, JavaScript esegue una chiamata remota per verificare se tale coppia di nomi è in uso.</span><span class="sxs-lookup"><span data-stu-id="0f538-192">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="0f538-193">Per convalidare due o più campi aggiuntivi, specificarli sotto forma di elenco delimitato da virgole.</span><span class="sxs-lookup"><span data-stu-id="0f538-193">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="0f538-194">Ad esempio, per aggiungere una proprietà `MiddleName` al modello, impostare l'attributo `[Remote]` come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="0f538-194">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="0f538-195">`AdditionalFields`, come tutti gli argomenti dell'attributo, deve essere un'espressione costante.</span><span class="sxs-lookup"><span data-stu-id="0f538-195">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="0f538-196">Non usare quindi una [stringa interpolata](/dotnet/csharp/language-reference/keywords/interpolated-strings) oppure chiamare <xref:System.String.Join*> per inizializzare `AdditionalFields`.</span><span class="sxs-lookup"><span data-stu-id="0f538-196">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="0f538-197">Alternative agli attributi predefiniti</span><span class="sxs-lookup"><span data-stu-id="0f538-197">Alternatives to built-in attributes</span></span>

<span data-ttu-id="0f538-198">Se si vuole usare un tipo di convalida non definita da attributi predefiniti, è possibile eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="0f538-198">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="0f538-199">[Creare attributi personalizzati](#custom-attributes).</span><span class="sxs-lookup"><span data-stu-id="0f538-199">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="0f538-200">[Implementare IValidatableObject](#ivalidatableobject).</span><span class="sxs-lookup"><span data-stu-id="0f538-200">[Implement IValidatableObject](#ivalidatableobject).</span></span>


## <a name="custom-attributes"></a><span data-ttu-id="0f538-201">Attributi personalizzati</span><span class="sxs-lookup"><span data-stu-id="0f538-201">Custom attributes</span></span>

<span data-ttu-id="0f538-202">Per gli scenari non gestiti dagli attributi di convalida predefiniti, è possibile creare attributi di convalida personalizzati.</span><span class="sxs-lookup"><span data-stu-id="0f538-202">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="0f538-203">Creare una classe che eredita da <xref:System.ComponentModel.DataAnnotations.ValidationAttribute> ed eseguire l'override del metodo <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*>.</span><span class="sxs-lookup"><span data-stu-id="0f538-203">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="0f538-204">Il metodo `IsValid` accetta un oggetto denominato *value*, ovvero l'input da convalidare.</span><span class="sxs-lookup"><span data-stu-id="0f538-204">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="0f538-205">Un overload accetta anche un oggetto `ValidationContext`, che contiene informazioni aggiuntive, ad esempio l'istanza del modello creato dall'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="0f538-205">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="0f538-206">Nell'esempio seguente si convalida che la data di uscita di un film di genere *Classic* non sia successiva a un anno specifico.</span><span class="sxs-lookup"><span data-stu-id="0f538-206">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="0f538-207">L'attributo `[ClassicMovie2]` prima controlla il genere, poi prosegue solo se il genere è *Classic*.</span><span class="sxs-lookup"><span data-stu-id="0f538-207">The `[ClassicMovie2]` attribute checks the genre first and continues only if it's *Classic*.</span></span> <span data-ttu-id="0f538-208">Per i film identificati come classici, controlla la data di uscita per verificare che non sia successiva al limite passato al costruttore dell'attributo.</span><span class="sxs-lookup"><span data-stu-id="0f538-208">For movies identified as classics, it checks the release date to make sure it's not later than the limit passed to the attribute constructor.)</span></span>

[!code-csharp[](validation/sample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

<span data-ttu-id="0f538-209">La variabile `movie` nell'esempio precedente rappresenta un oggetto `Movie` contenente i dati dell'invio del modulo.</span><span class="sxs-lookup"><span data-stu-id="0f538-209">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="0f538-210">Il metodo `IsValid` controlla la data e il genere.</span><span class="sxs-lookup"><span data-stu-id="0f538-210">The `IsValid` method checks the date and genre.</span></span> <span data-ttu-id="0f538-211">Se la convalida ha esito positivo, `IsValid` restituisce un codice `ValidationResult.Success`.</span><span class="sxs-lookup"><span data-stu-id="0f538-211">Upon successful validation, `IsValid` returns a `ValidationResult.Success` code.</span></span> <span data-ttu-id="0f538-212">Se la convalida ha esito negativo, viene restituito un codice `ValidationResult` con un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="0f538-212">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="0f538-213">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="0f538-213">IValidatableObject</span></span>

<span data-ttu-id="0f538-214">L'esempio precedente usa solo tipi `Movie`.</span><span class="sxs-lookup"><span data-stu-id="0f538-214">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="0f538-215">Un'altra opzione per la convalida a livello di classe consiste nell'implementare `IValidatableObject` nella classe del modello, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="0f538-215">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/sample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

::: moniker range=">= aspnetcore-2.1"

## <a name="top-level-node-validation"></a><span data-ttu-id="0f538-216">Convalida del nodo di primo livello</span><span class="sxs-lookup"><span data-stu-id="0f538-216">Top-level node validation</span></span>

<span data-ttu-id="0f538-217">I nodi di primo livello includono:</span><span class="sxs-lookup"><span data-stu-id="0f538-217">Top-level nodes include:</span></span>

* <span data-ttu-id="0f538-218">Parametri di azione</span><span class="sxs-lookup"><span data-stu-id="0f538-218">Action parameters</span></span>
* <span data-ttu-id="0f538-219">Proprietà del controller</span><span class="sxs-lookup"><span data-stu-id="0f538-219">Controller properties</span></span>
* <span data-ttu-id="0f538-220">Parametri del gestore di pagina</span><span class="sxs-lookup"><span data-stu-id="0f538-220">Page handler parameters</span></span>
* <span data-ttu-id="0f538-221">Proprietà del modello di pagina</span><span class="sxs-lookup"><span data-stu-id="0f538-221">Page model properties</span></span>

<span data-ttu-id="0f538-222">Vengono convalidati i nodi di primo livello associati al modello, oltre a convalidare le proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="0f538-222">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="0f538-223">Nell'esempio seguente dell'app di esempio il metodo `VerifyPhone` usa <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> per convalidare il parametro di azione `phone`:</span><span class="sxs-lookup"><span data-stu-id="0f538-223">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="0f538-224">I nodi di primo livello possono usare <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> con gli attributi di convalida.</span><span class="sxs-lookup"><span data-stu-id="0f538-224">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="0f538-225">Nell'esempio seguente dall'app di esempio, il metodo `CheckAge` specifica che il parametro `age` deve essere associato dalla stringa di query quando viene inviato il modulo:</span><span class="sxs-lookup"><span data-stu-id="0f538-225">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_CheckAge)]

<span data-ttu-id="0f538-226">La pagina Check Age (*CheckAge.cshtml*), include due moduli.</span><span class="sxs-lookup"><span data-stu-id="0f538-226">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="0f538-227">Il primo modulo invia un valore `Age` `99` come stringa di query: `https://localhost:5001/Users/CheckAge?Age=99`.</span><span class="sxs-lookup"><span data-stu-id="0f538-227">The first form submits an `Age` value of `99` as a query string: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="0f538-228">Quando viene inviato un parametro correttamente formattato `age` dalla stringa di query, il modulo viene convalidato.</span><span class="sxs-lookup"><span data-stu-id="0f538-228">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="0f538-229">Il secondo modulo nella pagina Check Age invia il valore `Age` nel corpo della richiesta e la convalida ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="0f538-229">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="0f538-230">L'associazione non riesce perché il parametro `age` deve provenire da una stringa di query.</span><span class="sxs-lookup"><span data-stu-id="0f538-230">Binding fails because the `age` parameter must come from a query string.</span></span>

<span data-ttu-id="0f538-231">Quando viene eseguita con `CompatibilityVersion.Version_2_1` o versione successiva, la convalida del nodo di primo livello è abilitata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="0f538-231">When running with `CompatibilityVersion.Version_2_1` or later, top-level node validation is enabled by default.</span></span> <span data-ttu-id="0f538-232">In caso contrario, la convalida del nodo di primo livello è disabilitata.</span><span class="sxs-lookup"><span data-stu-id="0f538-232">Otherwise, top-level node validation is disabled.</span></span> <span data-ttu-id="0f538-233">L'opzione predefinita può essere sovrascritta impostando la proprietà <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> in (`Startup.ConfigureServices`), come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="0f538-233">The default option can be overridden by setting the <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> property in (`Startup.ConfigureServices`), as shown here:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup.cs?name=snippet_AddMvc&highlight=4)]

::: moniker-end

## <a name="maximum-errors"></a><span data-ttu-id="0f538-234">Numero massimo di errori</span><span class="sxs-lookup"><span data-stu-id="0f538-234">Maximum errors</span></span>

<span data-ttu-id="0f538-235">La convalida si interrompe quando viene raggiunto il numero di errori (200 per impostazione predefinita).</span><span class="sxs-lookup"><span data-stu-id="0f538-235">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="0f538-236">È possibile configurare questo numero con il codice seguente in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0f538-236">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

::: moniker range=">= aspnetcore-2.1"

## <a name="maximum-recursion"></a><span data-ttu-id="0f538-237">Numero massimo di ricorsioni</span><span class="sxs-lookup"><span data-stu-id="0f538-237">Maximum recursion</span></span>

<span data-ttu-id="0f538-238"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> attraversare l'oggetto grafico del modello che deve essere convalidato.</span><span class="sxs-lookup"><span data-stu-id="0f538-238"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="0f538-239">Per i modelli molti profondi o ricorsivi all'infinito, la convalida può generare un overflow dello stack.</span><span class="sxs-lookup"><span data-stu-id="0f538-239">For models that are very deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="0f538-240">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) consente di arrestare tempestivamente la convalida se la ricorsione del visitatore supera la profondità configurata.</span><span class="sxs-lookup"><span data-stu-id="0f538-240">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="0f538-241">Il valore predefinito di `MvcOptions.MaxValidationDepth` è 200 quando la convalida viene eseguita con `CompatibilityVersion.Version_2_2` o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="0f538-241">The default value of `MvcOptions.MaxValidationDepth` is 200 when running with `CompatibilityVersion.Version_2_2` or later.</span></span> <span data-ttu-id="0f538-242">Per le versioni precedenti, il valore è Null, vale a dire che non esistono limiti di profondità.</span><span class="sxs-lookup"><span data-stu-id="0f538-242">For earlier versions, the value is null, which means no depth constraint.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="0f538-243">Corto circuito automatico</span><span class="sxs-lookup"><span data-stu-id="0f538-243">Automatic short-circuit</span></span>

<span data-ttu-id="0f538-244">La convalida viene ignorata automaticamente (corto circuito) se l'oggetto grafico non richiede la convalida.</span><span class="sxs-lookup"><span data-stu-id="0f538-244">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="0f538-245">Gli oggetti per cui viene ignorata la convalida sono le raccolte di primitive (ad esempio `byte[]`, `string[]`, `Dictionary<string, string>`) e gli oggetti grafici complessi che non hanno validator.</span><span class="sxs-lookup"><span data-stu-id="0f538-245">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

::: moniker-end

## <a name="disable-validation"></a><span data-ttu-id="0f538-246">Disabilitare la convalida</span><span class="sxs-lookup"><span data-stu-id="0f538-246">Disable validation</span></span>

<span data-ttu-id="0f538-247">Per disabilitare la convalida:</span><span class="sxs-lookup"><span data-stu-id="0f538-247">To disable validation:</span></span>

1. <span data-ttu-id="0f538-248">Creare un'implementazione di `IObjectModelValidator` che non contrassegna i campi come non validi.</span><span class="sxs-lookup"><span data-stu-id="0f538-248">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/sample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. <span data-ttu-id="0f538-249">Aggiungere il codice seguente a `Startup.ConfigureServices` per sostituire l'impostazione `IObjectModelValidator` predefinita nel contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="0f538-249">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="0f538-250">Potrebbero essere visualizzati errori di stato del modello generati dall'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="0f538-250">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="0f538-251">Convalida lato client</span><span class="sxs-lookup"><span data-stu-id="0f538-251">Client-side validation</span></span>

<span data-ttu-id="0f538-252">La convalida lato client non consente l'invio del modulo finché non è ritenuto valido.</span><span class="sxs-lookup"><span data-stu-id="0f538-252">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="0f538-253">Tramite il pulsante Invia viene eseguito JavaScript, che invia il modulo oppure visualizza i messaggi di errore.</span><span class="sxs-lookup"><span data-stu-id="0f538-253">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="0f538-254">La convalida lato client evitare un inutile round trip al server in caso di errori di input in un modulo.</span><span class="sxs-lookup"><span data-stu-id="0f538-254">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="0f538-255">I riferimenti dello script seguenti in *_Layout.cshtml* e *_ValidationScriptsPartial.cshtml* supportano la convalida lato client:</span><span class="sxs-lookup"><span data-stu-id="0f538-255">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

<span data-ttu-id="0f538-256">Lo script [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) è una libreria front-end Microsoft personalizzata che si basa sul noto plug-in [jQuery Validate](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="0f538-256">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="0f538-257">Senza Query Unobtrusive Validation, sarebbe necessario scrivere il codice della stessa logica di convalida in due posizioni, vale a dire negli attributi di convalida lato server nelle proprietà del modello e nuovamente negli script lato client.</span><span class="sxs-lookup"><span data-stu-id="0f538-257">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="0f538-258">Invece, gli [helper tag](xref:mvc/views/tag-helpers/intro)e agli [helper HTML](xref:mvc/views/overview) usano gli attributi di convalida e i metadati di tipo delle proprietà del modello per eseguire il rendering degli attributi `data-` HTML 5 negli elementi del modulo che devono essere convalidati.</span><span class="sxs-lookup"><span data-stu-id="0f538-258">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="0f538-259">jQuery Unobtrusive Validation analizza gli attributi `data-` e passa la logica a jQuery Validate, "copiando" in modo efficace la logica di convalida lato server nel client.</span><span class="sxs-lookup"><span data-stu-id="0f538-259">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="0f538-260">È possibile visualizzare gli errori di convalida nel client tramite gli helper tag, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="0f538-260">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

<span data-ttu-id="0f538-261">Gli helper tag precedenti eseguono il rendering del codice HTML seguente.</span><span class="sxs-lookup"><span data-stu-id="0f538-261">The preceding tag helpers render the following HTML.</span></span>

```html
<form action="/Movies/Create" method="post">
    <div class="form-horizontal">
        <h4>Movie</h4>
        <div class="text-danger"></div>
        <div class="form-group">
            <label class="col-md-2 control-label" for="ReleaseDate">ReleaseDate</label>
            <div class="col-md-10">
                <input class="form-control" type="datetime"
                data-val="true" data-val-required="The ReleaseDate field is required."
                id="ReleaseDate" name="ReleaseDate" value="" />
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

<span data-ttu-id="0f538-262">Si noti che gli attributi `data-` nell'output HTML corrispondono agli attributi di convalida per la proprietà `ReleaseDate`.</span><span class="sxs-lookup"><span data-stu-id="0f538-262">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="0f538-263">L'attributo `data-val-required` contiene un messaggio di errore che viene visualizzato se l'utente non compila il campo relativo alla data di uscita.</span><span class="sxs-lookup"><span data-stu-id="0f538-263">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="0f538-264">Lo script jQuery Unobtrusive Validation passa questo valore al metodo [`required()`](https://jqueryvalidation.org/required-method/) jQuery Validate, che visualizza il messaggio nell'elemento **\<span>** di accompagnamento.</span><span class="sxs-lookup"><span data-stu-id="0f538-264">jQuery Unobtrusive Validation passes this value to the jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="0f538-265">La convalida del tipo di dati si basa sul tipo .NET di una proprietà, a meno che sia sostituito dall'attributo `[DataType]`.</span><span class="sxs-lookup"><span data-stu-id="0f538-265">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="0f538-266">I browser generano messaggi di errore predefiniti propri che il pacchetto jQuery Validation Unobtrusive Validation può comunque sostituire.</span><span class="sxs-lookup"><span data-stu-id="0f538-266">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="0f538-267">Gli attributi `[DataType]` e le sottoclassi, ad esempio `[EmailAddress]`, consentono di specificare il messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="0f538-267">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="0f538-268">Aggiungere la convalida a moduli dinamici</span><span class="sxs-lookup"><span data-stu-id="0f538-268">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="0f538-269">Quando la pagina viene caricata, jQuery Unobtrusive Validation passa la logica e i parametri di convalida a jQuery Validate.</span><span class="sxs-lookup"><span data-stu-id="0f538-269">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="0f538-270">La convalida non viene quindi eseguita automaticamente nei moduli generati in modo dinamico.</span><span class="sxs-lookup"><span data-stu-id="0f538-270">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="0f538-271">Per abilitare la convalida, chiedere a jQuery Unobtrusive Validation di analizzare il modulo dinamico immediatamente dopo essere stato creato.</span><span class="sxs-lookup"><span data-stu-id="0f538-271">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="0f538-272">Il codice riportato di seguito configura ad esempio la convalida lato client in un modulo che è stato aggiunto tramite AJAX.</span><span class="sxs-lookup"><span data-stu-id="0f538-272">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

<span data-ttu-id="0f538-273">Il metodo `$.validator.unobtrusive.parse()` accetta un selettore jQuery per ogni argomento.</span><span class="sxs-lookup"><span data-stu-id="0f538-273">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="0f538-274">Questo metodo indica a jQuery Unobtrusive Validation di analizzare gli attributi `data-` dei moduli all'interno di tale tipo di selettore.</span><span class="sxs-lookup"><span data-stu-id="0f538-274">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="0f538-275">I valori di tali attributi vengono quindi passati al plug-in jQuery Validate.</span><span class="sxs-lookup"><span data-stu-id="0f538-275">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="0f538-276">Aggiungere la convalida a controlli dinamici</span><span class="sxs-lookup"><span data-stu-id="0f538-276">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="0f538-277">Il metodo `$.validator.unobtrusive.parse()` funziona su un intero modulo e non sui singoli controlli generati dinamicamente, ad esempio `<input/>` e `<select/>`.</span><span class="sxs-lookup"><span data-stu-id="0f538-277">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input/>` and `<select/>`.</span></span> <span data-ttu-id="0f538-278">Per eseguire il reparse del modulo, rimuovere i dati di convalida aggiunti quando il modulo è stato precedentemente analizzato, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="0f538-278">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add control. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        $(form).removeData("validator")    // Added by jQuery Validate
               .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="custom-client-side-validation"></a><span data-ttu-id="0f538-279">Convalida lato client personalizzata</span><span class="sxs-lookup"><span data-stu-id="0f538-279">Custom client-side validation</span></span>

<span data-ttu-id="0f538-280">La convalida lato client personalizzata viene eseguita generando attributi `data-` HTML che vengono usati con un adapter jQuery Validate personalizzato.</span><span class="sxs-lookup"><span data-stu-id="0f538-280">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="0f538-281">Il codice dell'adapter di esempio seguente è stato scritto per gli attributi `ClassicMovie` e `ClassicMovie2` presentati precedentemente nell'articolo:</span><span class="sxs-lookup"><span data-stu-id="0f538-281">The following sample adapter code was written for the `ClassicMovie` and `ClassicMovie2` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/sample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

<span data-ttu-id="0f538-282">Per informazioni sulla scrittura degli adapter, vedere la [documentazione di jQuery Validate](http://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="0f538-282">For information about how to write adapters, see the [jQuery Validate documentation](http://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="0f538-283">L'uso di un adapter per un determinato campo viene attivato dagli attributi `data-`, i quali eseguono le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="0f538-283">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="0f538-284">Contrassegnano il campo come campo da sottoporre a convalida (`data-val="true"`).</span><span class="sxs-lookup"><span data-stu-id="0f538-284">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="0f538-285">Identificano un nome della regola di convalida e un testo del messaggio di errore, ad esempio `data-val-rulename="Error message."`.</span><span class="sxs-lookup"><span data-stu-id="0f538-285">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="0f538-286">Specificare i parametri aggiuntivi necessari al validator, ad esempio `data-val-rulename-parm1="value"`.</span><span class="sxs-lookup"><span data-stu-id="0f538-286">Provide any additional parameters the validator needs (for example, `data-val-rulename-parm1="value"`).</span></span>

<span data-ttu-id="0f538-287">L'esempio seguente contiene gli attributi `data-` per l'attributo `ClassicMovie` dell'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="0f538-287">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

<span data-ttu-id="0f538-288">Come già accennato in precedenza, gli [helper tag](xref:mvc/views/tag-helpers/intro) e gli [helper HTML](xref:mvc/views/overview) usano le informazioni degli attributi di convalida per eseguire il rendering degli attributi `data-`.</span><span class="sxs-lookup"><span data-stu-id="0f538-288">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="0f538-289">Per scrivere il codice che crea attributi HTML `data-` personalizzati, sono disponibili due opzioni:</span><span class="sxs-lookup"><span data-stu-id="0f538-289">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="0f538-290">Creare una classe che deriva da `AttributeAdapterBase<TAttribute>` e una classe che implementa `IValidationAttributeAdapterProvider` e registrare l'attributo e il relativo adapter nell'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="0f538-290">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="0f538-291">Questo metodo segue il [principio di singola responsabilità](https://wikipedia.org/wiki/Single_responsibility_principle) in quanto il codice di convalida relativo a server e client si trova in classi separate.</span><span class="sxs-lookup"><span data-stu-id="0f538-291">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="0f538-292">L'adapter offre anche un altro vantaggio. Essendo registrato nell'inserimento delle dipendenze, può usare gli altri servizi disponibili nell'inserimento delle dipendenze se necessario.</span><span class="sxs-lookup"><span data-stu-id="0f538-292">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="0f538-293">Implementare `IClientModelValidator` nella classe `ValidationAttribute`.</span><span class="sxs-lookup"><span data-stu-id="0f538-293">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="0f538-294">Questo metodo potrebbe essere appropriato se l'attributo non esegue la convalida lato server e non richiede altri servizi dell'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="0f538-294">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="0f538-295">AttributeAdapter per la convalida lato client</span><span class="sxs-lookup"><span data-stu-id="0f538-295">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="0f538-296">Questo metodo di rendering degli attributi `data-` in HTML viene usato dall'attributo `ClassicMovie` nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="0f538-296">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="0f538-297">Per aggiungere la convalida lato client usando questo metodo:</span><span class="sxs-lookup"><span data-stu-id="0f538-297">To add client validation by using this method:</span></span>

1. <span data-ttu-id="0f538-298">Creare una classe di adapter dell'attributo per l'attributo di convalida personalizzata.</span><span class="sxs-lookup"><span data-stu-id="0f538-298">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="0f538-299">Derivare la classe da [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="0f538-299">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="0f538-300">Creare un metodo `AddValidation` che aggiunge gli attributi `data-` all'output sottoposto a rendering, come illustrato in questo esempio:</span><span class="sxs-lookup"><span data-stu-id="0f538-300">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. <span data-ttu-id="0f538-301">Creare una classe di provider dell'adapter che implementa <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span><span class="sxs-lookup"><span data-stu-id="0f538-301">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="0f538-302">Nel metodo `GetAttributeAdapter` passare l'attributo personalizzato al costruttore dell'adapter, come illustrato in questo esempio:</span><span class="sxs-lookup"><span data-stu-id="0f538-302">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. <span data-ttu-id="0f538-303">Registrare il provider dell'adapter per l'inserimento delle dipendenze in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0f538-303">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="0f538-304">IClientModelValidator per la convalida lato client</span><span class="sxs-lookup"><span data-stu-id="0f538-304">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="0f538-305">Questo metodo di rendering degli attributi `data-` in HTML viene usato dall'attributo `ClassicMovie2` nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="0f538-305">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie2` attribute in the sample app.</span></span> <span data-ttu-id="0f538-306">Per aggiungere la convalida lato client usando questo metodo:</span><span class="sxs-lookup"><span data-stu-id="0f538-306">To add client validation by using this method:</span></span>

* <span data-ttu-id="0f538-307">Nell'attributo di convalida personalizzato implementare l'interfaccia `IClientModelValidator` e creare un metodo `AddValidation`.</span><span class="sxs-lookup"><span data-stu-id="0f538-307">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="0f538-308">Nel metodo `AddValidation` aggiungere gli attributi `data-` per la convalida, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="0f538-308">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="0f538-309">Disabilitare la convalida lato client</span><span class="sxs-lookup"><span data-stu-id="0f538-309">Disable client-side validation</span></span>

<span data-ttu-id="0f538-310">Il codice seguente disabilita la convalida lato client nelle visualizzazioni MVC:</span><span class="sxs-lookup"><span data-stu-id="0f538-310">The following code disables client validation in MVC views:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup2.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="0f538-311">Questo codice funziona solo nelle viste MVC e non in Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0f538-311">This works only in MVC views, not in Razor Pages.</span></span> <span data-ttu-id="0f538-312">Un'altra opzione per disabilitare la convalida lato client consiste nell'impostare come commento il riferimento a `_ValidationScriptsPartial` nel file *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0f538-312">Another option for disabling client validation is to comment out the reference to `_ValidationScriptsPartial` in your *.cshtml* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0f538-313">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="0f538-313">Additional resources</span></span>

* [<span data-ttu-id="0f538-314">System.ComponentModel.DataAnnotations namespace</span><span class="sxs-lookup"><span data-stu-id="0f538-314">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="0f538-315">Associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="0f538-315">Model Binding</span></span>](model-binding.md)