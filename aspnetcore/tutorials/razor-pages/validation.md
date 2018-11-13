---
title: Aggiungere la convalida a una pagina Razor ASP.NET Core
author: rick-anderson
description: Informazioni su come aggiungere la convalida a una pagina Razor in ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages/validation
ms.openlocfilehash: d4cc0ab9de314c0c5a1a9016efd1e566ff1c47d2
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505778"
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a><span data-ttu-id="03ade-103">Aggiungere la convalida a una pagina Razor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="03ade-103">Add validation to an ASP.NET Core Razor Page</span></span>

<span data-ttu-id="03ade-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="03ade-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="03ade-105">In questa sezione la logica di convalida viene aggiunta al modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="03ade-105">In this section, validation logic is added to the `Movie` model.</span></span> <span data-ttu-id="03ade-106">Le regole di convalida vengono applicate ogni volta che un utente crea o modifica un film.</span><span class="sxs-lookup"><span data-stu-id="03ade-106">The validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="validation"></a><span data-ttu-id="03ade-107">Convalida</span><span class="sxs-lookup"><span data-stu-id="03ade-107">Validation</span></span>

<span data-ttu-id="03ade-108">Un concetto di base dello sviluppo del software si chiama [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself", Non ripeterti).</span><span class="sxs-lookup"><span data-stu-id="03ade-108">A key tenet of software development is called [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself").</span></span> <span data-ttu-id="03ade-109">Le pagine Razor promuovono lo sviluppo in cui la funzionalità è stata specificata una volta e questa modifica è riflessa sull'intera app.</span><span class="sxs-lookup"><span data-stu-id="03ade-109">Razor Pages encourages development where functionality is specified once, and it's reflected throughout the app.</span></span> <span data-ttu-id="03ade-110">DRY consente di ridurre la quantità di codice in un'app.</span><span class="sxs-lookup"><span data-stu-id="03ade-110">DRY can help reduce the amount of code in an app.</span></span> <span data-ttu-id="03ade-111">DRY rende il codice meno soggetto ad errori ed è più facile da testare e gestire.</span><span class="sxs-lookup"><span data-stu-id="03ade-111">DRY makes the code less error prone, and easier to test and maintain.</span></span>

<span data-ttu-id="03ade-112">Il supporto della convalida fornito dalle pagine di Razor e da Entity Framework è un buon esempio del principio di DRY.</span><span class="sxs-lookup"><span data-stu-id="03ade-112">The validation support provided by Razor Pages and Entity Framework is a good example of the DRY principle.</span></span> <span data-ttu-id="03ade-113">Le regole di convalida vengono specificate in modo dichiarativo in un'unica posizione (nella classe del modello) e le regole vengono applicate ovunque nell'app.</span><span class="sxs-lookup"><span data-stu-id="03ade-113">Validation rules are declaratively specified in one place (in the model class), and the rules are enforced everywhere in the app.</span></span>

### <a name="adding-validation-rules-to-the-movie-model"></a><span data-ttu-id="03ade-114">Aggiunta di regole di convalida al modello movie</span><span class="sxs-lookup"><span data-stu-id="03ade-114">Adding validation rules to the movie model</span></span>

<span data-ttu-id="03ade-115">Aprire il file *Models/Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="03ade-115">Open the *Models/Movie.cs* file.</span></span> <span data-ttu-id="03ade-116">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) fornisce un set incorporato di attributi di convalida che si applicano in modo dichiarativo a una classe o proprietà.</span><span class="sxs-lookup"><span data-stu-id="03ade-116">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) provides a built-in set of validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="03ade-117">DataAnnotations contiene anche gli attributi di formattazione come ad esempio `DataType` che guidano nella formattazione e non forniscono la convalida.</span><span class="sxs-lookup"><span data-stu-id="03ade-117">DataAnnotations also contains formatting attributes like `DataType` that help with formatting and don't provide validation.</span></span>

