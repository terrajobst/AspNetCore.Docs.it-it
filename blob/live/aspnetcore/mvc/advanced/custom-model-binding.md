---
title: Associazione di modelli personalizzati
author: ardalis
description: Personalizzazione di associazione del modello in ASP.NET MVC di base.
ms.author: riande
manager: wpickett
ms.date: 04/10/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: d8b94f53954c5ab63ccf3aab4eb7a7a7dbea487b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="custom-model-binding"></a><span data-ttu-id="44a9a-103">Associazione di modelli personalizzati</span><span class="sxs-lookup"><span data-stu-id="44a9a-103">Custom Model Binding</span></span>

<span data-ttu-id="44a9a-104">Da [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="44a9a-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="44a9a-105">Associazione di modelli consente di azioni del controller lavorare direttamente con i tipi di modello (passati come argomenti dei metodi), invece di richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="44a9a-105">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="44a9a-106">Mapping tra i modelli di data e l'applicazione richiesta in ingresso sono gestite da gestori di associazione del modello.</span><span class="sxs-lookup"><span data-stu-id="44a9a-106">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="44a9a-107">Gli sviluppatori possono estendere la funzionalità di associazione di modelli predefiniti implementando raccoglitori di modelli personalizzati (ma in genere, non è necessario scrivere un provider personalizzato).</span><span class="sxs-lookup"><span data-stu-id="44a9a-107">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="44a9a-108">Consente di visualizzare o scaricare l'esempio da GitHub</span><span class="sxs-lookup"><span data-stu-id="44a9a-108">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="44a9a-109">Limitazioni di gestore di associazione del modello predefinito</span><span class="sxs-lookup"><span data-stu-id="44a9a-109">Default model binder limitations</span></span>

<span data-ttu-id="44a9a-110">I raccoglitori di modelli predefiniti supportano la maggior parte dei comuni tipi di dati .NET Core e devono soddisfare le esigenze di maggior parte degli sviluppatori.</span><span class="sxs-lookup"><span data-stu-id="44a9a-110">The default model binders support most of the common .NET Core data types and should meet most developers\` needs.</span></span> <span data-ttu-id="44a9a-111">È prevista l'associazione basata su testo di input dalla richiesta direttamente a tipi di modello.</span><span class="sxs-lookup"><span data-stu-id="44a9a-111">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="44a9a-112">Potrebbe essere necessario trasformare l'input prima dell'associazione.</span><span class="sxs-lookup"><span data-stu-id="44a9a-112">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="44a9a-113">Ad esempio, quando si dispone di una chiave che può essere usata per cercare i dati del modello.</span><span class="sxs-lookup"><span data-stu-id="44a9a-113">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="44a9a-114">È possibile utilizzare un raccoglitore di modelli personalizzati per recuperare i dati in base alla chiave.</span><span class="sxs-lookup"><span data-stu-id="44a9a-114">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="44a9a-115">Esame del modello di associazione</span><span class="sxs-lookup"><span data-stu-id="44a9a-115">Model binding review</span></span>

<span data-ttu-id="44a9a-116">Associazione di modelli utilizza le definizioni specifiche per i tipi su che opera.</span><span class="sxs-lookup"><span data-stu-id="44a9a-116">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="44a9a-117">Oggetto *tipo semplice* viene convertito da una singola stringa di input.</span><span class="sxs-lookup"><span data-stu-id="44a9a-117">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="44a9a-118">Oggetto *tipo complesso* viene convertito da più valori di input.</span><span class="sxs-lookup"><span data-stu-id="44a9a-118">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="44a9a-119">Il framework determina la differenza in base alla presenza di un `TypeConverter`.</span><span class="sxs-lookup"><span data-stu-id="44a9a-119">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="44a9a-120">È consigliabile creare un convertitore di tipi se si dispone di una semplice `string`  ->  `SomeType` mapping che non richiedono risorse esterne.</span><span class="sxs-lookup"><span data-stu-id="44a9a-120">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="44a9a-121">Prima di creare il propria Raccoglitore di modelli personalizzati, è opportuno esaminare i risultati del modello esistente come vengono implementati i gestori di associazione.</span><span class="sxs-lookup"><span data-stu-id="44a9a-121">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="44a9a-122">Si consideri il [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) che consente di convertire le stringhe con codifica base64 in matrici di byte.</span><span class="sxs-lookup"><span data-stu-id="44a9a-122">Consider the [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="44a9a-123">Le matrici di byte vengono spesso archiviate come file o i campi BLOB del database.</span><span class="sxs-lookup"><span data-stu-id="44a9a-123">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="44a9a-124">Utilizzo di ByteArrayModelBinder</span><span class="sxs-lookup"><span data-stu-id="44a9a-124">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="44a9a-125">Per rappresentare i dati binari, è possono utilizzare le stringhe con codifica Base64.</span><span class="sxs-lookup"><span data-stu-id="44a9a-125">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="44a9a-126">Nell'immagine seguente, ad esempio, può essere codificato come stringa.</span><span class="sxs-lookup"><span data-stu-id="44a9a-126">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="44a9a-127">![dotnet bot](custom-model-binding/images/bot.png "bot dotnet")</span><span class="sxs-lookup"><span data-stu-id="44a9a-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="44a9a-128">Una piccola parte della stringa con codifica è illustrata nella figura seguente:</span><span class="sxs-lookup"><span data-stu-id="44a9a-128">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="44a9a-129">![bot dotnet codificato](custom-model-binding/images/encoded-bot.png "bot dotnet con codificato")</span><span class="sxs-lookup"><span data-stu-id="44a9a-129">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="44a9a-130">Seguire le istruzioni di [Leggimi dell'esempio](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) per convertire la stringa con codifica base64 in un file.</span><span class="sxs-lookup"><span data-stu-id="44a9a-130">Follow the instructions in the [sample's README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="44a9a-131">ASP.NET MVC di base può accettare una stringa con codifica base64 e utilizzare un `ByteArrayModelBinder` per convertirlo in una matrice di byte.</span><span class="sxs-lookup"><span data-stu-id="44a9a-131">ASP.NET Core MVC can take a base64-encoded strings and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="44a9a-132">Il [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) che implementa l'interfaccia [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) mappe `byte[]` argomenti `ByteArrayModelBinder`:</span><span class="sxs-lookup"><span data-stu-id="44a9a-132">The [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

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

<span data-ttu-id="44a9a-133">Quando si crea il propria Raccoglitore di modelli personalizzati, è possibile implementare la propria `IModelBinderProvider` digitare oppure utilizzare il [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span><span class="sxs-lookup"><span data-stu-id="44a9a-133">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="44a9a-134">Nell'esempio seguente viene illustrato come utilizzare `ByteArrayModelBinder` per convertire una stringa con codifica base64 in un `byte[]` e salvare il risultato in un file:</span><span class="sxs-lookup"><span data-stu-id="44a9a-134">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

<span data-ttu-id="44a9a-135">È possibile inviare una stringa con codifica base64 a questo metodo api usando uno strumento come [Postman](https://www.getpostman.com/):</span><span class="sxs-lookup"><span data-stu-id="44a9a-135">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="44a9a-136">![postman](custom-model-binding/images/postman.png "postman")</span><span class="sxs-lookup"><span data-stu-id="44a9a-136">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="44a9a-137">Fino a quando il gestore di associazione è possibile associare dati di richiesta alle proprietà denominate in modo appropriato o argomenti, l'associazione di modelli avrà esito positivo.</span><span class="sxs-lookup"><span data-stu-id="44a9a-137">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="44a9a-138">Nell'esempio seguente viene illustrato come utilizzare `ByteArrayModelBinder` con un modello di visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="44a9a-138">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="44a9a-139">Esempio di Raccoglitore di modelli personalizzati</span><span class="sxs-lookup"><span data-stu-id="44a9a-139">Custom model binder sample</span></span>

<span data-ttu-id="44a9a-140">In questa sezione verrà implementato un raccoglitore di modelli personalizzati che:</span><span class="sxs-lookup"><span data-stu-id="44a9a-140">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="44a9a-141">Converte i dati della richiesta in ingresso in argomenti chiavi fortemente tipizzati.</span><span class="sxs-lookup"><span data-stu-id="44a9a-141">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="44a9a-142">Usa Entity Framework Core per recuperare l'entità associata.</span><span class="sxs-lookup"><span data-stu-id="44a9a-142">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="44a9a-143">Passa l'entità associata come argomento al metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="44a9a-143">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="44a9a-144">L'esempio seguente usa il `ModelBinder` attributo di `Author` modello:</span><span class="sxs-lookup"><span data-stu-id="44a9a-144">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

<span data-ttu-id="44a9a-145">Nel codice precedente, il `ModelBinder` attributo specifica il tipo di `IModelBinder` che deve essere utilizzata per associare `Author` parametri dell'azione.</span><span class="sxs-lookup"><span data-stu-id="44a9a-145">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span> 

<span data-ttu-id="44a9a-146">Il `AuthorEntityBinder` viene utilizzata per associare un `Author` parametro mediante il recupero dell'entità da un'origine dati tramite Entity Framework Core e un `authorId`:</span><span class="sxs-lookup"><span data-stu-id="44a9a-146">The `AuthorEntityBinder` is used to bind an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

<span data-ttu-id="44a9a-147">Il codice seguente viene illustrato come utilizzare il `AuthorEntityBinder` in un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="44a9a-147">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="44a9a-148">Il `ModelBinder` attributo può essere utilizzato per applicare il `AuthorEntityBinder` ai parametri che non utilizzano convenzioni predefinite:</span><span class="sxs-lookup"><span data-stu-id="44a9a-148">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that do not use default conventions:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="44a9a-149">In questo esempio, poiché il nome dell'argomento non è il valore predefinito `authorId`, viene specificato nel parametro usando `ModelBinder` attributo.</span><span class="sxs-lookup"><span data-stu-id="44a9a-149">In this example, since the name of the argument is not the default `authorId`, it's specified on the parameter using `ModelBinder` attribute.</span></span> <span data-ttu-id="44a9a-150">Si noti che il controller e l'azione metodo sono semplificati rispetto alla ricerca di entità nel metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="44a9a-150">Note that both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="44a9a-151">La logica per recuperare l'autore usando Entity Framework Core viene spostata lo strumento di associazione del modello.</span><span class="sxs-lookup"><span data-stu-id="44a9a-151">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="44a9a-152">Può trattarsi di una notevole semplificazione, quando si dispongono di diversi metodi che associare al modello di autore e può essere utile per seguire il [principio secca](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="44a9a-152">This can be considerable simplification when you have several methods that bind to the author model, and can help you to follow the [DRY principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="44a9a-153">È possibile applicare il `ModelBinder` attributo alle proprietà del modello singolo (ad esempio in un elemento viewmodel) o ai parametri di metodo di azione per specificare un determinato raccoglitore di modelli o nome del modello per solo quel tipo o l'azione.</span><span class="sxs-lookup"><span data-stu-id="44a9a-153">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="44a9a-154">Implementazione di un ModelBinderProvider</span><span class="sxs-lookup"><span data-stu-id="44a9a-154">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="44a9a-155">Anziché applicare un attributo, è possibile implementare `IModelBinderProvider`.</span><span class="sxs-lookup"><span data-stu-id="44a9a-155">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="44a9a-156">Questo è l'implementazione dei gestori di associazione del framework incorporato.</span><span class="sxs-lookup"><span data-stu-id="44a9a-156">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="44a9a-157">Quando si specifica il tipo del gestore di associazione opera su, specificare il tipo di argomento, viene generato, **non** accetta il gestore di associazione di input.</span><span class="sxs-lookup"><span data-stu-id="44a9a-157">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="44a9a-158">Il provider del gestore di associazione seguente funziona con il `AuthorEntityBinder`.</span><span class="sxs-lookup"><span data-stu-id="44a9a-158">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="44a9a-159">Quando viene aggiunto alla raccolta di MVC di provider, non è necessario utilizzare il `ModelBinder` attributo `Author` o `Author` parametri tipizzati.</span><span class="sxs-lookup"><span data-stu-id="44a9a-159">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author` typed parameters.</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="44a9a-160">Nota: Il codice precedente restituisce un `BinderTypeModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="44a9a-160">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="44a9a-161">`BinderTypeModelBinder`funge da una factory per raccoglitori di modelli e fornisce l'inserimento di dipendenze (DI).</span><span class="sxs-lookup"><span data-stu-id="44a9a-161">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="44a9a-162">Il `AuthorEntityBinder` richiede DI accedere EF Core.</span><span class="sxs-lookup"><span data-stu-id="44a9a-162">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="44a9a-163">Utilizzare `BinderTypeModelBinder` se il gestore di associazione del modello richiede servizi DI.</span><span class="sxs-lookup"><span data-stu-id="44a9a-163">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="44a9a-164">Per utilizzare un provider di strumento di associazione del modello personalizzato, aggiungerlo in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="44a9a-164">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

<span data-ttu-id="44a9a-165">Quando si valuta raccoglitori di modelli, la raccolta di provider viene esaminata in ordine.</span><span class="sxs-lookup"><span data-stu-id="44a9a-165">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="44a9a-166">Viene utilizzato il primo provider che restituisce uno strumento di associazione.</span><span class="sxs-lookup"><span data-stu-id="44a9a-166">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="44a9a-167">La figura seguente mostra il valore predefinito raccoglitori dal debugger.</span><span class="sxs-lookup"><span data-stu-id="44a9a-167">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="44a9a-168">![raccoglitori di modelli predefinito](custom-model-binding/images/default-model-binders.png "predefinito raccoglitori di modelli")</span><span class="sxs-lookup"><span data-stu-id="44a9a-168">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="44a9a-169">Aggiunta alla fine della raccolta di provider può comportare un raccoglitore di modelli predefiniti viene chiamato prima che il gestore di associazione personalizzato con una possibilità.</span><span class="sxs-lookup"><span data-stu-id="44a9a-169">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="44a9a-170">In questo esempio, il provider personalizzato viene aggiunto all'inizio della raccolta per assicurare venga utilizzato per `Author` argomenti dell'azione.</span><span class="sxs-lookup"><span data-stu-id="44a9a-170">In this example, the custom provider is added to the beginning of the collection to ensure it is used for `Author` action arguments.</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="44a9a-171">Indicazioni e procedure consigliate</span><span class="sxs-lookup"><span data-stu-id="44a9a-171">Recommendations and best practices</span></span>

<span data-ttu-id="44a9a-172">Raccoglitori di modelli personalizzati:</span><span class="sxs-lookup"><span data-stu-id="44a9a-172">Custom model binders:</span></span>
- <span data-ttu-id="44a9a-173">Non tentare di impostare i codici di stato o restituire risultati (ad esempio 404 non trovato).</span><span class="sxs-lookup"><span data-stu-id="44a9a-173">Should not attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="44a9a-174">Se si verifica un errore di associazione del modello, un [filtro azione](xref:mvc/controllers/filters) o logica all'interno del metodo di azione stesso deve gestire l'errore.</span><span class="sxs-lookup"><span data-stu-id="44a9a-174">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="44a9a-175">Sono particolarmente utili per eliminare codice ripetitivo e problemi di montaggio incrociato da metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="44a9a-175">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="44a9a-176">In genere non devono essere utilizzate per convertire una stringa in un tipo personalizzato, un [ `TypeConverter` ](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) in genere è un'opzione migliore.</span><span class="sxs-lookup"><span data-stu-id="44a9a-176">Typically should not be used to convert a string into a custom type, a [`TypeConverter`](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
