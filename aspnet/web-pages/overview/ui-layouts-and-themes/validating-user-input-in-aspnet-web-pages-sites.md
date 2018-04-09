---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Convalida dell'Input utente in ASP.NET Web Pages siti (Razor) | Documenti Microsoft
author: tfitzmac
description: In questo articolo viene descritto come convalidare le informazioni ricevute dagli utenti &mdash; , ovvero per assicurarsi che gli utenti immettano valido informazioni in formato HTML basata su form in un sistema autonomo....
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 34f703e6db70ac79c22f4a50d4cfd4e2326b4c74
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="49949-103">Convalida dell'Input utente in siti Web di ASP.NET di pagine (Razor)</span><span class="sxs-lookup"><span data-stu-id="49949-103">Validating User Input in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="49949-104">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="49949-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="49949-105">In questo articolo viene descritto come convalidare le informazioni ricevute dagli utenti &mdash; , ovvero per assicurarsi che gli utenti immettano valido informazioni in formato HTML basata su form in un sito di pagine Web ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="49949-105">This article discusses how to validate information you get from users &mdash; that is, to make sure that users enter valid information in HTML forms in an ASP.NET Web Pages (Razor) site.</span></span>
> 
> <span data-ttu-id="49949-106">Illustra quanto segue:</span><span class="sxs-lookup"><span data-stu-id="49949-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="49949-107">Come controllare l'input che un utente corrisponde ai criteri di convalida che definiscono.</span><span class="sxs-lookup"><span data-stu-id="49949-107">How to check that a user's input matches validation criteria that you define.</span></span>
> - <span data-ttu-id="49949-108">Come determinare se sono stati superati tutti i test di convalida.</span><span class="sxs-lookup"><span data-stu-id="49949-108">How to determine whether all validation tests have passed.</span></span>
> - <span data-ttu-id="49949-109">Come visualizzare gli errori di convalida e come formattarle.</span><span class="sxs-lookup"><span data-stu-id="49949-109">How to display validation errors (and how to format them).</span></span>
> - <span data-ttu-id="49949-110">Come convalidare i dati che non provengano direttamente dagli utenti.</span><span class="sxs-lookup"><span data-stu-id="49949-110">How to validate data that doesn't come directly from users.</span></span>
> 
> <span data-ttu-id="49949-111">Programmazione di concetti introdotti nell'articolo ASP.NET sono:</span><span class="sxs-lookup"><span data-stu-id="49949-111">These are the ASP.NET programming concepts introduced in the article:</span></span>
> 
> - <span data-ttu-id="49949-112">Il `Validation` helper.</span><span class="sxs-lookup"><span data-stu-id="49949-112">The `Validation` helper.</span></span>
> - <span data-ttu-id="49949-113">Il `Html.ValidationSummary` e `Html.ValidationMessage` metodi.</span><span class="sxs-lookup"><span data-stu-id="49949-113">The `Html.ValidationSummary` and `Html.ValidationMessage` methods.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="49949-114">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="49949-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="49949-115">Pagine Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="49949-115">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="49949-116">In questa esercitazione si integra inoltre con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="49949-116">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="49949-117">In questo articolo sono contenute le sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="49949-117">This article contains the following sections:</span></span>