<span data-ttu-id="03ade-118">Aggiornamento della classe `Movie` per poter sfruttare gli attributi di convalida `Required`, `StringLength`, `RegularExpression`, e `Range`.</span><span class="sxs-lookup"><span data-stu-id="03ade-118">Update the `Movie` class to take advantage of the `Required`, `StringLength`, `RegularExpression`, and `Range` validation attributes.</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRatingDA.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="03ade-119">Gli attributi di convalida specificano il comportamento che viene applicato alle proprietà del modello:</span><span class="sxs-lookup"><span data-stu-id="03ade-119">Validation attributes specify behavior that's enforced on model properties:</span></span>

* <span data-ttu-id="03ade-120">Gli attributi `Required` e `MinimumLength` indicano che una proprietà deve avere un valore.</span><span class="sxs-lookup"><span data-stu-id="03ade-120">The `Required` and `MinimumLength` attributes indicate that a property must have a value.</span></span> <span data-ttu-id="03ade-121">Tuttavia, niente impedisce a un utente di immettere spazi vuoti per soddisfare il vincolo di convalida per un tipo nullable.</span><span class="sxs-lookup"><span data-stu-id="03ade-121">However, nothing prevents a user from entering whitespace to satisfy the validation constraint for a nullable type.</span></span> <span data-ttu-id="03ade-122">I [tipi valore](/dotnet/csharp/language-reference/keywords/value-types) non nullable (ad esempio `decimal`, `int`, `float` e `DateTime`) sono intrinsecamente necessari e non richiedono l'attributo `Required`.</span><span class="sxs-lookup"><span data-stu-id="03ade-122">Non-nullable [value types](/dotnet/csharp/language-reference/keywords/value-types) (such as `decimal`, `int`, `float`, and `DateTime`) are inherently required and don't need the `Required` attribute.</span></span>
* <span data-ttu-id="03ade-123">L'attributo `RegularExpression` limita i caratteri che può immettere l'utente.</span><span class="sxs-lookup"><span data-stu-id="03ade-123">The `RegularExpression` attribute limits the characters that the user can enter.</span></span> <span data-ttu-id="03ade-124">Nel codice precedente `Genre` deve iniziare con una o più lettere maiuscole seguite da zero o più lettere, virgolette singole o doppie, spazi vuoti o trattini.</span><span class="sxs-lookup"><span data-stu-id="03ade-124">In the preceding code, `Genre` must start with one or more capital letters and follow with zero or more letters, single or double quotes, whitespace characters, or dashes.</span></span> <span data-ttu-id="03ade-125">`Rating` deve iniziare con una o più lettere maiuscole seguite da zero o più lettere, numeri, virgolette singole o doppie, spazi vuoti o trattini.</span><span class="sxs-lookup"><span data-stu-id="03ade-125">`Rating` must start with one or more capital letters and follow with zero or more letters, numbers, single or double quotes, whitespace characters, or dashes.</span></span>
* <span data-ttu-id="03ade-126">L'attributo `Range` vincola un valore a un intervallo specificato.</span><span class="sxs-lookup"><span data-stu-id="03ade-126">The `Range` attribute constrains a value to a specified range.</span></span>
* <span data-ttu-id="03ade-127">L'attributo `StringLength` imposta la lunghezza massima di una stringa e, facoltativamente, la lunghezza minima.</span><span class="sxs-lookup"><span data-stu-id="03ade-127">The `StringLength` attribute sets the maximum length of a string, and optionally the minimum length.</span></span> 

<span data-ttu-id="03ade-128">Il possesso di regole di convalida applicate automaticamente da ASP.NET Core consente di rendere un'app più solida.</span><span class="sxs-lookup"><span data-stu-id="03ade-128">Having validation rules automatically enforced by ASP.NET Core helps make an app more robust.</span></span> <span data-ttu-id="03ade-129">La convalida automatica sui modelli consente di proteggere l'app perché non è necessario ricordarsi di applicarli quando viene aggiunto nuovo codice.</span><span class="sxs-lookup"><span data-stu-id="03ade-129">Automatic validation on models helps protect the app because you don't have to remember to apply them when new code is added.</span></span>

