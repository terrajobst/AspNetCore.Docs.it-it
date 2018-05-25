---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Parametro di associazione in ASP.NET Web API | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5aa532137436922519c86246ebfa834910ac0d86
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="65b76-102">Parametro di associazione in ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="65b76-102">Parameter Binding in ASP.NET Web API</span></span>
====================
<span data-ttu-id="65b76-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="65b76-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="65b76-104">Quando l'API Web chiama un metodo in un controller, è necessario impostare i valori per i parametri, un processo denominato *associazione*.</span><span class="sxs-lookup"><span data-stu-id="65b76-104">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span> <span data-ttu-id="65b76-105">In questo articolo viene descritto come API Web associa i parametri e come è possibile personalizzare il processo di associazione.</span><span class="sxs-lookup"><span data-stu-id="65b76-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span>

<span data-ttu-id="65b76-106">Per impostazione predefinita, l'API Web utilizza le regole seguenti per associare parametri:</span><span class="sxs-lookup"><span data-stu-id="65b76-106">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="65b76-107">Se il parametro è un tipo "semplice", l'API Web tenta di ottenere il valore dell'URI.</span><span class="sxs-lookup"><span data-stu-id="65b76-107">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="65b76-108">I tipi semplici includono .NET [tipi primitivi](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **doppie**e così via), oltre a **TimeSpan**, **DateTime**, **Guid**, **decimale**, e **stringa**, *più* qualsiasi tipo con un convertitore di tipi che è possibile convertire una stringa.</span><span class="sxs-lookup"><span data-stu-id="65b76-108">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="65b76-109">(Ulteriori informazioni sui convertitori di tipi in un secondo momento.)</span><span class="sxs-lookup"><span data-stu-id="65b76-109">(More about type converters later.)</span></span>
- <span data-ttu-id="65b76-110">Per tipi complessi, API Web tenta di leggere il valore dal corpo del messaggio, utilizzando un [formattatore di media type](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="65b76-110">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="65b76-111">Ad esempio, ecco un metodo del controller Web API tipico:</span><span class="sxs-lookup"><span data-stu-id="65b76-111">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="65b76-112">Il *id* parametro è un &quot;semplice&quot; digitare, in modo che l'API Web tenta di ottenere il valore dall'URI della richiesta.</span><span class="sxs-lookup"><span data-stu-id="65b76-112">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="65b76-113">Il *elemento* parametro è un tipo complesso, in modo API Web utilizza un formattatore di media type per leggere il valore dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="65b76-113">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="65b76-114">Per ottenere un valore dall'URI, API Web Cerca nella stringa di query e di dati della route.</span><span class="sxs-lookup"><span data-stu-id="65b76-114">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="65b76-115">I dati della route viene popolati quando il sistema di routing analizza l'URI e a esso corrispondente a una route.</span><span class="sxs-lookup"><span data-stu-id="65b76-115">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="65b76-116">Per ulteriori informazioni, vedere [Routing e la selezione di azione](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="65b76-116">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="65b76-117">Nella parte restante di questo articolo, verrà illustrato come è possibile personalizzare il processo di associazione del modello.</span><span class="sxs-lookup"><span data-stu-id="65b76-117">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="65b76-118">Per tipi complessi, tuttavia, è consigliabile usare i formattatori di media type laddove possibile.</span><span class="sxs-lookup"><span data-stu-id="65b76-118">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="65b76-119">Un principio chiave HTTP è che le risorse vengano inviate nel corpo del messaggio, utilizzando la negoziazione del contenuto per specificare la rappresentazione della risorsa.</span><span class="sxs-lookup"><span data-stu-id="65b76-119">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="65b76-120">Formattatori di Media type sono stati progettati per questo scopo.</span><span class="sxs-lookup"><span data-stu-id="65b76-120">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="65b76-121">Utilizzo [FromUri]</span><span class="sxs-lookup"><span data-stu-id="65b76-121">Using [FromUri]</span></span>

<span data-ttu-id="65b76-122">Per forzare l'API Web per la lettura di un tipo complesso dall'URI, aggiungere il **[FromUri]** al parametro.</span><span class="sxs-lookup"><span data-stu-id="65b76-122">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="65b76-123">L'esempio seguente definisce un `GeoPoint` tipo, insieme a un metodo del controller che ottiene il `GeoPoint` dall'URI.</span><span class="sxs-lookup"><span data-stu-id="65b76-123">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="65b76-124">Il client è possibile inserire i valori di latitudine e longitudine nella stringa di query e l'API Web verrà utilizzata per creare un `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="65b76-124">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="65b76-125">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="65b76-125">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="65b76-126">Utilizzo [FromBody]</span><span class="sxs-lookup"><span data-stu-id="65b76-126">Using [FromBody]</span></span>

<span data-ttu-id="65b76-127">Per forzare l'API Web da cui leggere il corpo della richiesta di un tipo semplice, aggiungere il **[FromBody]** al parametro:</span><span class="sxs-lookup"><span data-stu-id="65b76-127">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="65b76-128">In questo esempio, API Web utilizzerà un formattatore di media type per leggere il valore di *nome* dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="65b76-128">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="65b76-129">Di seguito è riportato un esempio di richiesta client.</span><span class="sxs-lookup"><span data-stu-id="65b76-129">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="65b76-130">Quando un parametro è [FromBody], API Web utilizza l'intestazione Content-Type per selezionare un formattatore.</span><span class="sxs-lookup"><span data-stu-id="65b76-130">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="65b76-131">In questo esempio, il tipo di contenuto è &quot;application/json&quot; e il corpo della richiesta è una stringa JSON non elaborata (non un oggetto JSON).</span><span class="sxs-lookup"><span data-stu-id="65b76-131">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="65b76-132">È consentito al massimo un parametro di leggere il corpo del messaggio.</span><span class="sxs-lookup"><span data-stu-id="65b76-132">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="65b76-133">Pertanto, non funzionerà:</span><span class="sxs-lookup"><span data-stu-id="65b76-133">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="65b76-134">Il motivo per la regola è che il corpo della richiesta potrebbe essere archiviato in un flusso non memorizzato nel buffer che può essere letto una sola volta.</span><span class="sxs-lookup"><span data-stu-id="65b76-134">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="65b76-135">Convertitori di tipi</span><span class="sxs-lookup"><span data-stu-id="65b76-135">Type Converters</span></span>

<span data-ttu-id="65b76-136">È possibile rendere API Web considerare una classe come un tipo semplice (in modo che l'API Web tenterà di associarlo dall'URI) creando un **TypeConverter** e fornire una conversione di stringhe.</span><span class="sxs-lookup"><span data-stu-id="65b76-136">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="65b76-137">Il codice seguente illustra un `GeoPoint` classe che rappresenta un punto geografico, oltre a una **TypeConverter** che consente di convertire da stringhe a `GeoPoint` istanze.</span><span class="sxs-lookup"><span data-stu-id="65b76-137">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="65b76-138">Il `GeoPoint` classe è decorata con un **[TypeConverter]** attributo per specificare il convertitore di tipi.</span><span class="sxs-lookup"><span data-stu-id="65b76-138">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="65b76-139">(In questo esempio è stato ispirato da post di blog di stallo Mike [l'associazione a oggetti personalizzati nelle firme di azione in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span><span class="sxs-lookup"><span data-stu-id="65b76-139">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="65b76-140">Ora l'API Web considererà `GeoPoint` come un tipo semplice, ovvero tenterà di associare `GeoPoint` parametri dall'URI.</span><span class="sxs-lookup"><span data-stu-id="65b76-140">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="65b76-141">Non è necessario includere **[FromUri]** sul parametro.</span><span class="sxs-lookup"><span data-stu-id="65b76-141">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="65b76-142">Il client può richiamare il metodo con un URI simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="65b76-142">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="65b76-143">Raccoglitori di modelli</span><span class="sxs-lookup"><span data-stu-id="65b76-143">Model Binders</span></span>

<span data-ttu-id="65b76-144">Un'opzione più flessibile rispetto a un convertitore di tipi consiste nel creare un raccoglitore di modelli personalizzati.</span><span class="sxs-lookup"><span data-stu-id="65b76-144">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="65b76-145">Con un raccoglitore di modelli, si possono accedere a elementi quali la richiesta HTTP, la descrizione dell'azione e i valori non elaborati dai dati di route.</span><span class="sxs-lookup"><span data-stu-id="65b76-145">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="65b76-146">Per creare un raccoglitore di modelli, implementare il **IModelBinder** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="65b76-146">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="65b76-147">Questa interfaccia definisce un singolo metodo, **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="65b76-147">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="65b76-148">Ecco un raccoglitore di modelli per `GeoPoint` oggetti.</span><span class="sxs-lookup"><span data-stu-id="65b76-148">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="65b76-149">Un raccoglitore di modelli Ottiene i valori di input non elaborati da un *provider di valori*.</span><span class="sxs-lookup"><span data-stu-id="65b76-149">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="65b76-150">Questa progettazione separa due funzioni distinte:</span><span class="sxs-lookup"><span data-stu-id="65b76-150">This design separates two distinct functions:</span></span>

- <span data-ttu-id="65b76-151">Il provider di valori accetta la richiesta HTTP e popola un dizionario di coppie chiave-valore.</span><span class="sxs-lookup"><span data-stu-id="65b76-151">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="65b76-152">Lo strumento di associazione del modello viene utilizzato questo dizionario per popolare il modello.</span><span class="sxs-lookup"><span data-stu-id="65b76-152">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="65b76-153">Il provider di valori predefiniti nell'API Web ottiene i valori di dati della route e la stringa di query.</span><span class="sxs-lookup"><span data-stu-id="65b76-153">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="65b76-154">Ad esempio, se l'URI è `http://localhost/api/values/1?location=48,-122`, il provider di valori Crea le coppie chiave-valore seguente:</span><span class="sxs-lookup"><span data-stu-id="65b76-154">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="65b76-155">id = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="65b76-155">id = &quot;1&quot;</span></span>
- <span data-ttu-id="65b76-156">percorso = &quot;48,122&quot;</span><span class="sxs-lookup"><span data-stu-id="65b76-156">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="65b76-157">(Si suppone che il modello di route predefinito, ovvero &quot;api / {controller} / {id}&quot;.)</span><span class="sxs-lookup"><span data-stu-id="65b76-157">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="65b76-158">Il nome del parametro per l'associazione viene archiviato nel **ModelBindingContext.ModelName** proprietà.</span><span class="sxs-lookup"><span data-stu-id="65b76-158">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="65b76-159">Lo strumento di associazione del modello esegue la ricerca di una chiave con questo valore nel dizionario.</span><span class="sxs-lookup"><span data-stu-id="65b76-159">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="65b76-160">Se il valore è presente e può essere convertito in un `GeoPoint`, lo strumento di associazione del modello assegna il valore associato per il **ModelBindingContext.Model** proprietà.</span><span class="sxs-lookup"><span data-stu-id="65b76-160">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="65b76-161">Si noti che il gestore di associazione del modello non è limitato a una conversione del tipo semplice.</span><span class="sxs-lookup"><span data-stu-id="65b76-161">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="65b76-162">In questo esempio, il gestore di associazione del modello prima una ricerca in una tabella dei percorsi noti e se il problema persiste, utilizza la conversione di tipo.</span><span class="sxs-lookup"><span data-stu-id="65b76-162">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="65b76-163">**Impostare il gestore di associazione del modello**</span><span class="sxs-lookup"><span data-stu-id="65b76-163">**Setting the Model Binder**</span></span>

<span data-ttu-id="65b76-164">Esistono diversi modi per impostare un raccoglitore di modelli.</span><span class="sxs-lookup"><span data-stu-id="65b76-164">There are several ways to set a model binder.</span></span> <span data-ttu-id="65b76-165">In primo luogo, è possibile aggiungere un **[ModelBinder]** al parametro.</span><span class="sxs-lookup"><span data-stu-id="65b76-165">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="65b76-166">È inoltre possibile aggiungere un **[ModelBinder]** per il tipo di attributo.</span><span class="sxs-lookup"><span data-stu-id="65b76-166">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="65b76-167">API Web utilizzerà lo strumento di associazione del modello specificato per tutti i parametri di quel tipo.</span><span class="sxs-lookup"><span data-stu-id="65b76-167">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="65b76-168">Infine, è possibile aggiungere un provider dello strumento di associazione del modello per il **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="65b76-168">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="65b76-169">Un provider dello strumento di associazione del modello è semplicemente una classe factory che crea un raccoglitore di modelli.</span><span class="sxs-lookup"><span data-stu-id="65b76-169">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="65b76-170">È possibile creare un provider mediante derivazione da di [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) classe.</span><span class="sxs-lookup"><span data-stu-id="65b76-170">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="65b76-171">Tuttavia, se il gestore di associazione del modello gestisce un solo tipo, è più facile da utilizzare l'oggetto incorporato **SimpleModelBinderProvider**, che è progettato per questo scopo.</span><span class="sxs-lookup"><span data-stu-id="65b76-171">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="65b76-172">A tale scopo, osservare il codice indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="65b76-172">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="65b76-173">Con un provider di associazione di modelli, è comunque necessario aggiungere il **[ModelBinder]** al parametro, per indicare API Web che deve utilizzare un raccoglitore di modelli e non un formattatore di media type.</span><span class="sxs-lookup"><span data-stu-id="65b76-173">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="65b76-174">Tuttavia, ora non è necessario specificare il tipo di strumento di associazione nell'attributo:</span><span class="sxs-lookup"><span data-stu-id="65b76-174">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="65b76-175">Provider di valori</span><span class="sxs-lookup"><span data-stu-id="65b76-175">Value Providers</span></span>

<span data-ttu-id="65b76-176">Ho detto che un raccoglitore di modelli Ottiene i valori da un provider di valori.</span><span class="sxs-lookup"><span data-stu-id="65b76-176">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="65b76-177">Per scrivere un provider di valori personalizzati, implementare il **IValueProvider** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="65b76-177">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="65b76-178">Di seguito è riportato un esempio che inserisce i valori dai cookie nella richiesta:</span><span class="sxs-lookup"><span data-stu-id="65b76-178">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="65b76-179">È anche necessario creare una factory di provider di valori mediante la derivazione da di **ValueProviderFactory** classe.</span><span class="sxs-lookup"><span data-stu-id="65b76-179">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="65b76-180">Aggiungere la factory del provider di valore per il **HttpConfiguration** come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="65b76-180">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="65b76-181">API Web consente di comporre tutte i provider di valori, pertanto, quando viene chiamato un raccoglitore di modelli **ValueProvider.GetValue**, lo strumento di associazione del modello riceve il valore del primo provider di valori che è in grado di produrre il.</span><span class="sxs-lookup"><span data-stu-id="65b76-181">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="65b76-182">In alternativa, è possibile impostare la factory del provider di valore a livello di parametro utilizzando il **ValueProvider** attributo, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="65b76-182">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="65b76-183">In questo modo l'API Web per utilizzare l'associazione di modelli con la factory del provider di valore specificato e non per usare uno degli altri provider di valore registrato.</span><span class="sxs-lookup"><span data-stu-id="65b76-183">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="65b76-184">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="65b76-184">HttpParameterBinding</span></span>

<span data-ttu-id="65b76-185">Raccoglitori di modelli sono un'istanza specifica di un meccanismo più generale.</span><span class="sxs-lookup"><span data-stu-id="65b76-185">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="65b76-186">Se si esamina il **[ModelBinder]** attributo, si noterà che deriva dalla classe astratta **ParameterBindingAttribute** classe.</span><span class="sxs-lookup"><span data-stu-id="65b76-186">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="65b76-187">Questa classe definisce un singolo metodo, **GetBinding**, che restituisce un **HttpParameterBinding** oggetto:</span><span class="sxs-lookup"><span data-stu-id="65b76-187">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="65b76-188">Un **HttpParameterBinding** è responsabile per l'associazione di un parametro a un valore.</span><span class="sxs-lookup"><span data-stu-id="65b76-188">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="65b76-189">In caso di **[ModelBinder]**, l'attributo restituisce un **HttpParameterBinding** implementazione che utilizza un **IModelBinder** per eseguire l'associazione effettiva.</span><span class="sxs-lookup"><span data-stu-id="65b76-189">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="65b76-190">È anche possibile implementare una propria **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="65b76-190">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="65b76-191">Ad esempio, si supponga che si desidera ottenere eTag da `if-match` e `if-none-match` intestazioni della richiesta.</span><span class="sxs-lookup"><span data-stu-id="65b76-191">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="65b76-192">Si inizierà definendo una classe per rappresentare valori eTag.</span><span class="sxs-lookup"><span data-stu-id="65b76-192">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="65b76-193">È inoltre verranno definite un'enumerazione che indica se il valore ETag da ottenere il `if-match` intestazione o il `if-none-match` intestazione.</span><span class="sxs-lookup"><span data-stu-id="65b76-193">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="65b76-194">Ecco un **HttpParameterBinding** che ottiene il valore ETag dall'intestazione desiderato e lo associa a un parametro di tipo ETag:</span><span class="sxs-lookup"><span data-stu-id="65b76-194">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="65b76-195">Il **ExecuteBindingAsync** metodo esegue l'associazione.</span><span class="sxs-lookup"><span data-stu-id="65b76-195">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="65b76-196">All'interno di questo metodo, aggiungere il valore del parametro associato per il **oggetto ActionArgument** dizionario il **HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="65b76-196">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="65b76-197">Se il **ExecuteBindingAsync** metodo legge il corpo del messaggio di richiesta, eseguire l'override di **WillReadBody** proprietà da restituire true.</span><span class="sxs-lookup"><span data-stu-id="65b76-197">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="65b76-198">Il corpo della richiesta potrebbe essere un flusso non memorizzato nel buffer che può essere letto solo una volta, in modo API Web consente di applicare una regola che al massimo un binding può leggere il corpo del messaggio.</span><span class="sxs-lookup"><span data-stu-id="65b76-198">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>


<span data-ttu-id="65b76-199">Per applicare un oggetto personalizzato **HttpParameterBinding**, è possibile definire un attributo che deriva da **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="65b76-199">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="65b76-200">Per `ETagParameterBinding`, definiamo due attributi, uno per `if-match` intestazioni e uno per `if-none-match` intestazioni.</span><span class="sxs-lookup"><span data-stu-id="65b76-200">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="65b76-201">Derivano dalla classe base astratta.</span><span class="sxs-lookup"><span data-stu-id="65b76-201">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="65b76-202">Ecco un metodo del controller che utilizza il `[IfNoneMatch]` attributo.</span><span class="sxs-lookup"><span data-stu-id="65b76-202">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="65b76-203">Oltre a **ParameterBindingAttribute**, vi è un altro hook per l'aggiunta di un oggetto personalizzato **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="65b76-203">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="65b76-204">Nel **HttpConfiguration** oggetto, il **ParameterBindingRules** proprietà è una raccolta di funzioni anomymous di tipo (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="65b76-204">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anomymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="65b76-205">Ad esempio, è possibile aggiungere una regola che utilizza qualsiasi parametro ETag su un metodo GET `ETagParameterBinding` con `if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="65b76-205">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="65b76-206">La funzione deve restituire `null` per i parametri in cui l'associazione non è applicabile.</span><span class="sxs-lookup"><span data-stu-id="65b76-206">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="65b76-207">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="65b76-207">IActionValueBinder</span></span>

<span data-ttu-id="65b76-208">L'intero processo di associazione di parametri è controllato da un servizio di collegamento, **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="65b76-208">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="65b76-209">L'implementazione predefinita di **IActionValueBinder** esegue le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="65b76-209">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="65b76-210">Cercare un **ParameterBindingAttribute** sul parametro.</span><span class="sxs-lookup"><span data-stu-id="65b76-210">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="65b76-211">Ciò include **[FromBody]**, **[FromUri]**, e **[ModelBinder]**, o gli attributi personalizzati.</span><span class="sxs-lookup"><span data-stu-id="65b76-211">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="65b76-212">In caso contrario, Cerca in **HttpConfiguration.ParameterBindingRules** per una funzione che restituisce un valore non null **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="65b76-212">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="65b76-213">In caso contrario, utilizzare le regole predefinite descritte in precedenza.</span><span class="sxs-lookup"><span data-stu-id="65b76-213">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="65b76-214">Se il tipo di parametro è "semplice" o ha un convertitore di tipi, associare dall'URI.</span><span class="sxs-lookup"><span data-stu-id="65b76-214">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="65b76-215">Ciò equivale a inserire il **[FromUri]** attributo per il parametro.</span><span class="sxs-lookup"><span data-stu-id="65b76-215">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="65b76-216">In caso contrario, provare a leggere il parametro dal corpo del messaggio.</span><span class="sxs-lookup"><span data-stu-id="65b76-216">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="65b76-217">Ciò equivale a inserire **[FromBody]** sul parametro.</span><span class="sxs-lookup"><span data-stu-id="65b76-217">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="65b76-218">Se si desidera, è possibile sostituire l'intero **IActionValueBinder** servizio con un'implementazione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="65b76-218">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="65b76-219">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="65b76-219">Additional Resources</span></span>

[<span data-ttu-id="65b76-220">Esempio di associazione di parametri personalizzati</span><span class="sxs-lookup"><span data-stu-id="65b76-220">Custom Parameter Binding Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

<span data-ttu-id="65b76-221">Mike stallo scritto una buona serie di post di blog sull'associazione di parametro API Web:</span><span class="sxs-lookup"><span data-stu-id="65b76-221">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="65b76-222">Funzionamento delle API Web sull'associazione di parametri</span><span class="sxs-lookup"><span data-stu-id="65b76-222">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="65b76-223">Associazione di parametro di tipo MVC per l'API Web</span><span class="sxs-lookup"><span data-stu-id="65b76-223">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="65b76-224">L'associazione a oggetti personalizzati nelle firme di azione MVC o Web API</span><span class="sxs-lookup"><span data-stu-id="65b76-224">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="65b76-225">Come creare un provider di valori personalizzati in Web API</span><span class="sxs-lookup"><span data-stu-id="65b76-225">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="65b76-226">Associazione di parametri di API Web dietro le quinte</span><span class="sxs-lookup"><span data-stu-id="65b76-226">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
