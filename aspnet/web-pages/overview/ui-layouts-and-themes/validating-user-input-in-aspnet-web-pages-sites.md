---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Convalida dell'Input utente in ASP.NET Web Pages (Razor) siti | Microsoft Docs
author: tfitzmac
description: Questo articolo viene illustrato come convalidare le informazioni ricevute dagli utenti &mdash; , ovvero per assicurarsi che gli utenti immettano valido informazioni in formato HTML basata su form in un sistema autonomo....
ms.author: aspnetcontent
ms.date: 02/20/2014
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: d412f3fa4ca144a8a9107c971279f7bf2663cfe5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819266"
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="c638a-103">Convalida dell'Input utente in ASP.NET Web Pages (Razor) Sites</span><span class="sxs-lookup"><span data-stu-id="c638a-103">Validating User Input in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="c638a-104">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c638a-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="c638a-105">Questo articolo viene illustrato come convalidare le informazioni ricevute dagli utenti &mdash; , ovvero per assicurarsi che gli utenti immettano valido informazioni in formato HTML basata su form in un sito di ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="c638a-105">This article discusses how to validate information you get from users &mdash; that is, to make sure that users enter valid information in HTML forms in an ASP.NET Web Pages (Razor) site.</span></span>
> 
> <span data-ttu-id="c638a-106">Che cosa si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="c638a-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="c638a-107">Come verificare che un utente di input corrisponde ai criteri di convalida definite.</span><span class="sxs-lookup"><span data-stu-id="c638a-107">How to check that a user's input matches validation criteria that you define.</span></span>
> - <span data-ttu-id="c638a-108">Come determinare se sono stati superati tutti i test di convalida.</span><span class="sxs-lookup"><span data-stu-id="c638a-108">How to determine whether all validation tests have passed.</span></span>
> - <span data-ttu-id="c638a-109">Come visualizzare gli errori di convalida (e come formattarle).</span><span class="sxs-lookup"><span data-stu-id="c638a-109">How to display validation errors (and how to format them).</span></span>
> - <span data-ttu-id="c638a-110">Come convalidare i dati che non provengono direttamente dagli utenti.</span><span class="sxs-lookup"><span data-stu-id="c638a-110">How to validate data that doesn't come directly from users.</span></span>
> 
> <span data-ttu-id="c638a-111">Questi sono i concetti introdotti nell'articolo di programmazione ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="c638a-111">These are the ASP.NET programming concepts introduced in the article:</span></span>
> 
> - <span data-ttu-id="c638a-112">Il `Validation` helper.</span><span class="sxs-lookup"><span data-stu-id="c638a-112">The `Validation` helper.</span></span>
> - <span data-ttu-id="c638a-113">Il `Html.ValidationSummary` e `Html.ValidationMessage` metodi.</span><span class="sxs-lookup"><span data-stu-id="c638a-113">The `Html.ValidationSummary` and `Html.ValidationMessage` methods.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c638a-114">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="c638a-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="c638a-115">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="c638a-115">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="c638a-116">Questa esercitazione si integra inoltre con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="c638a-116">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="c638a-117">In questo articolo sono contenute le sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="c638a-117">This article contains the following sections:</span></span>