### <a name="validation-error-ui-in-razor-pages"></a><span data-ttu-id="03ade-130">Errore di convalida dell'interfaccia utente nelle pagine Razor</span><span class="sxs-lookup"><span data-stu-id="03ade-130">Validation Error UI in Razor Pages</span></span>

<span data-ttu-id="03ade-131">Eseguire l'app e passare a pagine/film.</span><span class="sxs-lookup"><span data-stu-id="03ade-131">Run the app and navigate to Pages/Movies.</span></span>

<span data-ttu-id="03ade-132">Selezionare il collegamento **Crea nuovo**.</span><span class="sxs-lookup"><span data-stu-id="03ade-132">Select the **Create New** link.</span></span> <span data-ttu-id="03ade-133">Completare il modulo con alcuni valori non validi.</span><span class="sxs-lookup"><span data-stu-id="03ade-133">Complete the form with some invalid values.</span></span> <span data-ttu-id="03ade-134">Quando la convalida del lato client jQuery rileva l'errore, viene visualizzato un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="03ade-134">When jQuery client-side validation detects the error, it displays an error message.</span></span>

![Il modulo di vista del film con più errori di convalida del lato client jQuery](validation/_static/val.png)

[!INCLUDE[](~/includes/currency.md)]

<span data-ttu-id="03ade-136">Si noti come il modulo ha eseguito automaticamente il rendering di un messaggio di errore di convalida in ogni campo che contiene un valore non valido.</span><span class="sxs-lookup"><span data-stu-id="03ade-136">Notice how the form has automatically rendered a validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="03ade-137">Gli errori vengono applicati sia sul lato client (uso di JavaScript e jQuery) sia sul lato server (quando un utente ha JavaScript disabilitato).</span><span class="sxs-lookup"><span data-stu-id="03ade-137">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (when a user has JavaScript disabled).</span></span>

<span data-ttu-id="03ade-138">Un vantaggio significativo è che non c'era **nessuna** modifica del codice necessaria nelle pagine Crea o Modifica.</span><span class="sxs-lookup"><span data-stu-id="03ade-138">A significant benefit is that **no** code changes were necessary in the Create  or Edit pages.</span></span> <span data-ttu-id="03ade-139">Una volta che le DataAnnotations sono state applicate al modello, è stata abilitata la convalida dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="03ade-139">Once DataAnnotations were applied to the model, the validation UI was enabled.</span></span> <span data-ttu-id="03ade-140">Le pagine Razor create in questa esercitazione hanno selezionato automaticamente le regole di convalida (usando gli attributi di convalida delle proprietà della classe `Movie` del modello).</span><span class="sxs-lookup"><span data-stu-id="03ade-140">The Razor Pages created in this tutorial automatically picked up the validation rules (using validation attributes on the properties of the `Movie` model class).</span></span> <span data-ttu-id="03ade-141">Convalida del test tramite la pagina Modifica, viene applicata la stessa convalida.</span><span class="sxs-lookup"><span data-stu-id="03ade-141">Test validation using the Edit page, the same validation is applied.</span></span>

<span data-ttu-id="03ade-142">I dati del modulo non vengono registrati nel server fino a quando non sono presenti errori di convalida nel lato client.</span><span class="sxs-lookup"><span data-stu-id="03ade-142">The form data isn't posted to the server until there are no client-side validation errors.</span></span> <span data-ttu-id="03ade-143">Verificare che i dati del modulo non siano stati registrati da uno o più degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="03ade-143">Verify form data isn't posted by one or more of the following approaches:</span></span>

