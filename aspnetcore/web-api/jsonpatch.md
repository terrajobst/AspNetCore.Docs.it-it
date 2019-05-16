---
title: JSON Patch nell'API Web ASP.NET Core
author: tdykstra
description: Informazioni su come gestire le richieste JSON Patch in un'API Web ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/24/2019
uid: web-api/jsonpatch
ms.openlocfilehash: 14710e6431a2a7ce60fa7f190bef184da85281a0
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64888416"
---
# <a name="jsonpatch-in-aspnet-core-web-api"></a><span data-ttu-id="e6b9e-103">JSON Patch nell'API Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6b9e-103">JsonPatch in ASP.NET Core web API</span></span>

<span data-ttu-id="e6b9e-104">[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="e6b9e-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="e6b9e-105">Questo articolo descrive come gestire le richieste JSON Patch in un'API Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-105">This article explains how to handle JSON Patch requests in an ASP.NET Core web API.</span></span>

## <a name="patch-http-request-method"></a><span data-ttu-id="e6b9e-106">Metodo di richiesta HTTP PATCH</span><span class="sxs-lookup"><span data-stu-id="e6b9e-106">PATCH HTTP request method</span></span>

<span data-ttu-id="e6b9e-107">I metodi PUT e [PATCH](https://tools.ietf.org/html/rfc5789) vengono usati per aggiornare una risorsa esistente.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-107">The PUT and [PATCH](https://tools.ietf.org/html/rfc5789) methods are used to update an existing resource.</span></span> <span data-ttu-id="e6b9e-108">La differenza tra i due metodi è che PUT sostituisce l'intera risorsa, mentre PATCH specifica solo le modifiche.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-108">The difference between them is that PUT replaces the entire resource, while PATCH specifies only the changes.</span></span>

## <a name="json-patch"></a><span data-ttu-id="e6b9e-109">JSON Patch</span><span class="sxs-lookup"><span data-stu-id="e6b9e-109">JSON Patch</span></span>

<span data-ttu-id="e6b9e-110">[JSON Patch](https://tools.ietf.org/html/rfc6902) è un formato per specificare gli aggiornamenti da applicare a una risorsa.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-110">[JSON Patch](https://tools.ietf.org/html/rfc6902) is a format for specifying updates to be applied to a resource.</span></span> <span data-ttu-id="e6b9e-111">Un documento JSON Patch è una matrice di *operazioni*.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-111">A JSON Patch document has an array of *operations*.</span></span> <span data-ttu-id="e6b9e-112">Ogni operazione identifica un determinato tipo di modifica, ad esempio l'aggiunta di un elemento della matrice o la sostituzione di un valore di proprietà.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-112">Each operation identifies a particular type of change, such as add an array element or replace a property value.</span></span>

<span data-ttu-id="e6b9e-113">Ad esempio, i documenti JSON seguenti rappresentano una risorsa, un documento di patch JSON per la risorsa e il risultato dell'applicazione delle operazioni patch.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-113">For example, the following JSON documents represent a resource, a JSON patch document for the resource, and the result of applying the patch operations.</span></span>

### <a name="resource-example"></a><span data-ttu-id="e6b9e-114">Esempio di risorsa</span><span class="sxs-lookup"><span data-stu-id="e6b9e-114">Resource example</span></span>

[!code-csharp[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a><span data-ttu-id="e6b9e-115">Esempio di patch JSON</span><span class="sxs-lookup"><span data-stu-id="e6b9e-115">JSON patch example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

<span data-ttu-id="e6b9e-116">Nel codice JSON precedente:</span><span class="sxs-lookup"><span data-stu-id="e6b9e-116">In the preceding JSON:</span></span>

* <span data-ttu-id="e6b9e-117">La proprietà `op` indica il tipo di operazione.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-117">The `op` property indicates the type of operation.</span></span>
* <span data-ttu-id="e6b9e-118">La proprietà `path` indica l'elemento da aggiornare.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-118">The `path` property indicates the element to update.</span></span>
* <span data-ttu-id="e6b9e-119">La proprietà `value` fornisce il nuovo valore.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-119">The `value` property provides the new value.</span></span>

### <a name="resource-after-patch"></a><span data-ttu-id="e6b9e-120">Risorsa dopo la patch</span><span class="sxs-lookup"><span data-stu-id="e6b9e-120">Resource after patch</span></span>

<span data-ttu-id="e6b9e-121">Ecco la risorsa dopo aver applicato il documento JSON Patch precedente:</span><span class="sxs-lookup"><span data-stu-id="e6b9e-121">Here's the resource after applying the preceding JSON Patch document:</span></span>

```json
{
  "customerName": "Barry",
  "orders": [
    {
      "orderName": "Order0",
      "orderType": null
    },
    {
      "orderName": "Order1",
      "orderType": null
    },
    {
      "orderName": "Order2",
      "orderType": null
    }
  ]
}
```

<span data-ttu-id="e6b9e-122">Le modifiche apportate tramite l'applicazione di un documento JSON Patch a una risorsa sono atomiche: se qualsiasi operazione nell'elenco non riesce, non viene applicata alcuna operazione contenuta nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-122">The changes made by applying a JSON Patch document to a resource are atomic: if any operation in the list fails, no operation in the list is applied.</span></span>

## <a name="path-syntax"></a><span data-ttu-id="e6b9e-123">Sintassi di path</span><span class="sxs-lookup"><span data-stu-id="e6b9e-123">Path syntax</span></span>

<span data-ttu-id="e6b9e-124">La proprietà [path](http://tools.ietf.org/html/rfc6901) di un oggetto operazione include barre tra livelli.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-124">The [path](http://tools.ietf.org/html/rfc6901) property of an operation object has slashes between levels.</span></span> <span data-ttu-id="e6b9e-125">Ad esempio `"/address/zipCode"`.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-125">For example, `"/address/zipCode"`.</span></span>

<span data-ttu-id="e6b9e-126">Vengono usati indici in base zero per specificare gli elementi della matrice.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-126">Zero-based indexes are used to specify array elements.</span></span> <span data-ttu-id="e6b9e-127">Il primo elemento della matrice `addresses` è in corrispondenza di `/addresses/0`.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-127">The first element of the `addresses` array would be at `/addresses/0`.</span></span> <span data-ttu-id="e6b9e-128">Per aggiungere (`add`) un elemento alla fine della matrice, usare un trattino (-) invece di un numero di indice: `/addresses/-`.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-128">To `add` to the end of an array, use a hyphen (-) rather than an index number: `/addresses/-`.</span></span>

### <a name="operations"></a><span data-ttu-id="e6b9e-129">Operazioni</span><span class="sxs-lookup"><span data-stu-id="e6b9e-129">Operations</span></span>

<span data-ttu-id="e6b9e-130">La tabella seguente mostra le operazioni supportate in base a quanto definito nella [specifica JSON Patch](https://tools.ietf.org/html/rfc6902):</span><span class="sxs-lookup"><span data-stu-id="e6b9e-130">The following table shows supported operations as defined in the [JSON Patch specification](https://tools.ietf.org/html/rfc6902):</span></span>

|<span data-ttu-id="e6b9e-131">Operazione</span><span class="sxs-lookup"><span data-stu-id="e6b9e-131">Operation</span></span>  | <span data-ttu-id="e6b9e-132">Note</span><span class="sxs-lookup"><span data-stu-id="e6b9e-132">Notes</span></span> |
|-----------|--------------------------------|
| `add`     | <span data-ttu-id="e6b9e-133">Aggiunge una proprietà o un elemento della matrice.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-133">Add a property or array element.</span></span> <span data-ttu-id="e6b9e-134">Per la proprietà esistente: impostare il valore.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-134">For existing property: set value.</span></span>|
| `remove`  | <span data-ttu-id="e6b9e-135">Rimuove una proprietà o un elemento della matrice.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-135">Remove a property or array element.</span></span> |
| `replace` | <span data-ttu-id="e6b9e-136">Uguale a `remove` seguita da `add` nella stessa posizione.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-136">Same as `remove` followed by `add` at same location.</span></span> |
| `move`    | <span data-ttu-id="e6b9e-137">Uguale a `remove` dall'origine seguita da `add` alla destinazione usando il valore dell'origine.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-137">Same as `remove` from source followed by `add` to destination using value from source.</span></span> |
| `copy`    | <span data-ttu-id="e6b9e-138">Uguale a `add` alla destinazione usando il valore dell'origine.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-138">Same as `add` to destination using value from source.</span></span> |
| `test`    | <span data-ttu-id="e6b9e-139">Restituisce il codice di stato di richiesta riuscita se il valore in `path` = `value` fornito.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-139">Return success status code if value at `path` = provided `value`.</span></span>|

## <a name="jsonpatch-in-aspnet-core"></a><span data-ttu-id="e6b9e-140">JSON Patch in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6b9e-140">JsonPatch in ASP.NET Core</span></span>

<span data-ttu-id="e6b9e-141">L'implementazione ASP.NET Core di JSON Patch viene fornita nel pacchetto NuGet [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/).</span><span class="sxs-lookup"><span data-stu-id="e6b9e-141">The ASP.NET Core implementation of JSON Patch is provided in the [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet package.</span></span> <span data-ttu-id="e6b9e-142">Il pacchetto è incluso nel metapacchetto [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="e6b9e-142">The package is included in the [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app) metapackage.</span></span>

## <a name="action-method-code"></a><span data-ttu-id="e6b9e-143">Codice del metodo di azione</span><span class="sxs-lookup"><span data-stu-id="e6b9e-143">Action method code</span></span>

<span data-ttu-id="e6b9e-144">In un controller API un metodo di azione per JSON Patch:</span><span class="sxs-lookup"><span data-stu-id="e6b9e-144">In an API controller, an action method for JSON Patch:</span></span>

* <span data-ttu-id="e6b9e-145">Viene annotato con l'attributo `HttpPatch`.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-145">Is annotated with the `HttpPatch` attribute.</span></span>
* <span data-ttu-id="e6b9e-146">Accetta un oggetto `JsonPatchDocument<T>`, in genere con [FromBody].</span><span class="sxs-lookup"><span data-stu-id="e6b9e-146">Accepts a `JsonPatchDocument<T>`, typically with [FromBody].</span></span>
* <span data-ttu-id="e6b9e-147">Chiama `ApplyTo` nel documento di patch per applicare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-147">Calls `ApplyTo` on the patch document to apply the changes.</span></span>

<span data-ttu-id="e6b9e-148">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="e6b9e-148">Here's an example:</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

<span data-ttu-id="e6b9e-149">Questo codice dell'app di esempio funziona con il modello `Customer` seguente.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-149">This code from the sample app works with the following `Customer` model.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

<span data-ttu-id="e6b9e-150">Il metodo di azione di esempio:</span><span class="sxs-lookup"><span data-stu-id="e6b9e-150">The sample action method:</span></span>

* <span data-ttu-id="e6b9e-151">Costruisce un oggetto `Customer`.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-151">Constructs a `Customer`.</span></span>
* <span data-ttu-id="e6b9e-152">Applica la patch.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-152">Applies the patch.</span></span>
* <span data-ttu-id="e6b9e-153">Restituisce il risultato nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-153">Returns the result in the body of the response.</span></span>

 <span data-ttu-id="e6b9e-154">In un'app reale il codice recupererebbe i dati da un archivio, ad esempio un database, e aggiornerebbe il database dopo aver applicato la patch.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-154">In a real app, the code would retrieve the data from a store such as a database and update the database after applying the patch.</span></span>

### <a name="model-state"></a><span data-ttu-id="e6b9e-155">Stato del modello</span><span class="sxs-lookup"><span data-stu-id="e6b9e-155">Model state</span></span>

<span data-ttu-id="e6b9e-156">L'esempio di metodo di azione precedente chiama un overload di `ApplyTo` che accetta lo stato del modello come uno dei propri parametri.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-156">The preceding action method example calls an overload of `ApplyTo` that takes model state as one of its parameters.</span></span> <span data-ttu-id="e6b9e-157">Con questa opzione, è possibile ottenere messaggi di errore nelle risposte.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-157">With this option, you can get error messages in responses.</span></span> <span data-ttu-id="e6b9e-158">L'esempio seguente mostra il corpo di una risposta 400 Richiesta non valida per un'operazione `test`:</span><span class="sxs-lookup"><span data-stu-id="e6b9e-158">The following example shows the body of a 400 Bad Request response for a `test` operation:</span></span>

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a><span data-ttu-id="e6b9e-159">Oggetti dinamici</span><span class="sxs-lookup"><span data-stu-id="e6b9e-159">Dynamic objects</span></span>

<span data-ttu-id="e6b9e-160">L'esempio di metodo di azione seguente mostra come applicare una patch a un oggetto dinamico.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-160">The following action method example shows how to apply a patch to a dynamic object.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a><span data-ttu-id="e6b9e-161">Operazione add</span><span class="sxs-lookup"><span data-stu-id="e6b9e-161">The add operation</span></span>

* <span data-ttu-id="e6b9e-162">Se `path` punta a un elemento della matrice: inserisce un nuovo elemento prima di quello specificato da `path`.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-162">If `path` points to an array element: inserts new element before the one specified by `path`.</span></span>
* <span data-ttu-id="e6b9e-163">Se `path` punta a una proprietà: imposta il valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-163">If `path` points to a property: sets the property value.</span></span>
* <span data-ttu-id="e6b9e-164">Se `path` punta a una posizione che non esiste:</span><span class="sxs-lookup"><span data-stu-id="e6b9e-164">If `path` points to a nonexistent location:</span></span>
  * <span data-ttu-id="e6b9e-165">Se la risorsa cui applicare la patch è un oggetto dinamico: aggiunge una proprietà.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-165">If the resource to patch is a dynamic object: adds a property.</span></span>
  * <span data-ttu-id="e6b9e-166">Se la risorsa cui applicare la patch è un oggetto statico: la richiesta non riesce.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-166">If the resource to patch is a static object: the request fails.</span></span>

<span data-ttu-id="e6b9e-167">Il documento di patch di esempio seguente imposta il valore di `CustomerName` e aggiunge un oggetto `Order` alla fine della matrice `Orders`.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-167">The following sample patch document sets the value of `CustomerName` and adds an `Order` object to the end of the `Orders` array.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a><span data-ttu-id="e6b9e-168">Operazione remove</span><span class="sxs-lookup"><span data-stu-id="e6b9e-168">The remove operation</span></span>

* <span data-ttu-id="e6b9e-169">Se `path` punta a un elemento della matrice: rimuove l'elemento.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-169">If `path` points to an array element: removes the element.</span></span>
* <span data-ttu-id="e6b9e-170">Se `path` punta a una proprietà:</span><span class="sxs-lookup"><span data-stu-id="e6b9e-170">If `path` points to a property:</span></span>
  * <span data-ttu-id="e6b9e-171">Se la risorsa cui applicare la patch è un oggetto dinamico: rimuove la proprietà.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-171">If resource to patch is a dynamic object: removes the property.</span></span>
  * <span data-ttu-id="e6b9e-172">Se la risorsa cui applicare la patch è un oggetto statico:</span><span class="sxs-lookup"><span data-stu-id="e6b9e-172">If resource to patch is a static object:</span></span> 
    * <span data-ttu-id="e6b9e-173">Se la proprietà ammette valori Null: la imposta su Null.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-173">If the property is nullable: sets it to null.</span></span>
    * <span data-ttu-id="e6b9e-174">Se la proprietà non ammette valori Null: la imposta su `default<T>`.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-174">If the property is non-nullable, sets it to `default<T>`.</span></span>

<span data-ttu-id="e6b9e-175">Il documento di patch di esempio seguente imposta `CustomerName` su Null ed elimina `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-175">The following sample patch document sets `CustomerName` to null and deletes `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a><span data-ttu-id="e6b9e-176">Operazione replace</span><span class="sxs-lookup"><span data-stu-id="e6b9e-176">The replace operation</span></span>

<span data-ttu-id="e6b9e-177">Questa operazione equivale a livello funzionale all'operazione `remove` seguita da un'operazione `add`.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-177">This operation is functionally the same as a `remove` followed by an `add`.</span></span>

<span data-ttu-id="e6b9e-178">Il documento di patch di esempio seguente imposta il valore di `CustomerName` e sostituisce `Orders[0]` con un nuovo oggetto `Order`.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-178">The following sample patch document sets the value of `CustomerName` and replaces `Orders[0]`with a new `Order` object.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a><span data-ttu-id="e6b9e-179">Operazione move</span><span class="sxs-lookup"><span data-stu-id="e6b9e-179">The move operation</span></span>

* <span data-ttu-id="e6b9e-180">Se `path` punta a un elemento della matrice: copia l'elemento `from` nella posizione dell'elemento `path`, quindi esegue un'operazione `remove` sull'elemento `from`.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-180">If `path` points to an array element: copies `from` element to location of `path` element, then runs a `remove` operation on the `from` element.</span></span>
* <span data-ttu-id="e6b9e-181">Se `path` punta a una proprietà: copia il valore della proprietà `from` nella proprietà `path`, quindi esegue un'operazione `remove` sulla proprietà `from`.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-181">If `path` points to a property: copies value of `from` property to `path` property, then runs a `remove` operation on the `from` property.</span></span>
* <span data-ttu-id="e6b9e-182">Se `path` punta a una proprietà che non esiste:</span><span class="sxs-lookup"><span data-stu-id="e6b9e-182">If `path` points to a nonexistent property:</span></span>
  * <span data-ttu-id="e6b9e-183">Se la risorsa cui applicare la patch è un oggetto statico: la richiesta non riesce.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-183">If the resource to patch is a static object: the request fails.</span></span>
  * <span data-ttu-id="e6b9e-184">Se la risorsa cui applicare la patch è un oggetto dinamico: copia la proprietà `from` nella posizione indicata da `path`, quindi esegue un'operazione `remove` sulla proprietà `from`.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-184">If the resource to patch is a dynamic object: copies `from` property to location indicated by `path`, then runs a `remove` operation on the `from` property.</span></span>

<span data-ttu-id="e6b9e-185">Il documento di patch di esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="e6b9e-185">The following sample patch document:</span></span>

* <span data-ttu-id="e6b9e-186">Copia il valore di `Orders[0].OrderName` in `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-186">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="e6b9e-187">Imposta `Orders[0].OrderName` su Null.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-187">Sets `Orders[0].OrderName` to null.</span></span>
* <span data-ttu-id="e6b9e-188">Sposta `Orders[1]` prima di `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-188">Moves `Orders[1]` to before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a><span data-ttu-id="e6b9e-189">Operazione copy</span><span class="sxs-lookup"><span data-stu-id="e6b9e-189">The copy operation</span></span>

<span data-ttu-id="e6b9e-190">Questa operazione equivale a livello funzionale all'operazione `move` senza il passaggio `remove` finale.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-190">This operation is functionally the same as a `move` operation without the final `remove` step.</span></span>

<span data-ttu-id="e6b9e-191">Il documento di patch di esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="e6b9e-191">The following sample patch document:</span></span>

* <span data-ttu-id="e6b9e-192">Copia il valore di `Orders[0].OrderName` in `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-192">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="e6b9e-193">Inserisce una copia di `Orders[1]` prima di `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-193">Inserts a copy of `Orders[1]` before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a><span data-ttu-id="e6b9e-194">Operazione test</span><span class="sxs-lookup"><span data-stu-id="e6b9e-194">The test operation</span></span>

<span data-ttu-id="e6b9e-195">Se il valore nella posizione indicata da `path` è diverso dal valore fornito in `value`, la richiesta non riesce.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-195">If the value at the location indicated by `path` is different from the value provided in `value`, the request fails.</span></span> <span data-ttu-id="e6b9e-196">In questo caso, l'intera richiesta PATCH non riesce anche se tutte le altre operazioni nel documento di patch potrebbero riuscire.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-196">In that case, the whole PATCH request fails even if all other operations in the patch document would otherwise succeed.</span></span>

<span data-ttu-id="e6b9e-197">L'operazione `test` viene usata in genere per impedire un aggiornamento in caso di conflitto di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-197">The `test` operation is commonly used to prevent an update when there's a concurrency conflict.</span></span>

<span data-ttu-id="e6b9e-198">Il documento di patch di esempio seguente non ha alcun effetto se il valore iniziale di `CustomerName` è "John", perché il test non riesce:</span><span class="sxs-lookup"><span data-stu-id="e6b9e-198">The following sample patch document has no effect if the initial value of `CustomerName` is "John", because the test fails:</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a><span data-ttu-id="e6b9e-199">Ottenere il codice</span><span class="sxs-lookup"><span data-stu-id="e6b9e-199">Get the code</span></span>

<span data-ttu-id="e6b9e-200">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span><span class="sxs-lookup"><span data-stu-id="e6b9e-200">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span></span> <span data-ttu-id="e6b9e-201">([Come scaricare un esempio](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="e6b9e-201">([How to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="e6b9e-202">Per testare l'esempio, eseguire l'app e inviare richieste HTTP con le impostazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="e6b9e-202">To test the sample, run the app and send HTTP requests with the following settings:</span></span>

* <span data-ttu-id="e6b9e-203">URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span><span class="sxs-lookup"><span data-stu-id="e6b9e-203">URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span></span>
* <span data-ttu-id="e6b9e-204">Metodo HTTP: `PATCH`</span><span class="sxs-lookup"><span data-stu-id="e6b9e-204">HTTP method: `PATCH`</span></span>
* <span data-ttu-id="e6b9e-205">Intestazione: `Content-Type: application/json-patch+json`</span><span class="sxs-lookup"><span data-stu-id="e6b9e-205">Header: `Content-Type: application/json-patch+json`</span></span>
* <span data-ttu-id="e6b9e-206">Corpo: copiare e incollare uno degli esempi di documento di patch JSON dalla cartella del progetto *JSON*.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-206">Body: Copy and paste one of the JSON patch document samples from the *JSON* project folder.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e6b9e-207">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e6b9e-207">Additional resources</span></span>

* [<span data-ttu-id="e6b9e-208">Specifica del metodo PATCH IETF RFC 5789</span><span class="sxs-lookup"><span data-stu-id="e6b9e-208">IETF RFC 5789 PATCH method specification</span></span>](https://tools.ietf.org/html/rfc5789)
* [<span data-ttu-id="e6b9e-209">Specifica JSON Patch IETF RFC 6902</span><span class="sxs-lookup"><span data-stu-id="e6b9e-209">IETF RFC 6902 JSON Patch specification</span></span>](https://tools.ietf.org/html/rfc6902)
* [<span data-ttu-id="e6b9e-210">Specifica del formato di percorso JSON Patch IETF RFC 6901</span><span class="sxs-lookup"><span data-stu-id="e6b9e-210">IETF RFC 6901 JSON Patch path format spec</span></span>](http://tools.ietf.org/html/rfc6901)
* <span data-ttu-id="e6b9e-211">[Documentazione di JSON Patch](http://jsonpatch.com/).</span><span class="sxs-lookup"><span data-stu-id="e6b9e-211">[JSON Patch documentation](http://jsonpatch.com/).</span></span> <span data-ttu-id="e6b9e-212">Include collegamenti a risorse per la creazione di documenti JSON Patch.</span><span class="sxs-lookup"><span data-stu-id="e6b9e-212">Includes links to resources for creating JSON Patch documents.</span></span>
* [<span data-ttu-id="e6b9e-213">Codice sorgente JSON Patch in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6b9e-213">ASP.NET Core JSON Patch source code</span></span>](https://github.com/aspnet/AspNetCore/tree/master/src/Features/JsonPatch/src)