- [<span data-ttu-id="49949-118">Panoramica di convalida dell'Input utente</span><span class="sxs-lookup"><span data-stu-id="49949-118">Overview of User Input Validation</span></span>](#Overview_of_User_Input_Validation)
- [<span data-ttu-id="49949-119">Convalida dell'Input utente</span><span class="sxs-lookup"><span data-stu-id="49949-119">Validating User Input</span></span>](#Validating_User_Input)
- [<span data-ttu-id="49949-120">Aggiunta della convalida lato Client</span><span class="sxs-lookup"><span data-stu-id="49949-120">Adding Client-Side Validation</span></span>](#Adding_Client-Side_Validation)
- [<span data-ttu-id="49949-121">Gli errori di convalida di formattazione</span><span class="sxs-lookup"><span data-stu-id="49949-121">Formatting Validation Errors</span></span>](#Formatting_Validation_Errors)
- [<span data-ttu-id="49949-122">Convalida dei dati che non provengano direttamente dagli utenti</span><span class="sxs-lookup"><span data-stu-id="49949-122">Validating Data That Doesn't Come Directly from Users</span></span>](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a><span data-ttu-id="49949-123">Panoramica di convalida dell'Input utente</span><span class="sxs-lookup"><span data-stu-id="49949-123">Overview of User Input Validation</span></span>

<span data-ttu-id="49949-124">Se chiedere agli utenti di immettere le informazioni in una pagina, ad esempio, in un form, è importante assicurarsi che i valori immessi siano validi.</span><span class="sxs-lookup"><span data-stu-id="49949-124">If you ask users to enter information in a page — for example, into a form — it's important to make sure that the values that they enter are valid.</span></span> <span data-ttu-id="49949-125">Ad esempio, non si desidera elaborare un modulo privo di informazioni critiche.</span><span class="sxs-lookup"><span data-stu-id="49949-125">For example, you don't want to process a form that's missing critical information.</span></span>

<span data-ttu-id="49949-126">Quando gli utenti immettono i valori in un form HTML, i valori immessi sono stringhe.</span><span class="sxs-lookup"><span data-stu-id="49949-126">When users enter values into an HTML form, the values that they enter are strings.</span></span> <span data-ttu-id="49949-127">In molti casi, i valori che necessari sono alcuni altri tipi di dati, ad esempio numeri interi o date.</span><span class="sxs-lookup"><span data-stu-id="49949-127">In many cases, the values you need are some other data types, like integers or dates.</span></span> <span data-ttu-id="49949-128">Pertanto, è inoltre necessario assicurarsi che i valori che gli utenti immettono possono essere convertiti correttamente ai tipi di dati appropriato.</span><span class="sxs-lookup"><span data-stu-id="49949-128">Therefore, you also have to make sure that the values that users enter can be correctly converted to the appropriate data types.</span></span>

<span data-ttu-id="49949-129">Può inoltre essere alcune limitazioni sui valori.</span><span class="sxs-lookup"><span data-stu-id="49949-129">You might also have certain restrictions on the values.</span></span> <span data-ttu-id="49949-130">Anche se gli utenti immettono correttamente un numero intero, ad esempio, si potrebbe essere necessario per assicurarsi che il valore è compreso in un determinato intervallo.</span><span class="sxs-lookup"><span data-stu-id="49949-130">Even if users correctly enter an integer, for example, you might need to make sure that the value falls within a certain range.</span></span>

![Errori di convalida che usano le classi di stile CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> <span data-ttu-id="49949-132">**Importante** convalida dell'input utente è anche importante per la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="49949-132">**Important** Validating user input is also important for security.</span></span> <span data-ttu-id="49949-133">Quando si limitano i valori che possono essere immesse nel form, si riduce la probabilità che un utente può immettere un valore che può compromettere la sicurezza del sito.</span><span class="sxs-lookup"><span data-stu-id="49949-133">When you restrict the values that users can enter in forms, you reduce the chance that someone can enter a value that can compromise the security of your site.</span></span>


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a><span data-ttu-id="49949-134">Convalida dell'Input utente</span><span class="sxs-lookup"><span data-stu-id="49949-134">Validating User Input</span></span>

<span data-ttu-id="49949-135">In ASP.NET Web Pages 2, è possibile utilizzare il `Validator` helper per verificare l'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="49949-135">In ASP.NET Web Pages 2, you can use the `Validator` helper to test user input.</span></span> <span data-ttu-id="49949-136">L'approccio di base consiste nell'eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="49949-136">The basic approach is to do the following:</span></span>

1. <span data-ttu-id="49949-137">Determinare quali input elementi (campi) che si desidera convalidare.</span><span class="sxs-lookup"><span data-stu-id="49949-137">Determine which input elements (fields) you want to validate.</span></span>

    <span data-ttu-id="49949-138">In genere convalidare i valori in `<input>` elementi in un form.</span><span class="sxs-lookup"><span data-stu-id="49949-138">You typically validate values in `<input>` elements in a form.</span></span> <span data-ttu-id="49949-139">Tuttavia, è buona norma convalidare tutti gli input, anche l'input proveniente da un elemento vincolato come un `<select>` elenco.</span><span class="sxs-lookup"><span data-stu-id="49949-139">However, it's a good practice to validate all input, even input that comes from a constrained element like a `<select>` list.</span></span> <span data-ttu-id="49949-140">Ciò consente di verificare che gli utenti non ignorare i controlli in una pagina e inviare un modulo.</span><span class="sxs-lookup"><span data-stu-id="49949-140">This helps to make sure that users don't bypass the controls on a page and submit a form.</span></span>
2. <span data-ttu-id="49949-141">Nel codice della pagina, aggiungere i controlli di convalida singoli per ogni elemento di input utilizzando metodi del `Validation` helper.</span><span class="sxs-lookup"><span data-stu-id="49949-141">In the page code, add individual validation checks for each input element by using methods of the `Validation` helper.</span></span>

    <span data-ttu-id="49949-142">Per verificare i campi obbligatori, utilizzare `Validation.RequireField(field, [error message])` (per un singolo campo) o `Validation.RequireFields(field1, field2, ...))` (per un elenco di campi).</span><span class="sxs-lookup"><span data-stu-id="49949-142">To check for required fields, use `Validation.RequireField(field, [error message])` (for an individual field) or `Validation.RequireFields(field1, field2, ...))` (for a list of fields).</span></span> <span data-ttu-id="49949-143">Per altri tipi di convalida, utilizzare `Validation.Add(field, ValidationType)`.</span><span class="sxs-lookup"><span data-stu-id="49949-143">For other types of validation, use `Validation.Add(field, ValidationType)`.</span></span> <span data-ttu-id="49949-144">Per `ValidationType`, è possibile utilizzare queste opzioni:</span><span class="sxs-lookup"><span data-stu-id="49949-144">For `ValidationType`, you can use these options:</span></span>

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
3. <span data-ttu-id="49949-145">Quando viene inviata la pagina, controllare se la convalida ha superato controllando `Validation.IsValid`:</span><span class="sxs-lookup"><span data-stu-id="49949-145">When the page is submitted, check whether validation has passed by checking `Validation.IsValid`:</span></span>

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    <span data-ttu-id="49949-146">Se sono presenti errori di convalida, ignorando l'elaborazione della pagina normale.</span><span class="sxs-lookup"><span data-stu-id="49949-146">If there are any validation errors, you skip normal page processing.</span></span> <span data-ttu-id="49949-147">Ad esempio, se lo scopo della pagina consiste nell'aggiornare un database, è non farlo fino a quando non sono stati risolti tutti gli errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="49949-147">For example, if the purpose of the page is to update a database, you don't do that until all validation errors have been fixed.</span></span>
4. <span data-ttu-id="49949-148">Se sono presenti errori di convalida, visualizzare i messaggi di errore nel markup della pagina utilizzando `Html.ValidationSummary` o `Html.ValidationMessage`, o entrambi.</span><span class="sxs-lookup"><span data-stu-id="49949-148">If there are validation errors, display error messages in the page's markup by using `Html.ValidationSummary` or `Html.ValidationMessage`, or both.</span></span>

<span data-ttu-id="49949-149">L'esempio seguente mostra una pagina in cui vengono illustrati tali passaggi.</span><span class="sxs-lookup"><span data-stu-id="49949-149">The following example shows a page that illustrates these steps.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

<span data-ttu-id="49949-150">Per visualizzare la modalità di convalida, eseguire questa pagina e deliberatamente commettere errori.</span><span class="sxs-lookup"><span data-stu-id="49949-150">To see how validation works, run this page and deliberately make mistakes.</span></span> <span data-ttu-id="49949-151">Ad esempio, di seguito come viene visualizzata la pagina se si dimentica di immettere un nome di linea, se si immette un, e se si immette una data non valida:</span><span class="sxs-lookup"><span data-stu-id="49949-151">For example, here's what the page looks like if you forget to enter a course name, if you enter an, and if you enter an invalid date:</span></span>

![Errori di convalida nella pagina sottoposta a rendering](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a><span data-ttu-id="49949-153">Aggiunta della convalida lato Client</span><span class="sxs-lookup"><span data-stu-id="49949-153">Adding Client-Side Validation</span></span>

<span data-ttu-id="49949-154">Per impostazione predefinita, viene convalidata l'input dell'utente dopo l'invio della pagina, vale a dire, la convalida viene eseguita nel codice server.</span><span class="sxs-lookup"><span data-stu-id="49949-154">By default, user input is validated after users submit the page — that is, the validation is performed in server code.</span></span> <span data-ttu-id="49949-155">Uno svantaggio di questo approccio è che gli utenti non sanno che sono state apportate errore fino a dopo invio della pagina.</span><span class="sxs-lookup"><span data-stu-id="49949-155">A disadvantage of this approach is that users don't know that they've made an error until after they submit the page.</span></span> <span data-ttu-id="49949-156">Se un form è lunga o complessa, la segnalazione degli errori solo dopo l'invio della pagina può risultare poco pratica per l'utente.</span><span class="sxs-lookup"><span data-stu-id="49949-156">If a form is long or complex, reporting errors only after the page is submitted can be inconvenient to the user.</span></span>

<span data-ttu-id="49949-157">È possibile aggiungere il supporto per eseguire la convalida negli script client.</span><span class="sxs-lookup"><span data-stu-id="49949-157">You can add support to perform validation in client script.</span></span> <span data-ttu-id="49949-158">In tal caso, la convalida viene eseguita con gli utenti del lavoro nel browser.</span><span class="sxs-lookup"><span data-stu-id="49949-158">In that case, the validation is performed as users work in the browser.</span></span> <span data-ttu-id="49949-159">Si supponga, ad esempio, che specificare che un valore deve essere un numero intero.</span><span class="sxs-lookup"><span data-stu-id="49949-159">For example, suppose you specify that a value should be an integer.</span></span> <span data-ttu-id="49949-160">Se un utente immette un valore non intero, viene segnalato l'errore non appena l'utente lascia il campo immissione.</span><span class="sxs-lookup"><span data-stu-id="49949-160">If a user enters a non-integer value, the error is reported as soon as the user leaves the entry field.</span></span> <span data-ttu-id="49949-161">Gli utenti ricevono un riscontro immediato, è opportuno.</span><span class="sxs-lookup"><span data-stu-id="49949-161">Users get immediate feedback, which is convenient for them.</span></span> <span data-ttu-id="49949-162">Convalida basata su client può anche ridurre il numero di volte in cui l'utente deve inviare il modulo per correggere gli errori più.</span><span class="sxs-lookup"><span data-stu-id="49949-162">Client-based validation can also reduce the number of times that the user has to submit the form to correct multiple errors.</span></span>

> [!NOTE]
> <span data-ttu-id="49949-163">Anche se si utilizza la convalida lato client, la convalida viene eseguita sempre anche nel codice server.</span><span class="sxs-lookup"><span data-stu-id="49949-163">Even if you use client-side validation, validation is always also performed in server code.</span></span> <span data-ttu-id="49949-164">Esegue la convalida nel codice server è una misura di sicurezza, nel caso in cui gli utenti di ignorare la convalida basata su client.</span><span class="sxs-lookup"><span data-stu-id="49949-164">Performing validation in server code is a security measure, in case users bypass client-based validation.</span></span>


1. <span data-ttu-id="49949-165">Registrare le librerie JavaScript seguente nella pagina:</span><span class="sxs-lookup"><span data-stu-id="49949-165">Register the following JavaScript libraries in the page:</span></span>  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   <span data-ttu-id="49949-166">Due delle librerie sono può essere caricato da una rete di distribuzione di contenuti (CDN), non è necessariamente in modo che nel computer o server.</span><span class="sxs-lookup"><span data-stu-id="49949-166">Two of the libraries are loadable from a content delivery network (CDN), so you don't necessarily have to have them on your computer or server.</span></span> <span data-ttu-id="49949-167">Tuttavia, è necessario disporre di una copia locale di *jquery.validate.unobtrusive.js*.</span><span class="sxs-lookup"><span data-stu-id="49949-167">However, you must have a local copy of *jquery.validate.unobtrusive.js*.</span></span> <span data-ttu-id="49949-168">Se sta non ancora lavorando con un modello di WebMatrix (ad esempio **Starter Site** ) che include la libreria, creare un sito di pagine Web che si basa sul **Starter Site**.</span><span class="sxs-lookup"><span data-stu-id="49949-168">If you are not already working with a WebMatrix template (like **Starter Site** ) that includes the library, create a Web Pages site that's based on **Starter Site**.</span></span> <span data-ttu-id="49949-169">Quindi copiare la *. js* file al sito corrente.</span><span class="sxs-lookup"><span data-stu-id="49949-169">Then copy the *.js* file to your current site.</span></span>
2. <span data-ttu-id="49949-170">Nel markup di ogni elemento che sta effettuando la convalida, aggiungere una chiamata a `Validation.For(field)`.</span><span class="sxs-lookup"><span data-stu-id="49949-170">In markup, for each element that you're validating, add a call to `Validation.For(field)`.</span></span> <span data-ttu-id="49949-171">Questo metodo genera gli attributi utilizzati per la convalida lato client.</span><span class="sxs-lookup"><span data-stu-id="49949-171">This method emits attributes that are used by client-side validation.</span></span> <span data-ttu-id="49949-172">(Anziché la creazione di codice JavaScript, il metodo genera attributi quali `data-val-...`.</span><span class="sxs-lookup"><span data-stu-id="49949-172">(Rather than emitting actual JavaScript code, the method emits attributes like `data-val-...`.</span></span> <span data-ttu-id="49949-173">Questi attributi supportano la convalida client non intrusiva che utilizza jQuery per eseguire il lavoro.)</span><span class="sxs-lookup"><span data-stu-id="49949-173">These attributes support unobtrusive client validation that uses jQuery to do the work.)</span></span>

<span data-ttu-id="49949-174">La pagina seguente mostra come aggiungere funzionalità di convalida client all'esempio illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="49949-174">The following page shows how to add client validation features to the example shown earlier.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

<span data-ttu-id="49949-175">Non tutti i controlli di convalida eseguiti sul client.</span><span class="sxs-lookup"><span data-stu-id="49949-175">Not all validation checks run on the client.</span></span> <span data-ttu-id="49949-176">In particolare, la convalida di tipo di dati (integer, data e così via) non vengono eseguiti nel client.</span><span class="sxs-lookup"><span data-stu-id="49949-176">In particular, data-type validation (integer, date, and so on) don't run on the client.</span></span> <span data-ttu-id="49949-177">I seguenti controlli di lavoro nel client e server:</span><span class="sxs-lookup"><span data-stu-id="49949-177">The following checks work on both the client and server:</span></span>

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

<span data-ttu-id="49949-178">In questo esempio, il test per una data valida non funzionerà nel codice client.</span><span class="sxs-lookup"><span data-stu-id="49949-178">In this example, the test for a valid date won't work in client code.</span></span> <span data-ttu-id="49949-179">Tuttavia, verrà eseguito il test nel codice server.</span><span class="sxs-lookup"><span data-stu-id="49949-179">However, the test will be performed in server code.</span></span>

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a><span data-ttu-id="49949-180">Gli errori di convalida di formattazione</span><span class="sxs-lookup"><span data-stu-id="49949-180">Formatting Validation Errors</span></span>

<span data-ttu-id="49949-181">È possibile controllare la modalità di visualizzazione degli errori di convalida la definizione delle classi CSS che hanno nomi riservati seguenti:</span><span class="sxs-lookup"><span data-stu-id="49949-181">You can control how validation errors are displayed by defining CSS classes that have the following reserved names:</span></span>

- <span data-ttu-id="49949-182">`field-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="49949-182">`field-validation-error`.</span></span> <span data-ttu-id="49949-183">Definisce l'output del `Html.ValidationMessage` metodo quando viene visualizzato un errore.</span><span class="sxs-lookup"><span data-stu-id="49949-183">Defines the output of the `Html.ValidationMessage` method when it's displaying an error.</span></span>
- <span data-ttu-id="49949-184">`field-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="49949-184">`field-validation-valid`.</span></span> <span data-ttu-id="49949-185">Definisce l'output del `Html.ValidationMessage` metodo quando non si verifica alcun errore.</span><span class="sxs-lookup"><span data-stu-id="49949-185">Defines the output of the `Html.ValidationMessage` method when there is no error.</span></span>
- <span data-ttu-id="49949-186">`input-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="49949-186">`input-validation-error`.</span></span> <span data-ttu-id="49949-187">Definisce come `<input>` gli elementi vengono visualizzati quando si verifica un errore.</span><span class="sxs-lookup"><span data-stu-id="49949-187">Defines how `<input>` elements are rendered when there's an error.</span></span> <span data-ttu-id="49949-188">(Ad esempio, è possibile utilizzare questa classe per impostare il colore di sfondo di un &lt;input&gt; elemento da un colore diverso se il relativo valore non è valido.) Questa classe CSS viene utilizzata solo durante la convalida del client (in ASP.NET Web Pages 2).</span><span class="sxs-lookup"><span data-stu-id="49949-188">(For example, you can use this class to set the background color of an &lt;input&gt; element to a different color if its value is invalid.) This CSS class is used only during client validation (in ASP.NET Web Pages 2).</span></span>
- <span data-ttu-id="49949-189">`input-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="49949-189">`input-validation-valid`.</span></span> <span data-ttu-id="49949-190">Definisce l'aspetto di `<input>` elementi quando non si è verificato alcun errore.</span><span class="sxs-lookup"><span data-stu-id="49949-190">Defines the appearance of `<input>` elements when there is no error.</span></span>
- <span data-ttu-id="49949-191">`validation-summary-errors`.</span><span class="sxs-lookup"><span data-stu-id="49949-191">`validation-summary-errors`.</span></span> <span data-ttu-id="49949-192">Definisce l'output del `Html.ValidationSummary` metodo visualizzato un elenco di errori.</span><span class="sxs-lookup"><span data-stu-id="49949-192">Defines the output of the `Html.ValidationSummary` method it's displaying a list of errors.</span></span>
- <span data-ttu-id="49949-193">`validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="49949-193">`validation-summary-valid`.</span></span> <span data-ttu-id="49949-194">Definisce l'output del `Html.ValidationSummary` metodo quando non si verifica alcun errore.</span><span class="sxs-lookup"><span data-stu-id="49949-194">Defines the output of the `Html.ValidationSummary` method when there is no error.</span></span>

<span data-ttu-id="49949-195">Nell'esempio `<style>` vengono mostrate le regole per le condizioni di errore.</span><span class="sxs-lookup"><span data-stu-id="49949-195">The following `<style>` block shows rules for error conditions.</span></span>

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

<span data-ttu-id="49949-196">Se si include questo stile di blocco nelle pagine di esempio da in questo articolo, la visualizzazione dell'errore sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="49949-196">If you include this style block in the example pages from earlier in the article, the error display will look like the following illustration:</span></span>

![Errori di convalida che usano le classi di stile CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="49949-198">Se non si utilizza la convalida del client in ASP.NET Web Pages 2, le classi CSS per il `<input>` elementi (`input-validation-error` e `input-validation-valid` non ha alcun effetto.</span><span class="sxs-lookup"><span data-stu-id="49949-198">If you're not using client validation in ASP.NET Web Pages 2, the CSS classes for the `<input>` elements (`input-validation-error` and `input-validation-valid` don't have any effect.</span></span>


### <a name="static-and-dynamic-error-display"></a><span data-ttu-id="49949-199">Visualizzazione degli errori statici e dinamici</span><span class="sxs-lookup"><span data-stu-id="49949-199">Static and Dynamic Error Display</span></span>

<span data-ttu-id="49949-200">Le regole CSS entrano nelle coppie, ad esempio `validation-summary-errors` e `validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="49949-200">The CSS rules come in pairs, such as `validation-summary-errors` and `validation-summary-valid`.</span></span> <span data-ttu-id="49949-201">Queste coppie consentono di definire le regole per entrambe le condizioni: una condizione di errore e una condizione (non degli errori) "normale".</span><span class="sxs-lookup"><span data-stu-id="49949-201">These pairs let you define rules for both conditions: an error condition and a "normal" (non-error) condition.</span></span> <span data-ttu-id="49949-202">È importante comprendere che il markup per la visualizzazione degli errori viene sempre eseguito, anche se non sono presenti errori.</span><span class="sxs-lookup"><span data-stu-id="49949-202">It's important to understand that the markup for the error display is always rendered, even if there are no errors.</span></span> <span data-ttu-id="49949-203">Ad esempio, se una pagina contiene un `Html.ValidationSummary` metodo nel codice, l'origine della pagina conterrà il markup seguente anche quando la pagina viene richiesta per la prima volta:</span><span class="sxs-lookup"><span data-stu-id="49949-203">For example, if a page has an `Html.ValidationSummary` method in the markup, the page source will contain the following markup even when the page is requested for the first time:</span></span>

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

<span data-ttu-id="49949-204">In altre parole, il `Html.ValidationSummary` metodo esegue il rendering sempre un `<div>` elemento e un elenco, anche se l'elenco degli errori è vuoto.</span><span class="sxs-lookup"><span data-stu-id="49949-204">In other words, the `Html.ValidationSummary` method always renders a `<div>` element and a list, even if the error list is empty.</span></span> <span data-ttu-id="49949-205">Analogamente, il `Html.ValidationMessage` metodo esegue il rendering sempre un `<span>` elemento come segnaposto per un errore di singoli campi, anche se non si verificano errori.</span><span class="sxs-lookup"><span data-stu-id="49949-205">Similarly, the `Html.ValidationMessage` method always renders a `<span>` element as a placeholder for an individual field error, even if there is no error.</span></span>

<span data-ttu-id="49949-206">In alcuni casi, la visualizzazione di un messaggio di errore possono utilizzare la pagina per ridisporre e può causare gli elementi nella pagina per spostarsi all'interno.</span><span class="sxs-lookup"><span data-stu-id="49949-206">In some situations, displaying an error message can cause the page to reflow and can cause elements on the page to move around.</span></span> <span data-ttu-id="49949-207">Le regole CSS che terminano con `-valid` consentono di definire un layout che consente di evitare questo problema.</span><span class="sxs-lookup"><span data-stu-id="49949-207">The CSS rules that end in `-valid` let you define a layout that can help prevent this problem.</span></span> <span data-ttu-id="49949-208">Ad esempio, è possibile definire `field-validation-error` e `field-validation-valid` sia hanno la stessa dimensione fissa.</span><span class="sxs-lookup"><span data-stu-id="49949-208">For example, you can define `field-validation-error` and `field-validation-valid` to both have the same fixed size.</span></span> <span data-ttu-id="49949-209">In questo modo, l'area di visualizzazione per il campo è statico e non modifica il flusso di pagina, se viene visualizzato un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="49949-209">That way, the display area for the field is static and won't change the page flow if an error message is displayed.</span></span>

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a><span data-ttu-id="49949-210">Convalida dei dati che non provengano direttamente dagli utenti</span><span class="sxs-lookup"><span data-stu-id="49949-210">Validating Data That Doesn't Come Directly from Users</span></span>

<span data-ttu-id="49949-211">A volte è necessario convalidare le informazioni che non provengano direttamente da un form HTML.</span><span class="sxs-lookup"><span data-stu-id="49949-211">Sometimes you have to validate information that doesn't come directly from an HTML form.</span></span> <span data-ttu-id="49949-212">Un esempio tipico è una pagina in cui viene passato un valore in una stringa di query, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="49949-212">A typical example is a page where a value is passed in a query string, as in the following example:</span></span>

`http://server/myapp/EditClassInformation?classid=1022`

<span data-ttu-id="49949-213">In questo caso, si desidera assicurarsi che il valore passato alla pagina (qui, 1022 per il valore di `classid`) è valido.</span><span class="sxs-lookup"><span data-stu-id="49949-213">In this case, you want to make sure that the value that's passed to the page (here, 1022 for the value of `classid`) is valid.</span></span> <span data-ttu-id="49949-214">È possibile utilizzare direttamente la `Validation` helper per eseguire la convalida.</span><span class="sxs-lookup"><span data-stu-id="49949-214">You can't directly use the `Validation` helper to perform this validation.</span></span> <span data-ttu-id="49949-215">Tuttavia, è possibile utilizzare altre funzionalità del sistema di convalida, ad esempio la possibilità di visualizzare i messaggi di errore di convalida.</span><span class="sxs-lookup"><span data-stu-id="49949-215">However, you can use other features of the validation system, like the ability to display validation error messages.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="49949-216">**Importante** convalidare sempre i valori che vengono recuperate dal *qualsiasi* origine, inclusi i valori del campo del form, i valori di stringa di query e valori dei cookie.</span><span class="sxs-lookup"><span data-stu-id="49949-216">**Important** Always validate values that you get from *any* source, including form-field values, query-string values, and cookie values.</span></span> <span data-ttu-id="49949-217">È facile per gli utenti di modificare questi valori (ad esempio, per finalità dannose).</span><span class="sxs-lookup"><span data-stu-id="49949-217">It's easy for people to change these values (perhaps for malicious purposes).</span></span> <span data-ttu-id="49949-218">È pertanto necessario verificare questi valori per proteggere l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="49949-218">So you must check these values in order to protect your application.</span></span>


<span data-ttu-id="49949-219">Nell'esempio seguente viene illustrato come convalidare un valore che viene passato una stringa di query.</span><span class="sxs-lookup"><span data-stu-id="49949-219">The following example shows how you might validate a value that's passed in a query string.</span></span> <span data-ttu-id="49949-220">Il codice verifica che il valore non sia vuoto e che sia un numero intero.</span><span class="sxs-lookup"><span data-stu-id="49949-220">The code tests that the value is not empty and that it's an integer.</span></span>

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

<span data-ttu-id="49949-221">Si noti che il test viene eseguito quando la richiesta non è un invio del modulo (`if(!IsPost)`).</span><span class="sxs-lookup"><span data-stu-id="49949-221">Notice that the test is performed when the request is not a form submission (`if(!IsPost)`).</span></span> <span data-ttu-id="49949-222">La prima volta che viene richiesta la pagina passa questo test, ma non quando la richiesta è di invio di un form.</span><span class="sxs-lookup"><span data-stu-id="49949-222">This test would pass the first time that the page is requested, but not when the request is a form submission.</span></span>

<span data-ttu-id="49949-223">Per visualizzare questo errore, è possibile aggiungere l'errore all'elenco di errori di convalida chiamando `Validation.AddFormError("message")`.</span><span class="sxs-lookup"><span data-stu-id="49949-223">To display this error, you can add the error to the list of validation errors by calling `Validation.AddFormError("message")`.</span></span> <span data-ttu-id="49949-224">Se la pagina contiene una chiamata al `Html.ValidationSummary` (metodo), l'errore viene visualizzato, come un errore di convalida dell'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="49949-224">If the page contains a call to the `Html.ValidationSummary` method, the error is displayed there, just like a user-input validation error.</span></span>

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="49949-225">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="49949-225">Additional Resources</span></span>

[<span data-ttu-id="49949-226">Utilizzo di form HTML nei siti di pagine Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="49949-226">Working with HTML Forms in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkID=202892)