* <span data-ttu-id="03ade-144">Inserire un punto di interruzione nel metodo `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="03ade-144">Put a break point in the `OnPostAsync` method.</span></span> <span data-ttu-id="03ade-145">Inviare il modulo (selezionare **Crea** o **Salva**).</span><span class="sxs-lookup"><span data-stu-id="03ade-145">Submit the form (select **Create** or **Save**).</span></span> <span data-ttu-id="03ade-146">Il punto di interruzione non viene mai raggiunto.</span><span class="sxs-lookup"><span data-stu-id="03ade-146">The break point is never hit.</span></span>
* <span data-ttu-id="03ade-147">Usare lo [Strumento Fiddler](http://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="03ade-147">Use the [Fiddler tool](http://www.telerik.com/fiddler).</span></span>
* <span data-ttu-id="03ade-148">Usare gli strumenti di sviluppo del browser per monitorare il traffico di rete.</span><span class="sxs-lookup"><span data-stu-id="03ade-148">Use the browser developer tools to monitor network traffic.</span></span>

### <a name="server-side-validation"></a><span data-ttu-id="03ade-149">Convalida sul lato server</span><span class="sxs-lookup"><span data-stu-id="03ade-149">Server-side validation</span></span>

<span data-ttu-id="03ade-150">Quando JavaScript è disabilitato nel browser, l'invio del modulo con errori verrà inoltrato al server.</span><span class="sxs-lookup"><span data-stu-id="03ade-150">When JavaScript is disabled in the browser, submitting the form with errors will post to the server.</span></span>

<span data-ttu-id="03ade-151">Facoltativo, convalida sul lato server del test:</span><span class="sxs-lookup"><span data-stu-id="03ade-151">Optional, test server-side validation:</span></span>

* <span data-ttu-id="03ade-152">Disabilitare JavaScript nel browser.</span><span class="sxs-lookup"><span data-stu-id="03ade-152">Disable JavaScript in the browser.</span></span> <span data-ttu-id="03ade-153">Per eseguire questa operazione, usare gli strumenti di sviluppo del browser.</span><span class="sxs-lookup"><span data-stu-id="03ade-153">You can do this using your browser's developer tools.</span></span> <span data-ttu-id="03ade-154">Se non è possibile disabilitare JavaScript nel browser, provare un altro browser.</span><span class="sxs-lookup"><span data-stu-id="03ade-154">If you can't disable JavaScript in the browser, try another browser.</span></span>
* <span data-ttu-id="03ade-155">Impostare un punto di interruzione nel metodo `OnPostAsync` della pagina Crea o Modifica.</span><span class="sxs-lookup"><span data-stu-id="03ade-155">Set a break point in the `OnPostAsync` method of the Create or Edit page.</span></span>
* <span data-ttu-id="03ade-156">Invio di un form con errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="03ade-156">Submit a form with validation errors.</span></span>
* <span data-ttu-id="03ade-157">Verificare che lo stato del modello non sia valido:</span><span class="sxs-lookup"><span data-stu-id="03ade-157">Verify the model state is invalid:</span></span>

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

<span data-ttu-id="03ade-158">Il codice seguente mostra una porzione della pagina *Create.cshtml* di cui è stato eseguito lo scaffolding in precedenza nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="03ade-158">The following code shows a portion of the *Create.cshtml* page that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="03ade-159">Viene usato dalle pagine Creazione e Modifica per visualizzare il modulo iniziale e per visualizzare nuovamente il modulo in caso di errore.</span><span class="sxs-lookup"><span data-stu-id="03ade-159">It's used by the Create and Edit pages to display the initial form and to redisplay the form in the event of an error.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

<span data-ttu-id="03ade-160">L'[helper tag di input](xref:mvc/views/working-with-forms) usa gli attributi [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) e produce gli attributi HTML necessari per la convalida jQuery sul lato client.</span><span class="sxs-lookup"><span data-stu-id="03ade-160">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span> <span data-ttu-id="03ade-161">L'[helper tag di convalida](xref:mvc/views/working-with-forms#the-validation-tag-helpers) visualizza gli errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="03ade-161">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="03ade-162">Per altre informazioni, vedere [Convalida](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="03ade-162">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="03ade-163">Le pagine Crea e Modifica non dispongono di nessuna regola di convalida.</span><span class="sxs-lookup"><span data-stu-id="03ade-163">The Create and Edit pages have no validation rules in them.</span></span> <span data-ttu-id="03ade-164">Le regole di convalida e le stringhe di errore vengono specificate solo nella classe `Movie`.</span><span class="sxs-lookup"><span data-stu-id="03ade-164">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="03ade-165">Queste regole di convalida vengono applicate automaticamente alle pagine Razor che modificano il modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="03ade-165">These validation rules are automatically applied to Razor Pages that edit the `Movie` model.</span></span>

<span data-ttu-id="03ade-166">Quando la logica di convalida deve cambiare, avviene solo nel modello.</span><span class="sxs-lookup"><span data-stu-id="03ade-166">When validation logic needs to change, it's done only in the model.</span></span> <span data-ttu-id="03ade-167">La convalida viene applicata in modo coerente in tutta l'applicazione (la logica di convalida è definita in un'unica posizione).</span><span class="sxs-lookup"><span data-stu-id="03ade-167">Validation is applied consistently throughout the application (validation logic is defined in one place).</span></span> <span data-ttu-id="03ade-168">La convalida in un'unica posizione consente di mantenere il codice pulito e rende più semplice la gestione e l'aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="03ade-168">Validation in one place helps keep the code clean, and makes it easier to maintain and update.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="03ade-169">Utilizzo degli attributi DataType</span><span class="sxs-lookup"><span data-stu-id="03ade-169">Using DataType Attributes</span></span>

<span data-ttu-id="03ade-170">Esaminare la classe `Movie`.</span><span class="sxs-lookup"><span data-stu-id="03ade-170">Examine the `Movie` class.</span></span> <span data-ttu-id="03ade-171">Lo spazio dei nomi `System.ComponentModel.DataAnnotations` fornisce gli attributi di formattazione oltre al set predefinito di attributi di convalida.</span><span class="sxs-lookup"><span data-stu-id="03ade-171">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="03ade-172">L'attributo `DataType` viene applicato alle proprietà `ReleaseDate` e `Price`.</span><span class="sxs-lookup"><span data-stu-id="03ade-172">The `DataType` attribute is applied to the `ReleaseDate` and `Price` properties.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

<span data-ttu-id="03ade-173">Gli attributi `DataType` forniscono solo gli hint per far sì che il motore di vista formatti i dati (e fornisca gli attributi, ad esempio `<a>` per gli URL e `<a href="mailto:EmailAddress.com">` per la posta elettronica).</span><span class="sxs-lookup"><span data-stu-id="03ade-173">The `DataType` attributes only provide hints for the view engine to format the data (and supplies attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email).</span></span> <span data-ttu-id="03ade-174">Usare l'attributo `RegularExpression` per convalidare il formato dei dati.</span><span class="sxs-lookup"><span data-stu-id="03ade-174">Use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="03ade-175">L'attributo `DataType` viene usato per specificare un tipo di dati che è più specifico del tipo intrinseco del database.</span><span class="sxs-lookup"><span data-stu-id="03ade-175">The `DataType` attribute is used to specify a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="03ade-176">Gli attributi `DataType` non sono gli attributi di convalida.</span><span class="sxs-lookup"><span data-stu-id="03ade-176">`DataType` attributes are not validation attributes.</span></span> <span data-ttu-id="03ade-177">Nell'applicazione di esempio, viene visualizzata solo la data, senza l'ora.</span><span class="sxs-lookup"><span data-stu-id="03ade-177">In the sample application, only the date is displayed, without time.</span></span>

<span data-ttu-id="03ade-178">L'enumerazione `DataType` fornisce per molti tipi di dati, ad esempio Data, Ora, Numero di telefono, Valuta, Indirizzo di posta elettronica e altro ancora.</span><span class="sxs-lookup"><span data-stu-id="03ade-178">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="03ade-179">L'attributo `DataType` può anche consentire all'applicazione di fornire automaticamente le funzionalità specifiche del tipo.</span><span class="sxs-lookup"><span data-stu-id="03ade-179">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="03ade-180">Ad esempio, è possibile creare un collegamento `mailto:` per `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="03ade-180">For example, a `mailto:` link can be created for `DataType.EmailAddress`.</span></span> <span data-ttu-id="03ade-181">È possibile fornire un selettore di dati per `DataType.Date` nei browser che supportano HTML5.</span><span class="sxs-lookup"><span data-stu-id="03ade-181">A date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="03ade-182">Gli attributi `DataType` generano attributi di HTML 5 `data-` (dash di dati pronunciati) che usano browser di HTML 5.</span><span class="sxs-lookup"><span data-stu-id="03ade-182">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="03ade-183">Gli attributi `DataType` **non** forniscono alcuna convalida.</span><span class="sxs-lookup"><span data-stu-id="03ade-183">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="03ade-184">`DataType.Date` non specifica il formato della data visualizzata.</span><span class="sxs-lookup"><span data-stu-id="03ade-184">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="03ade-185">Per impostazione predefinita, il campo dei dati viene visualizzato in base ai formati predefiniti per il valore `CultureInfo` del server.</span><span class="sxs-lookup"><span data-stu-id="03ade-185">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="03ade-186">L'annotazione dei dati `[Column(TypeName = "decimal(18, 2)")]` è necessaria per consentire a Entity Framework Core di eseguire correttamente il mapping di `Price` nella valuta del database.</span><span class="sxs-lookup"><span data-stu-id="03ade-186">The `[Column(TypeName = "decimal(18, 2)")]` data annotation is required so Entity Framework Core can correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="03ade-187">Per altre informazioni, vedere [Tipi di dati](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="03ade-187">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

