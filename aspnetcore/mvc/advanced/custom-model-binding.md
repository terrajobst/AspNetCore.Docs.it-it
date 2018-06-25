---
title: Associazione di modelli personalizzata in ASP.NET Core
author: ardalis
description: Informazioni su come l'associazione di modelli consente alle azioni del controller di funzionare direttamente con i tipi di modello in ASP.NET Core.
ms.author: riande
ms.date: 04/10/2017
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: f5bd9a3eefb1fd9c1534e8767ad8e8af37514adb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275393"
---
# <a name="custom-model-binding-in-aspnet-core"></a><span data-ttu-id="036d8-103">Associazione di modelli personalizzata in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="036d8-103">Custom Model Binding in ASP.NET Core</span></span>

<span data-ttu-id="036d8-104">Di [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="036d8-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="036d8-105">Mediante l'associazione di modelli le azioni dei controller possono operare direttamente con i tipi di modello (passati come argomenti dei metodi), anziché con le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="036d8-105">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="036d8-106">Il mapping tra i dati delle richieste in ingresso e i modelli applicativi è gestito dagli strumenti di associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="036d8-106">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="036d8-107">Gli sviluppatori possono estendere la funzionalità di associazione di modelli predefinita implementando strumenti di associazione di modelli personalizzati (anche se in genere non è necessario creare un provider personalizzato).</span><span class="sxs-lookup"><span data-stu-id="036d8-107">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="036d8-108">Visualizzare o scaricare un esempio da GitHub</span><span class="sxs-lookup"><span data-stu-id="036d8-108">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="036d8-109">Limiti degli strumenti di associazione di modelli predefiniti</span><span class="sxs-lookup"><span data-stu-id="036d8-109">Default model binder limitations</span></span>

<span data-ttu-id="036d8-110">Gli strumenti di associazione di modelli predefiniti supportano la maggior parte dei tipi di dati .NET Core comuni e soddisfano le esigenze della maggior parte degli sviluppatori.</span><span class="sxs-lookup"><span data-stu-id="036d8-110">The default model binders support most of the common .NET Core data types and should meet most developers\` needs.</span></span> <span data-ttu-id="036d8-111">Tali strumenti associano direttamente l'input di testo della richiesta ai tipi di modello.</span><span class="sxs-lookup"><span data-stu-id="036d8-111">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="036d8-112">Può essere necessario trasformare l'input prima dell'associazione.</span><span class="sxs-lookup"><span data-stu-id="036d8-112">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="036d8-113">Un esempio è il caso in cui è presente una chiave che può essere usata per cercare i dati del modello.</span><span class="sxs-lookup"><span data-stu-id="036d8-113">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="036d8-114">È possibile usare uno strumento di associazione di modelli personalizzato per il recupero di dati in base alla chiave.</span><span class="sxs-lookup"><span data-stu-id="036d8-114">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="036d8-115">Analisi dell'associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="036d8-115">Model binding review</span></span>

<span data-ttu-id="036d8-116">L'associazione di modelli usa definizioni specifiche per i tipi sui quali opera.</span><span class="sxs-lookup"><span data-stu-id="036d8-116">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="036d8-117">Un *tipo semplice* viene convertito da una stringa singola dell'input.</span><span class="sxs-lookup"><span data-stu-id="036d8-117">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="036d8-118">Un *tipo complesso* viene convertito da più valori di input.</span><span class="sxs-lookup"><span data-stu-id="036d8-118">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="036d8-119">Il framework determina la differenza in base all'esistenza di un elemento `TypeConverter`.</span><span class="sxs-lookup"><span data-stu-id="036d8-119">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="036d8-120">È consigliabile creare un convertitore di tipi se è presente un mapping `string` -> `SomeType` semplice che non richiede risorse esterne.</span><span class="sxs-lookup"><span data-stu-id="036d8-120">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="036d8-121">Prima di creare uno strumento di associazione di modelli personalizzato, è utile esaminare le modalità di implementazione degli strumenti di associazione di modelli esistenti.</span><span class="sxs-lookup"><span data-stu-id="036d8-121">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="036d8-122">Considerare [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder), che consente di convertire le stringhe con codifica base64 in matrici di byte.</span><span class="sxs-lookup"><span data-stu-id="036d8-122">Consider the [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="036d8-123">Le matrici di byte vengono spesso archiviate come file o campi BLOB del database.</span><span class="sxs-lookup"><span data-stu-id="036d8-123">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="036d8-124">Uso di ByteArrayModelBinder</span><span class="sxs-lookup"><span data-stu-id="036d8-124">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="036d8-125">Le stringhe con codifica Base64 possono essere usate per rappresentare i dati binari.</span><span class="sxs-lookup"><span data-stu-id="036d8-125">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="036d8-126">Ad esempio l'immagine seguente può essere codificata come stringa.</span><span class="sxs-lookup"><span data-stu-id="036d8-126">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="036d8-127">![bot dotnet](custom-model-binding/images/bot.png "bot dotnet")</span><span class="sxs-lookup"><span data-stu-id="036d8-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="036d8-128">Una piccola parte della stringa codificata è visualizzata nella figura seguente:</span><span class="sxs-lookup"><span data-stu-id="036d8-128">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="036d8-129">![bot dotnet codificato](custom-model-binding/images/encoded-bot.png "bot dotnet codificato")</span><span class="sxs-lookup"><span data-stu-id="036d8-129">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="036d8-130">Seguire le istruzioni nel [file Leggimi dell'esempio](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) per convertire la stringa con codifica base64 in un file.</span><span class="sxs-lookup"><span data-stu-id="036d8-130">Follow the instructions in the [sample's README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="036d8-131">ASP.NET Core MVC può accettare una stringa con codifica base64 e usare un `ByteArrayModelBinder` per convertirla in una matrice di byte.</span><span class="sxs-lookup"><span data-stu-id="036d8-131">ASP.NET Core MVC can take a base64-encoded strings and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="036d8-132">L'elemento [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) che implementa [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) esegue il mapping degli argomenti `byte[]` a `ByteArrayModelBinder`:</span><span class="sxs-lookup"><span data-stu-id="036d8-132">The [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

```csharp
public IModelBinder GetBinder(ModelBinderProviderContext context)
{
    if (context == null)
    {
        throw new ArgumentNullException(nameof(context));
    }

    if (context.Metadata.ModelType == typeof(byte[]))
    {
        return new ByteArrayModelBinder();
    }

    return null;
}
```

<span data-ttu-id="036d8-133">Quando si crea uno strumento di associazione di modelli personalizzato è possibile implementare il proprio tipo `IModelBinderProvider` o usare [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span><span class="sxs-lookup"><span data-stu-id="036d8-133">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="036d8-134">L'esempio indica come usare `ByteArrayModelBinder` per convertire una stringa con codifica base64 in un `byte[]` e salvare il risultato in un file:</span><span class="sxs-lookup"><span data-stu-id="036d8-134">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

<span data-ttu-id="036d8-135">È possibile pubblicare (POST) una stringa con codifica base64 in questo metodo API usando uno strumento come [Postman](https://www.getpostman.com/):</span><span class="sxs-lookup"><span data-stu-id="036d8-135">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="036d8-136">![postman](custom-model-binding/images/postman.png "postman")</span><span class="sxs-lookup"><span data-stu-id="036d8-136">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="036d8-137">Se lo strumento di associazione riesce ad associare i dati della richiesta a proprietà o argomenti denominati in modo appropriato, l'associazione di modelli ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="036d8-137">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="036d8-138">L'esempio seguente indica come usare `ByteArrayModelBinder` con un modello di visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="036d8-138">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="036d8-139">Esempio di strumento di associazione di modelli personalizzato</span><span class="sxs-lookup"><span data-stu-id="036d8-139">Custom model binder sample</span></span>

<span data-ttu-id="036d8-140">In questa sezione viene implementato uno strumento di associazione di modelli personalizzato che:</span><span class="sxs-lookup"><span data-stu-id="036d8-140">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="036d8-141">Converte i dati della richiesta in ingresso in argomenti chiave fortemente tipizzati.</span><span class="sxs-lookup"><span data-stu-id="036d8-141">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="036d8-142">Usa Entity Framework Core per recuperare l'entità associata.</span><span class="sxs-lookup"><span data-stu-id="036d8-142">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="036d8-143">Passa l'entità associata come argomento al metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="036d8-143">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="036d8-144">L'esempio seguente usa l'attributo `ModelBinder` nel modello `Author`:</span><span class="sxs-lookup"><span data-stu-id="036d8-144">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

<span data-ttu-id="036d8-145">Nel codice precedente l'attributo `ModelBinder` specifica il tipo di `IModelBinder` da usare per associare i parametri di azione `Author`.</span><span class="sxs-lookup"><span data-stu-id="036d8-145">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span> 

<span data-ttu-id="036d8-146">`AuthorEntityBinder` viene usato per associare un parametro `Author` recuperando l'entità da un'origine dati tramite Entity Framework Core e un `authorId`:</span><span class="sxs-lookup"><span data-stu-id="036d8-146">The `AuthorEntityBinder` is used to bind an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

<span data-ttu-id="036d8-147">L'esempio di codice seguente indica come usare `AuthorEntityBinder` in un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="036d8-147">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="036d8-148">L'attributo `ModelBinder` può essere usato per applicare gli elementi `AuthorEntityBinder` ai parametri che non usano convenzioni predefinite:</span><span class="sxs-lookup"><span data-stu-id="036d8-148">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that don't use default conventions:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="036d8-149">In questo esempio, dato che il nome dell'argomento non è il valore `authorId` predefinito, viene specificato nel parametro usando l'attributo `ModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="036d8-149">In this example, since the name of the argument isn't the default `authorId`, it's specified on the parameter using `ModelBinder` attribute.</span></span> <span data-ttu-id="036d8-150">Si noti che sia controller sia il metodo di azione risultano più semplici rispetto alla ricerca dell'entità nel metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="036d8-150">Note that both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="036d8-151">La logica per recuperare l'autore usando Entity Framework Core viene spostata nello strumento di associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="036d8-151">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="036d8-152">Ciò può rappresentare una semplificazione notevole quando vari metodi eseguono l'associazione al modello Author e favorisce l'osservanza del [principio DRY](http://deviq.com/don-t-repeat-yourself/) (Do not Repeat Yourself, Non ripeterti).</span><span class="sxs-lookup"><span data-stu-id="036d8-152">This can be considerable simplification when you have several methods that bind to the author model, and can help you to follow the [DRY principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="036d8-153">È possibile applicare l'attributo `ModelBinder` a singole proprietà del modello (ad esempio a ViewModel) o a parametri del metodo di azione per specificare un determinato strumento di associazione di modelli o nome di modello solo per quel determinato tipo o azione.</span><span class="sxs-lookup"><span data-stu-id="036d8-153">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="036d8-154">Implementazione di un elemento ModelBinderProvider</span><span class="sxs-lookup"><span data-stu-id="036d8-154">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="036d8-155">Anziché applicare un attributo, è possibile implementare `IModelBinderProvider`.</span><span class="sxs-lookup"><span data-stu-id="036d8-155">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="036d8-156">Gli strumenti di associazione del framework incorporati vengono implementati in questo modo.</span><span class="sxs-lookup"><span data-stu-id="036d8-156">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="036d8-157">Quando si specifica il tipo sul quale opera lo strumento di associazione, si indica il tipo di argomento prodotto dallo strumento, **non** l'input accettato dallo strumento.</span><span class="sxs-lookup"><span data-stu-id="036d8-157">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="036d8-158">Il provider strumento di associazione seguente funziona con l'elemento `AuthorEntityBinder`.</span><span class="sxs-lookup"><span data-stu-id="036d8-158">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="036d8-159">Quando viene aggiunto alla raccolta di provider di MVC non è necessario usare l'attributo `ModelBinder` sui parametri tipizzati `Author` o `Author`.</span><span class="sxs-lookup"><span data-stu-id="036d8-159">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author` typed parameters.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="036d8-160">Nota: il codice precedente restituisce un elemento `BinderTypeModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="036d8-160">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="036d8-161">`BinderTypeModelBinder` funziona come factory per gli strumenti di associazione di modelli e implementa la funzionalità DI (Dependency Injection, Inserimento di dipendenze).</span><span class="sxs-lookup"><span data-stu-id="036d8-161">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="036d8-162">`AuthorEntityBinder` richiede DI (Dependency Injection, Inserimento di dipendenze) per l'accesso a EF Core.</span><span class="sxs-lookup"><span data-stu-id="036d8-162">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="036d8-163">Usare `BinderTypeModelBinder` se lo strumento di associazione di modelli richiede servizi da DI (Dependency Injection, Inserimento di dipendenze).</span><span class="sxs-lookup"><span data-stu-id="036d8-163">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="036d8-164">Per usare un provider dello strumento di associazione di modelli personalizzato, aggiungerlo in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="036d8-164">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

<span data-ttu-id="036d8-165">Durante la valutazione degli strumenti di associazione di modelli, la raccolta di provider viene esaminata in ordine.</span><span class="sxs-lookup"><span data-stu-id="036d8-165">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="036d8-166">Viene usato il primo provider che restituisce uno strumento di associazione.</span><span class="sxs-lookup"><span data-stu-id="036d8-166">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="036d8-167">La figura seguente visualizza gli strumenti di associazione di modelli predefiniti dal debugger.</span><span class="sxs-lookup"><span data-stu-id="036d8-167">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="036d8-168">![strumenti di associazione di modelli predefiniti](custom-model-binding/images/default-model-binders.png "strumenti di associazione di modelli predefiniti")</span><span class="sxs-lookup"><span data-stu-id="036d8-168">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="036d8-169">Se il provider personalizzato viene aggiunto alla fine della raccolta, è possibile che uno strumento di associazione di modelli incorporato venga chiamato prima dello strumento di associazione di modelli personalizzato.</span><span class="sxs-lookup"><span data-stu-id="036d8-169">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="036d8-170">In questo esempio il provider personalizzato viene aggiunto all'inizio della raccolta, per garantire che venga usato per gli argomenti dell'azione `Author`.</span><span class="sxs-lookup"><span data-stu-id="036d8-170">In this example, the custom provider is added to the beginning of the collection to ensure it's used for `Author` action arguments.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="036d8-171">Suggerimenti e procedure ottimali</span><span class="sxs-lookup"><span data-stu-id="036d8-171">Recommendations and best practices</span></span>

<span data-ttu-id="036d8-172">Gli strumenti di associazione di modelli personalizzati:</span><span class="sxs-lookup"><span data-stu-id="036d8-172">Custom model binders:</span></span>
- <span data-ttu-id="036d8-173">Non devono provare a impostare codici di stato o restituire risultati (ad esempio 404 Non trovato).</span><span class="sxs-lookup"><span data-stu-id="036d8-173">Shouldn't attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="036d8-174">Se si verifica un errore nell'associazione di modelli, l'errore deve essere gestito da un [filtro azioni](xref:mvc/controllers/filters) o da logica inclusa nel metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="036d8-174">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="036d8-175">Sono particolarmente utili per eliminare codice ripetitivo e problemi di montaggio incrociato dai metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="036d8-175">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="036d8-176">In genere non devono essere usati per convertire una stringa in un tipo personalizzato. Un elemento [`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter) rappresenta solitamente una scelta migliore.</span><span class="sxs-lookup"><span data-stu-id="036d8-176">Typically shouldn't be used to convert a string into a custom type, a [`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