- [<span data-ttu-id="c638a-118">Panoramica della convalida dell'Input utente</span><span class="sxs-lookup"><span data-stu-id="c638a-118">Overview of User Input Validation</span></span>](#Overview_of_User_Input_Validation)
- [<span data-ttu-id="c638a-119">Convalida dell'Input utente</span><span class="sxs-lookup"><span data-stu-id="c638a-119">Validating User Input</span></span>](#Validating_User_Input)
- [<span data-ttu-id="c638a-120">Aggiunta della convalida lato Client</span><span class="sxs-lookup"><span data-stu-id="c638a-120">Adding Client-Side Validation</span></span>](#Adding_Client-Side_Validation)
- [<span data-ttu-id="c638a-121">Formattazione degli errori di convalida</span><span class="sxs-lookup"><span data-stu-id="c638a-121">Formatting Validation Errors</span></span>](#Formatting_Validation_Errors)
- [<span data-ttu-id="c638a-122">La convalida dei dati che non provengono direttamente da utenti</span><span class="sxs-lookup"><span data-stu-id="c638a-122">Validating Data That Doesn't Come Directly from Users</span></span>](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a><span data-ttu-id="c638a-123">Panoramica della convalida dell'Input utente</span><span class="sxs-lookup"><span data-stu-id="c638a-123">Overview of User Input Validation</span></span>

<span data-ttu-id="c638a-124">Se si chiede all'utente di immettere le informazioni in una pagina, ad esempio, in un form, è importante assicurarsi che i valori immessi siano validi.</span><span class="sxs-lookup"><span data-stu-id="c638a-124">If you ask users to enter information in a page — for example, into a form — it's important to make sure that the values that they enter are valid.</span></span> <span data-ttu-id="c638a-125">Ad esempio, non si desidera elaborare un form in cui mancanza informazioni critiche.</span><span class="sxs-lookup"><span data-stu-id="c638a-125">For example, you don't want to process a form that's missing critical information.</span></span>

<span data-ttu-id="c638a-126">Quando gli utenti immettono valori in un form HTML, i valori immessi sono stringhe.</span><span class="sxs-lookup"><span data-stu-id="c638a-126">When users enter values into an HTML form, the values that they enter are strings.</span></span> <span data-ttu-id="c638a-127">In molti casi, i valori che necessari sono alcuni altri tipi di dati, ad esempio date o numeri interi.</span><span class="sxs-lookup"><span data-stu-id="c638a-127">In many cases, the values you need are some other data types, like integers or dates.</span></span> <span data-ttu-id="c638a-128">Pertanto, è anche necessario assicurarsi che i valori immessi dall'utente possono essere convertiti in modo corretto per i tipi di dati appropriato.</span><span class="sxs-lookup"><span data-stu-id="c638a-128">Therefore, you also have to make sure that the values that users enter can be correctly converted to the appropriate data types.</span></span>

<span data-ttu-id="c638a-129">Potrebbe anche essere determinate restrizioni in base ai valori.</span><span class="sxs-lookup"><span data-stu-id="c638a-129">You might also have certain restrictions on the values.</span></span> <span data-ttu-id="c638a-130">Anche se gli utenti immettono correttamente un numero intero, ad esempio, è necessario assicurarsi che il valore è compreso in un determinato intervallo.</span><span class="sxs-lookup"><span data-stu-id="c638a-130">Even if users correctly enter an integer, for example, you might need to make sure that the value falls within a certain range.</span></span>

![Errori di convalida che utilizzano le classi di stile CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> <span data-ttu-id="c638a-132">**Importante** convalida dell'input utente è importante anche per la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="c638a-132">**Important** Validating user input is also important for security.</span></span> <span data-ttu-id="c638a-133">Quando si limitano i valori che possono essere immessi nel form, si riduce la probabilità che un utente può immettere un valore che può compromettere la sicurezza del sito.</span><span class="sxs-lookup"><span data-stu-id="c638a-133">When you restrict the values that users can enter in forms, you reduce the chance that someone can enter a value that can compromise the security of your site.</span></span>


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a><span data-ttu-id="c638a-134">Convalida dell'Input utente</span><span class="sxs-lookup"><span data-stu-id="c638a-134">Validating User Input</span></span>

<span data-ttu-id="c638a-135">In ASP.NET Web Pages 2, è possibile usare il `Validator` helper per testare l'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="c638a-135">In ASP.NET Web Pages 2, you can use the `Validator` helper to test user input.</span></span> <span data-ttu-id="c638a-136">L'approccio di base consiste nell'eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="c638a-136">The basic approach is to do the following:</span></span>

1. <span data-ttu-id="c638a-137">Determinare quali elementi (campi) che si desidera convalidare di input.</span><span class="sxs-lookup"><span data-stu-id="c638a-137">Determine which input elements (fields) you want to validate.</span></span>

    <span data-ttu-id="c638a-138">In genere convalidare i valori in `<input>` elementi in un form.</span><span class="sxs-lookup"><span data-stu-id="c638a-138">You typically validate values in `<input>` elements in a form.</span></span> <span data-ttu-id="c638a-139">Tuttavia, è consigliabile convalidare tutti gli input, anche input proveniente da un elemento vincolato, ad esempio un `<select>` elenco.</span><span class="sxs-lookup"><span data-stu-id="c638a-139">However, it's a good practice to validate all input, even input that comes from a constrained element like a `<select>` list.</span></span> <span data-ttu-id="c638a-140">Ciò consente di assicurarsi che gli utenti non ignorare i controlli in una pagina e inviare un modulo.</span><span class="sxs-lookup"><span data-stu-id="c638a-140">This helps to make sure that users don't bypass the controls on a page and submit a form.</span></span>
2. <span data-ttu-id="c638a-141">Nel codice della pagina, aggiungere i controlli di convalida singoli per ogni elemento di input usando i metodi del `Validation` helper.</span><span class="sxs-lookup"><span data-stu-id="c638a-141">In the page code, add individual validation checks for each input element by using methods of the `Validation` helper.</span></span>

    <span data-ttu-id="c638a-142">Per verificare i campi obbligatori, usare `Validation.RequireField(field, [error message])` (per un singolo campo) o `Validation.RequireFields(field1, field2, ...))` (per un elenco di campi).</span><span class="sxs-lookup"><span data-stu-id="c638a-142">To check for required fields, use `Validation.RequireField(field, [error message])` (for an individual field) or `Validation.RequireFields(field1, field2, ...))` (for a list of fields).</span></span> <span data-ttu-id="c638a-143">Per altri tipi di convalida, usare `Validation.Add(field, ValidationType)`.</span><span class="sxs-lookup"><span data-stu-id="c638a-143">For other types of validation, use `Validation.Add(field, ValidationType)`.</span></span> <span data-ttu-id="c638a-144">Per `ValidationType`, è possibile usare queste opzioni:</span><span class="sxs-lookup"><span data-stu-id="c638a-144">For `ValidationType`, you can use these options:</span></span>

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. <span data-ttu-id="c638a-145">Quando viene inviata la pagina, verificare se ha superato la convalida controllando `Validation.IsValid`:</span><span class="sxs-lookup"><span data-stu-id="c638a-145">When the page is submitted, check whether validation has passed by checking `Validation.IsValid`:</span></span>

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    <span data-ttu-id="c638a-146">Se sono presenti errori di convalida, si ignora l'elaborazione delle pagine normale.</span><span class="sxs-lookup"><span data-stu-id="c638a-146">If there are any validation errors, you skip normal page processing.</span></span> <span data-ttu-id="c638a-147">Ad esempio, se lo scopo della pagina si aggiorna un database, è non farlo fino a quando non sono stati risolti tutti gli errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="c638a-147">For example, if the purpose of the page is to update a database, you don't do that until all validation errors have been fixed.</span></span>
4. <span data-ttu-id="c638a-148">Se sono presenti errori di convalida, visualizzare i messaggi di errore nel markup della pagina mediante `Html.ValidationSummary` o `Html.ValidationMessage`, o entrambi.</span><span class="sxs-lookup"><span data-stu-id="c638a-148">If there are validation errors, display error messages in the page's markup by using `Html.ValidationSummary` or `Html.ValidationMessage`, or both.</span></span>

<span data-ttu-id="c638a-149">Nell'esempio seguente mostra una pagina che illustra questi passaggi.</span><span class="sxs-lookup"><span data-stu-id="c638a-149">The following example shows a page that illustrates these steps.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

<span data-ttu-id="c638a-150">Per informazioni sul funzionamento della convalida, eseguire questa pagina e deliberatamente commettono errori.</span><span class="sxs-lookup"><span data-stu-id="c638a-150">To see how validation works, run this page and deliberately make mistakes.</span></span> <span data-ttu-id="c638a-151">Ad esempio, ecco ciò che la pagina è simile se si dimentica di immettere il nome del corso, se si immette un, e se si immette una data non valida:</span><span class="sxs-lookup"><span data-stu-id="c638a-151">For example, here's what the page looks like if you forget to enter a course name, if you enter an, and if you enter an invalid date:</span></span>

![Errori di convalida nella pagina sottoposta a rendering](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a><span data-ttu-id="c638a-153">Aggiunta della convalida lato Client</span><span class="sxs-lookup"><span data-stu-id="c638a-153">Adding Client-Side Validation</span></span>

<span data-ttu-id="c638a-154">Per impostazione predefinita, viene convalidato input utente dopo l'invio della pagina, vale a dire, la convalida viene eseguita nel codice server.</span><span class="sxs-lookup"><span data-stu-id="c638a-154">By default, user input is validated after users submit the page — that is, the validation is performed in server code.</span></span> <span data-ttu-id="c638a-155">Uno svantaggio di questo approccio è che gli utenti non sappiano che sono state apportate un errore fino a dopo la pagina Invia.</span><span class="sxs-lookup"><span data-stu-id="c638a-155">A disadvantage of this approach is that users don't know that they've made an error until after they submit the page.</span></span> <span data-ttu-id="c638a-156">Se un modulo è lunga o complessa, la segnalazione degli errori solo dopo l'invio della pagina può risultare poco pratica all'utente.</span><span class="sxs-lookup"><span data-stu-id="c638a-156">If a form is long or complex, reporting errors only after the page is submitted can be inconvenient to the user.</span></span>

<span data-ttu-id="c638a-157">È possibile aggiungere il supporto per eseguire la convalida in script client.</span><span class="sxs-lookup"><span data-stu-id="c638a-157">You can add support to perform validation in client script.</span></span> <span data-ttu-id="c638a-158">In tal caso, la convalida viene eseguita con gli utenti del lavoro nel browser.</span><span class="sxs-lookup"><span data-stu-id="c638a-158">In that case, the validation is performed as users work in the browser.</span></span> <span data-ttu-id="c638a-159">Ad esempio, si supponga di che specificare che un valore deve essere un numero intero.</span><span class="sxs-lookup"><span data-stu-id="c638a-159">For example, suppose you specify that a value should be an integer.</span></span> <span data-ttu-id="c638a-160">Se un utente immette un valore diverso da integer, viene segnalato l'errore, non appena l'utente lascia il campo di immissione.</span><span class="sxs-lookup"><span data-stu-id="c638a-160">If a user enters a non-integer value, the error is reported as soon as the user leaves the entry field.</span></span> <span data-ttu-id="c638a-161">Gli utenti ricevono un riscontro immediato, che è facilmente.</span><span class="sxs-lookup"><span data-stu-id="c638a-161">Users get immediate feedback, which is convenient for them.</span></span> <span data-ttu-id="c638a-162">Convalida basata su client può anche ridurre il numero di volte in cui l'utente deve inviare il modulo per correggere gli errori di più.</span><span class="sxs-lookup"><span data-stu-id="c638a-162">Client-based validation can also reduce the number of times that the user has to submit the form to correct multiple errors.</span></span>

> [!NOTE]
> <span data-ttu-id="c638a-163">Anche se si usa la convalida lato client, la convalida viene eseguita sempre anche nel codice server.</span><span class="sxs-lookup"><span data-stu-id="c638a-163">Even if you use client-side validation, validation is always also performed in server code.</span></span> <span data-ttu-id="c638a-164">Esegue la convalida nel codice del server è una misura di sicurezza, nel caso in cui gli utenti di ignorare la convalida basata su client.</span><span class="sxs-lookup"><span data-stu-id="c638a-164">Performing validation in server code is a security measure, in case users bypass client-based validation.</span></span>


1. <span data-ttu-id="c638a-165">Registra le librerie JavaScript seguente nella pagina:</span><span class="sxs-lookup"><span data-stu-id="c638a-165">Register the following JavaScript libraries in the page:</span></span>  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   <span data-ttu-id="c638a-166">Due delle librerie sono può essere caricato da una rete di distribuzione di contenuti (CDN), quindi non necessariamente è necessario in modo che vengano nel computer o server.</span><span class="sxs-lookup"><span data-stu-id="c638a-166">Two of the libraries are loadable from a content delivery network (CDN), so you don't necessarily have to have them on your computer or server.</span></span> <span data-ttu-id="c638a-167">Tuttavia, è necessario disporre una copia locale del *jquery.validate.unobtrusive.js*.</span><span class="sxs-lookup"><span data-stu-id="c638a-167">However, you must have a local copy of *jquery.validate.unobtrusive.js*.</span></span> <span data-ttu-id="c638a-168">Se sta non ancora lavorando con un modello di WebMatrix (ad esempio **Starter Site** ) che include la libreria, creare un sito Web Pages base **Starter Site**.</span><span class="sxs-lookup"><span data-stu-id="c638a-168">If you are not already working with a WebMatrix template (like **Starter Site** ) that includes the library, create a Web Pages site that's based on **Starter Site**.</span></span> <span data-ttu-id="c638a-169">Copiare quindi il *js* file al sito corrente.</span><span class="sxs-lookup"><span data-stu-id="c638a-169">Then copy the *.js* file to your current site.</span></span>
2. <span data-ttu-id="c638a-170">Nel markup, per ogni elemento che sta effettuando la convalida, aggiungere una chiamata a `Validation.For(field)`.</span><span class="sxs-lookup"><span data-stu-id="c638a-170">In markup, for each element that you're validating, add a call to `Validation.For(field)`.</span></span> <span data-ttu-id="c638a-171">Questo metodo genera gli attributi utilizzati per la convalida lato client.</span><span class="sxs-lookup"><span data-stu-id="c638a-171">This method emits attributes that are used by client-side validation.</span></span> <span data-ttu-id="c638a-172">(Piuttosto che genera il codice JavaScript effettivo, il metodo genera attributi quali `data-val-...`.</span><span class="sxs-lookup"><span data-stu-id="c638a-172">(Rather than emitting actual JavaScript code, the method emits attributes like `data-val-...`.</span></span> <span data-ttu-id="c638a-173">Questi attributi supportano la convalida del client non intrusivi che usa jQuery per svolgere il lavoro).</span><span class="sxs-lookup"><span data-stu-id="c638a-173">These attributes support unobtrusive client validation that uses jQuery to do the work.)</span></span>

<span data-ttu-id="c638a-174">La pagina seguente viene illustrato come aggiungere funzionalità di convalida client all'esempio illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="c638a-174">The following page shows how to add client validation features to the example shown earlier.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

<span data-ttu-id="c638a-175">Non tutti i controlli di convalida eseguiti sul client.</span><span class="sxs-lookup"><span data-stu-id="c638a-175">Not all validation checks run on the client.</span></span> <span data-ttu-id="c638a-176">In particolare, la convalida del tipo di dati (integer, data e così via) non vengono eseguiti nel client.</span><span class="sxs-lookup"><span data-stu-id="c638a-176">In particular, data-type validation (integer, date, and so on) don't run on the client.</span></span> <span data-ttu-id="c638a-177">I controlli seguenti funzionano sia il client di server:</span><span class="sxs-lookup"><span data-stu-id="c638a-177">The following checks work on both the client and server:</span></span>

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

<span data-ttu-id="c638a-178">In questo esempio, il test per una data valida non funzionerà nel codice client.</span><span class="sxs-lookup"><span data-stu-id="c638a-178">In this example, the test for a valid date won't work in client code.</span></span> <span data-ttu-id="c638a-179">Tuttavia, verrà eseguito il test nel codice server.</span><span class="sxs-lookup"><span data-stu-id="c638a-179">However, the test will be performed in server code.</span></span>

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a><span data-ttu-id="c638a-180">Formattazione degli errori di convalida</span><span class="sxs-lookup"><span data-stu-id="c638a-180">Formatting Validation Errors</span></span>

<span data-ttu-id="c638a-181">È possibile controllare la modalità di visualizzazione degli errori di convalida la definizione delle classi CSS che hanno nomi riservati seguenti:</span><span class="sxs-lookup"><span data-stu-id="c638a-181">You can control how validation errors are displayed by defining CSS classes that have the following reserved names:</span></span>

- <span data-ttu-id="c638a-182">`field-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="c638a-182">`field-validation-error`.</span></span> <span data-ttu-id="c638a-183">Definisce l'output del `Html.ValidationMessage` metodo quando visualizza un errore.</span><span class="sxs-lookup"><span data-stu-id="c638a-183">Defines the output of the `Html.ValidationMessage` method when it's displaying an error.</span></span>
- <span data-ttu-id="c638a-184">`field-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="c638a-184">`field-validation-valid`.</span></span> <span data-ttu-id="c638a-185">Definisce l'output del `Html.ValidationMessage` metodo quando si verifica alcun errore.</span><span class="sxs-lookup"><span data-stu-id="c638a-185">Defines the output of the `Html.ValidationMessage` method when there is no error.</span></span>
- <span data-ttu-id="c638a-186">`input-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="c638a-186">`input-validation-error`.</span></span> <span data-ttu-id="c638a-187">Definisce come `<input>` rendering degli elementi quando si verifica un errore.</span><span class="sxs-lookup"><span data-stu-id="c638a-187">Defines how `<input>` elements are rendered when there's an error.</span></span> <span data-ttu-id="c638a-188">(Ad esempio, è possibile utilizzare questa classe per impostare il colore di sfondo di un' &lt;input&gt; elemento da un colore diverso se il relativo valore non è valido.) Questa classe CSS viene usata solo durante la convalida del client (in ASP.NET Web Pages 2).</span><span class="sxs-lookup"><span data-stu-id="c638a-188">(For example, you can use this class to set the background color of an &lt;input&gt; element to a different color if its value is invalid.) This CSS class is used only during client validation (in ASP.NET Web Pages 2).</span></span>
- <span data-ttu-id="c638a-189">`input-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="c638a-189">`input-validation-valid`.</span></span> <span data-ttu-id="c638a-190">Definisce l'aspetto del `<input>` elementi quando non si è verificato alcun errore.</span><span class="sxs-lookup"><span data-stu-id="c638a-190">Defines the appearance of `<input>` elements when there is no error.</span></span>
- <span data-ttu-id="c638a-191">`validation-summary-errors`.</span><span class="sxs-lookup"><span data-stu-id="c638a-191">`validation-summary-errors`.</span></span> <span data-ttu-id="c638a-192">Definisce l'output del `Html.ValidationSummary` metodo viene visualizzato un elenco di errori.</span><span class="sxs-lookup"><span data-stu-id="c638a-192">Defines the output of the `Html.ValidationSummary` method it's displaying a list of errors.</span></span>
- <span data-ttu-id="c638a-193">`validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="c638a-193">`validation-summary-valid`.</span></span> <span data-ttu-id="c638a-194">Definisce l'output del `Html.ValidationSummary` metodo quando si verifica alcun errore.</span><span class="sxs-lookup"><span data-stu-id="c638a-194">Defines the output of the `Html.ValidationSummary` method when there is no error.</span></span>

<span data-ttu-id="c638a-195">Nell'esempio `<style>` vengono mostrate le regole per condizioni di errore.</span><span class="sxs-lookup"><span data-stu-id="c638a-195">The following `<style>` block shows rules for error conditions.</span></span>

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

<span data-ttu-id="c638a-196">Se si include questo blocco di stile nelle pagine di esempio da sopra nell'articolo, la visualizzazione dell'errore sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="c638a-196">If you include this style block in the example pages from earlier in the article, the error display will look like the following illustration:</span></span>

![Errori di convalida che utilizzano le classi di stile CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="c638a-198">Se non si usa la convalida del client in ASP.NET Web Pages 2, le classi CSS per il `<input>` elementi (`input-validation-error` e `input-validation-valid` non hanno alcun effetto.</span><span class="sxs-lookup"><span data-stu-id="c638a-198">If you're not using client validation in ASP.NET Web Pages 2, the CSS classes for the `<input>` elements (`input-validation-error` and `input-validation-valid` don't have any effect.</span></span>


### <a name="static-and-dynamic-error-display"></a><span data-ttu-id="c638a-199">Visualizzazione degli errori statici e dinamici</span><span class="sxs-lookup"><span data-stu-id="c638a-199">Static and Dynamic Error Display</span></span>

<span data-ttu-id="c638a-200">Le regole CSS sono disponibili in coppie, ad esempio `validation-summary-errors` e `validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="c638a-200">The CSS rules come in pairs, such as `validation-summary-errors` and `validation-summary-valid`.</span></span> <span data-ttu-id="c638a-201">Queste coppie consentono di definire le regole per entrambe le condizioni: una condizione di errore e una condizione (non degli errori) "normale".</span><span class="sxs-lookup"><span data-stu-id="c638a-201">These pairs let you define rules for both conditions: an error condition and a "normal" (non-error) condition.</span></span> <span data-ttu-id="c638a-202">È importante comprendere che il markup per la visualizzazione degli errori viene sempre eseguito, anche se non sono presenti errori.</span><span class="sxs-lookup"><span data-stu-id="c638a-202">It's important to understand that the markup for the error display is always rendered, even if there are no errors.</span></span> <span data-ttu-id="c638a-203">Ad esempio, se una pagina presenta un `Html.ValidationSummary` metodo nel markup, l'origine della pagina conterrà il markup seguente anche quando viene richiesta la pagina per la prima volta:</span><span class="sxs-lookup"><span data-stu-id="c638a-203">For example, if a page has an `Html.ValidationSummary` method in the markup, the page source will contain the following markup even when the page is requested for the first time:</span></span>

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

<span data-ttu-id="c638a-204">In altre parole, il `Html.ValidationSummary` metodo esegue il rendering sempre un `<div>` elemento e un elenco, anche se l'elenco di errori è vuota.</span><span class="sxs-lookup"><span data-stu-id="c638a-204">In other words, the `Html.ValidationSummary` method always renders a `<div>` element and a list, even if the error list is empty.</span></span> <span data-ttu-id="c638a-205">Allo stesso modo, il `Html.ValidationMessage` metodo esegue il rendering sempre un `<span>` elemento come segnaposto per un errore di singoli campi, anche se è presente alcun errore.</span><span class="sxs-lookup"><span data-stu-id="c638a-205">Similarly, the `Html.ValidationMessage` method always renders a `<span>` element as a placeholder for an individual field error, even if there is no error.</span></span>

<span data-ttu-id="c638a-206">In alcune situazioni, la visualizzazione di un messaggio di errore possono utilizzare la pagina per ridisporre e può causare gli elementi nella pagina per spostarsi all'interno.</span><span class="sxs-lookup"><span data-stu-id="c638a-206">In some situations, displaying an error message can cause the page to reflow and can cause elements on the page to move around.</span></span> <span data-ttu-id="c638a-207">Le regole CSS che terminano in `-valid` consentono di definire un layout che consentono di evitare questo problema.</span><span class="sxs-lookup"><span data-stu-id="c638a-207">The CSS rules that end in `-valid` let you define a layout that can help prevent this problem.</span></span> <span data-ttu-id="c638a-208">Ad esempio, è possibile definire `field-validation-error` e `field-validation-valid` a entrambi hanno lo stesso a dimensione fissa.</span><span class="sxs-lookup"><span data-stu-id="c638a-208">For example, you can define `field-validation-error` and `field-validation-valid` to both have the same fixed size.</span></span> <span data-ttu-id="c638a-209">In questo modo, l'area di visualizzazione per il campo è statico e non verrà modificato il flusso della pagina, se viene visualizzato un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="c638a-209">That way, the display area for the field is static and won't change the page flow if an error message is displayed.</span></span>

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a><span data-ttu-id="c638a-210">La convalida dei dati che non provengono direttamente da utenti</span><span class="sxs-lookup"><span data-stu-id="c638a-210">Validating Data That Doesn't Come Directly from Users</span></span>

<span data-ttu-id="c638a-211">A volte è necessario convalidare le informazioni che non provengono direttamente da un form HTML.</span><span class="sxs-lookup"><span data-stu-id="c638a-211">Sometimes you have to validate information that doesn't come directly from an HTML form.</span></span> <span data-ttu-id="c638a-212">Un esempio tipico è una pagina in cui viene passato un valore in una stringa di query, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="c638a-212">A typical example is a page where a value is passed in a query string, as in the following example:</span></span>

`http://server/myapp/EditClassInformation?classid=1022`

<span data-ttu-id="c638a-213">In questo caso, si desidera assicurarsi che il valore passato alla pagina (a questo punto, per il valore di 1022 `classid`) è valido.</span><span class="sxs-lookup"><span data-stu-id="c638a-213">In this case, you want to make sure that the value that's passed to the page (here, 1022 for the value of `classid`) is valid.</span></span> <span data-ttu-id="c638a-214">È possibile utilizzare direttamente la `Validation` helper per eseguire la convalida.</span><span class="sxs-lookup"><span data-stu-id="c638a-214">You can't directly use the `Validation` helper to perform this validation.</span></span> <span data-ttu-id="c638a-215">Tuttavia, è possibile usare altre funzionalità del sistema di convalida, come la possibilità di visualizzare i messaggi di errore di convalida.</span><span class="sxs-lookup"><span data-stu-id="c638a-215">However, you can use other features of the validation system, like the ability to display validation error messages.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="c638a-216">**Importanti** convalidare sempre i valori che si ottiene mediante *qualsiasi* origine, inclusi i valori del campo del form, i valori di stringa di query e i valori dei cookie.</span><span class="sxs-lookup"><span data-stu-id="c638a-216">**Important** Always validate values that you get from *any* source, including form-field values, query-string values, and cookie values.</span></span> <span data-ttu-id="c638a-217">È facile agli utenti di modificare questi valori (ad esempio per scopi dannosi).</span><span class="sxs-lookup"><span data-stu-id="c638a-217">It's easy for people to change these values (perhaps for malicious purposes).</span></span> <span data-ttu-id="c638a-218">Pertanto, è necessario verificare questi valori per proteggere l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c638a-218">So you must check these values in order to protect your application.</span></span>


<span data-ttu-id="c638a-219">Nell'esempio seguente viene illustrato come è possibile convalidare un valore che viene passato una stringa di query.</span><span class="sxs-lookup"><span data-stu-id="c638a-219">The following example shows how you might validate a value that's passed in a query string.</span></span> <span data-ttu-id="c638a-220">Il codice verifica che il valore non vuoto e che sia un numero intero.</span><span class="sxs-lookup"><span data-stu-id="c638a-220">The code tests that the value is not empty and that it's an integer.</span></span>

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

<span data-ttu-id="c638a-221">Si noti che il test viene eseguito quando la richiesta non è l'invio di un form (`if(!IsPost)`).</span><span class="sxs-lookup"><span data-stu-id="c638a-221">Notice that the test is performed when the request is not a form submission (`if(!IsPost)`).</span></span> <span data-ttu-id="c638a-222">La prima volta che viene richiesta la pagina passa questo test, ma non quando la richiesta è l'invio di un form.</span><span class="sxs-lookup"><span data-stu-id="c638a-222">This test would pass the first time that the page is requested, but not when the request is a form submission.</span></span>

<span data-ttu-id="c638a-223">Per visualizzare questo errore, è possibile aggiungere l'errore all'elenco di errori di convalida tramite una chiamata `Validation.AddFormError("message")`.</span><span class="sxs-lookup"><span data-stu-id="c638a-223">To display this error, you can add the error to the list of validation errors by calling `Validation.AddFormError("message")`.</span></span> <span data-ttu-id="c638a-224">Se la pagina contiene una chiamata al `Html.ValidationSummary` metodo, l'errore viene visualizzato, proprio come un errore di convalida dell'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="c638a-224">If the page contains a call to the `Html.ValidationSummary` method, the error is displayed there, just like a user-input validation error.</span></span>

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="c638a-225">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c638a-225">Additional Resources</span></span>

[<span data-ttu-id="c638a-226">Utilizzo di moduli HTML in siti con pagine Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c638a-226">Working with HTML Forms in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkID=202892)