::: moniker-end

<span data-ttu-id="03ade-188">L'attributo `DisplayFormat` viene usato per specificare in modo esplicito il formato della data:</span><span class="sxs-lookup"><span data-stu-id="03ade-188">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="03ade-189">L'impostazione `ApplyFormatInEditMode` specifica che deve essere applicata la formattazione quando il valore viene visualizzato per la modifica.</span><span class="sxs-lookup"><span data-stu-id="03ade-189">The `ApplyFormatInEditMode` setting specifies that the formatting should be applied when the value is displayed for editing.</span></span> <span data-ttu-id="03ade-190">Questo comportamento potrebbe non essere desiderato per alcuni campi.</span><span class="sxs-lookup"><span data-stu-id="03ade-190">You might not want that behavior for some fields.</span></span> <span data-ttu-id="03ade-191">Ad esempio, in valori di valuta, è probabile che non si desideri il simbolo di valuta in modalità di modifica dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="03ade-191">For example, in currency values, you probably don't want the currency symbol in the edit UI.</span></span>

<span data-ttu-id="03ade-192">L'attributo `DisplayFormat` può essere usato da solo, ma in genere è consigliabile usare l'attributo `DataType`.</span><span class="sxs-lookup"><span data-stu-id="03ade-192">The `DisplayFormat` attribute can be used by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="03ade-193">L'attributo `DataType` fornisce la semantica dei dati anziché la modalità di esecuzione del rendering in una schermata e fornisce i seguenti vantaggi che non si ottengono con DisplayFormat:</span><span class="sxs-lookup"><span data-stu-id="03ade-193">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="03ade-194">Il browser può abilitare le funzionalità HTML5, ad esempio visualizzare un controllo di calendario, il simbolo della valuta appropriato per le impostazioni locali, i collegamenti alla posta elettronica e così via.</span><span class="sxs-lookup"><span data-stu-id="03ade-194">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>
* <span data-ttu-id="03ade-195">Per impostazione predefinita, il browser eseguirà il rendering dei dati usando il formato corretto in base alle impostazioni locali del sistema.</span><span class="sxs-lookup"><span data-stu-id="03ade-195">By default, the browser will render data using the correct format based on your locale.</span></span>
* <span data-ttu-id="03ade-196">L'attributo `DataType` può abilitare il framework di ASP.NET Core a scegliere il modello di campo corretto per il rendering dei dati.</span><span class="sxs-lookup"><span data-stu-id="03ade-196">The `DataType` attribute can enable the ASP.NET Core framework to choose the right field template to render the data.</span></span> <span data-ttu-id="03ade-197">Il `DisplayFormat` se usato da solo usa il modello di stringa.</span><span class="sxs-lookup"><span data-stu-id="03ade-197">The `DisplayFormat` if used by itself uses the string template.</span></span>

<span data-ttu-id="03ade-198">Nota: la convalida jQuery non funziona con l'attributo `Range` e con `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="03ade-198">Note: jQuery validation doesn't work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="03ade-199">Ad esempio, il codice seguente visualizzerà sempre un errore di convalida sul lato client, anche quando la data è compreso nell'intervallo specificato:</span><span class="sxs-lookup"><span data-stu-id="03ade-199">For example, the following code will always display a client-side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="03ade-200">In genere non è consigliabile compilare date reali nei modelli, quindi l'uso dell'attributo `Range` e di `DateTime` è sconsigliato.</span><span class="sxs-lookup"><span data-stu-id="03ade-200">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="03ade-201">Il codice seguente illustra la combinazione di attributi in una sola riga:</span><span class="sxs-lookup"><span data-stu-id="03ade-201">The following code shows combining attributes on one line:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="03ade-202">[Introduzione a Razor Pages ed Entity Framework Core](xref:data/ef-rp/intro) include operazioni avanzate di Entity Framework Core con Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="03ade-202">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) shows advanced EF Core operations with Razor Pages.</span></span>

### <a name="publish-to-azure"></a><span data-ttu-id="03ade-203">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="03ade-203">Publish to Azure</span></span>

<span data-ttu-id="03ade-204">Per informazioni sulla distribuzione in Azure, vedere [Esercitazione: Creare un'app ASP.NET in Azure con un database SQL](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase).</span><span class="sxs-lookup"><span data-stu-id="03ade-204">For information on deploying to Azure, See [Tutorial: Build an ASP.NET app in Azure with SQL Database](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase).</span></span> <span data-ttu-id="03ade-205">Le istruzioni sono relative a un'app ASP.NET, non a un'app ASP.NET Core, ma i passaggi sono identici.</span><span class="sxs-lookup"><span data-stu-id="03ade-205">These instructions are for an ASP.NET app, not an ASP.NET Core app, but the steps are the same.</span></span>

<span data-ttu-id="03ade-206">Grazie aver completato questa introduzione alle pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="03ade-206">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="03ade-207">I suggerimenti degli utenti sono importanti.</span><span class="sxs-lookup"><span data-stu-id="03ade-207">We appreciate feedback.</span></span> <span data-ttu-id="03ade-208">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) (Introduzione a Razor Pages ed Entity Framework Core) è un complemento ideale per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="03ade-208">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="03ade-209">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="03ade-209">Additional resources</span></span>

* <xref:mvc/views/working-with-forms>
* <xref:fundamentals/localization>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/authoring>

> [!div class="step-by-step"]
> [<span data-ttu-id="03ade-210">Precedente: Aggiunta di un nuovo campo</span><span class="sxs-lookup"><span data-stu-id="03ade-210">Previous: Adding a new field</span></span>](xref:tutorials/razor-pages/new-field)
