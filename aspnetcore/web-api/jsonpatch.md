---
title: JSON Patch nell'API Web ASP.NET Core
author: rick-anderson
description: Informazioni su come gestire le richieste JSON Patch in un'API Web ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2019
uid: web-api/jsonpatch
ms.openlocfilehash: cf1a00c1928652bf5210b2442087209e23b8868e
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661784"
---
# <a name="jsonpatch-in-aspnet-core-web-api"></a><span data-ttu-id="56635-103">JSON Patch nell'API Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56635-103">JsonPatch in ASP.NET Core web API</span></span>

<span data-ttu-id="56635-104">Di [Tom Dykstra](https://github.com/tdykstra) e [Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="56635-104">By [Tom Dykstra](https://github.com/tdykstra) and [Kirk Larkin](https://github.com/serpent5)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="56635-105">Questo articolo descrive come gestire le richieste JSON Patch in un'API Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="56635-105">This article explains how to handle JSON Patch requests in an ASP.NET Core web API.</span></span>

## <a name="package-installation"></a><span data-ttu-id="56635-106">Installazione del pacchetto</span><span class="sxs-lookup"><span data-stu-id="56635-106">Package installation</span></span>

<span data-ttu-id="56635-107">Il supporto per JsonPatch è abilitato con il pacchetto `Microsoft.AspNetCore.Mvc.NewtonsoftJson`.</span><span class="sxs-lookup"><span data-stu-id="56635-107">Support for JsonPatch is enabled using the `Microsoft.AspNetCore.Mvc.NewtonsoftJson` package.</span></span> <span data-ttu-id="56635-108">Per abilitare questa funzionalità, le app devono:</span><span class="sxs-lookup"><span data-stu-id="56635-108">To enable this feature, apps must:</span></span>

* <span data-ttu-id="56635-109">Installare il pacchetto NuGet [Microsoft. AspNetCore. Mvc. NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) .</span><span class="sxs-lookup"><span data-stu-id="56635-109">Install the [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package.</span></span>
* <span data-ttu-id="56635-110">Aggiornare il metodo `Startup.ConfigureServices` del progetto per includere una chiamata a `AddNewtonsoftJson`:</span><span class="sxs-lookup"><span data-stu-id="56635-110">Update the project's `Startup.ConfigureServices` method to include a call to `AddNewtonsoftJson`:</span></span>

  ```csharp
  services
      .AddControllersWithViews()
      .AddNewtonsoftJson();
  ```

<span data-ttu-id="56635-111">`AddNewtonsoftJson` è compatibile con i metodi di registrazione del servizio MVC:</span><span class="sxs-lookup"><span data-stu-id="56635-111">`AddNewtonsoftJson` is compatible with the MVC service registration methods:</span></span>

  * `AddRazorPages`
  * `AddControllersWithViews`
  * `AddControllers`

## <a name="jsonpatch-addnewtonsoftjson-and-systemtextjson"></a><span data-ttu-id="56635-112">JsonPatch, AddNewtonsoftJson e System. Text. JSON</span><span class="sxs-lookup"><span data-stu-id="56635-112">JsonPatch, AddNewtonsoftJson, and System.Text.Json</span></span>
  
<span data-ttu-id="56635-113">`AddNewtonsoftJson` sostituisce i formattatori di input e output basati su `System.Text.Json` usati per formattare **tutto** il contenuto JSON.</span><span class="sxs-lookup"><span data-stu-id="56635-113">`AddNewtonsoftJson` replaces the `System.Text.Json` based input and output formatters used for formatting **all** JSON content.</span></span> <span data-ttu-id="56635-114">Per aggiungere il supporto per `JsonPatch` usando `Newtonsoft.Json`, lasciando invariati gli altri formattatori, aggiornare il `Startup.ConfigureServices` del progetto come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="56635-114">To add support for `JsonPatch` using `Newtonsoft.Json`, while leaving the other formatters unchanged, update the project's `Startup.ConfigureServices` as follows:</span></span>

[!code-csharp[](jsonpatch/samples/3.0/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="56635-115">Il codice precedente richiede un riferimento a [Microsoft. AspNetCore. Mvc. NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson) e le istruzioni using seguenti:</span><span class="sxs-lookup"><span data-stu-id="56635-115">The preceding code requires a reference to [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson) and the following using statements:</span></span>

[!code-csharp[](jsonpatch/samples/3.0/WebApp1/Startup.cs?name=snippet1)]

## <a name="patch-http-request-method"></a><span data-ttu-id="56635-116">Metodo di richiesta HTTP PATCH</span><span class="sxs-lookup"><span data-stu-id="56635-116">PATCH HTTP request method</span></span>

<span data-ttu-id="56635-117">I metodi PUT e [PATCH](https://tools.ietf.org/html/rfc5789) vengono usati per aggiornare una risorsa esistente.</span><span class="sxs-lookup"><span data-stu-id="56635-117">The PUT and [PATCH](https://tools.ietf.org/html/rfc5789) methods are used to update an existing resource.</span></span> <span data-ttu-id="56635-118">La differenza tra i due metodi è che PUT sostituisce l'intera risorsa, mentre PATCH specifica solo le modifiche.</span><span class="sxs-lookup"><span data-stu-id="56635-118">The difference between them is that PUT replaces the entire resource, while PATCH specifies only the changes.</span></span>

## <a name="json-patch"></a><span data-ttu-id="56635-119">JSON Patch</span><span class="sxs-lookup"><span data-stu-id="56635-119">JSON Patch</span></span>

<span data-ttu-id="56635-120">[JSON Patch](https://tools.ietf.org/html/rfc6902) è un formato per specificare gli aggiornamenti da applicare a una risorsa.</span><span class="sxs-lookup"><span data-stu-id="56635-120">[JSON Patch](https://tools.ietf.org/html/rfc6902) is a format for specifying updates to be applied to a resource.</span></span> <span data-ttu-id="56635-121">Un documento JSON Patch è una matrice di *operazioni*.</span><span class="sxs-lookup"><span data-stu-id="56635-121">A JSON Patch document has an array of *operations*.</span></span> <span data-ttu-id="56635-122">Ogni operazione identifica un determinato tipo di modifica, ad esempio l'aggiunta di un elemento della matrice o la sostituzione di un valore di proprietà.</span><span class="sxs-lookup"><span data-stu-id="56635-122">Each operation identifies a particular type of change, such as add an array element or replace a property value.</span></span>

<span data-ttu-id="56635-123">Ad esempio, i documenti JSON seguenti rappresentano una risorsa, un documento di patch JSON per la risorsa e il risultato dell'applicazione delle operazioni patch.</span><span class="sxs-lookup"><span data-stu-id="56635-123">For example, the following JSON documents represent a resource, a JSON patch document for the resource, and the result of applying the patch operations.</span></span>

### <a name="resource-example"></a><span data-ttu-id="56635-124">Esempio di risorsa</span><span class="sxs-lookup"><span data-stu-id="56635-124">Resource example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a><span data-ttu-id="56635-125">Esempio di patch JSON</span><span class="sxs-lookup"><span data-stu-id="56635-125">JSON patch example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

<span data-ttu-id="56635-126">Nel codice JSON precedente:</span><span class="sxs-lookup"><span data-stu-id="56635-126">In the preceding JSON:</span></span>

* <span data-ttu-id="56635-127">La proprietà `op` indica il tipo di operazione.</span><span class="sxs-lookup"><span data-stu-id="56635-127">The `op` property indicates the type of operation.</span></span>
* <span data-ttu-id="56635-128">La proprietà `path` indica l'elemento da aggiornare.</span><span class="sxs-lookup"><span data-stu-id="56635-128">The `path` property indicates the element to update.</span></span>
* <span data-ttu-id="56635-129">La proprietà `value` fornisce il nuovo valore.</span><span class="sxs-lookup"><span data-stu-id="56635-129">The `value` property provides the new value.</span></span>

### <a name="resource-after-patch"></a><span data-ttu-id="56635-130">Risorsa dopo la patch</span><span class="sxs-lookup"><span data-stu-id="56635-130">Resource after patch</span></span>

<span data-ttu-id="56635-131">Ecco la risorsa dopo aver applicato il documento JSON Patch precedente:</span><span class="sxs-lookup"><span data-stu-id="56635-131">Here's the resource after applying the preceding JSON Patch document:</span></span>

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

<span data-ttu-id="56635-132">Le modifiche apportate tramite l'applicazione di un documento JSON Patch a una risorsa sono atomiche: se qualsiasi operazione nell'elenco non riesce, non viene applicata alcuna operazione contenuta nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="56635-132">The changes made by applying a JSON Patch document to a resource are atomic: if any operation in the list fails, no operation in the list is applied.</span></span>

## <a name="path-syntax"></a><span data-ttu-id="56635-133">Sintassi di path</span><span class="sxs-lookup"><span data-stu-id="56635-133">Path syntax</span></span>

<span data-ttu-id="56635-134">La proprietà [path](https://tools.ietf.org/html/rfc6901) di un oggetto operazione include barre tra livelli.</span><span class="sxs-lookup"><span data-stu-id="56635-134">The [path](https://tools.ietf.org/html/rfc6901) property of an operation object has slashes between levels.</span></span> <span data-ttu-id="56635-135">Ad esempio: `"/address/zipCode"`.</span><span class="sxs-lookup"><span data-stu-id="56635-135">For example, `"/address/zipCode"`.</span></span>

<span data-ttu-id="56635-136">Vengono usati indici in base zero per specificare gli elementi della matrice.</span><span class="sxs-lookup"><span data-stu-id="56635-136">Zero-based indexes are used to specify array elements.</span></span> <span data-ttu-id="56635-137">Il primo elemento della matrice `addresses` è in corrispondenza di `/addresses/0`.</span><span class="sxs-lookup"><span data-stu-id="56635-137">The first element of the `addresses` array would be at `/addresses/0`.</span></span> <span data-ttu-id="56635-138">Per aggiungere (`add`) un elemento alla fine della matrice, usare un trattino (-) invece di un numero di indice: `/addresses/-`.</span><span class="sxs-lookup"><span data-stu-id="56635-138">To `add` to the end of an array, use a hyphen (-) rather than an index number: `/addresses/-`.</span></span>

### <a name="operations"></a><span data-ttu-id="56635-139">Operazioni</span><span class="sxs-lookup"><span data-stu-id="56635-139">Operations</span></span>

<span data-ttu-id="56635-140">La tabella seguente mostra le operazioni supportate in base a quanto definito nella [specifica JSON Patch](https://tools.ietf.org/html/rfc6902):</span><span class="sxs-lookup"><span data-stu-id="56635-140">The following table shows supported operations as defined in the [JSON Patch specification](https://tools.ietf.org/html/rfc6902):</span></span>

|<span data-ttu-id="56635-141">Operazione</span><span class="sxs-lookup"><span data-stu-id="56635-141">Operation</span></span>  | <span data-ttu-id="56635-142">Note</span><span class="sxs-lookup"><span data-stu-id="56635-142">Notes</span></span> |
|-----------|--------------------------------|
| `add`     | <span data-ttu-id="56635-143">Aggiunge una proprietà o un elemento della matrice.</span><span class="sxs-lookup"><span data-stu-id="56635-143">Add a property or array element.</span></span> <span data-ttu-id="56635-144">Per la proprietà esistente: impostare il valore.</span><span class="sxs-lookup"><span data-stu-id="56635-144">For existing property: set value.</span></span>|
| `remove`  | <span data-ttu-id="56635-145">Rimuove una proprietà o un elemento della matrice.</span><span class="sxs-lookup"><span data-stu-id="56635-145">Remove a property or array element.</span></span> |
| `replace` | <span data-ttu-id="56635-146">Uguale a `remove` seguita da `add` nella stessa posizione.</span><span class="sxs-lookup"><span data-stu-id="56635-146">Same as `remove` followed by `add` at same location.</span></span> |
| `move`    | <span data-ttu-id="56635-147">Uguale a `remove` dall'origine seguita da `add` alla destinazione usando il valore dell'origine.</span><span class="sxs-lookup"><span data-stu-id="56635-147">Same as `remove` from source followed by `add` to destination using value from source.</span></span> |
| `copy`    | <span data-ttu-id="56635-148">Uguale a `add` alla destinazione usando il valore dell'origine.</span><span class="sxs-lookup"><span data-stu-id="56635-148">Same as `add` to destination using value from source.</span></span> |
| `test`    | <span data-ttu-id="56635-149">Restituisce il codice di stato di richiesta riuscita se il valore in `path` = `value` fornito.</span><span class="sxs-lookup"><span data-stu-id="56635-149">Return success status code if value at `path` = provided `value`.</span></span>|

## <a name="jsonpatch-in-aspnet-core"></a><span data-ttu-id="56635-150">JSON Patch in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56635-150">JsonPatch in ASP.NET Core</span></span>

<span data-ttu-id="56635-151">L'implementazione ASP.NET Core di JSON Patch viene fornita nel pacchetto NuGet [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/).</span><span class="sxs-lookup"><span data-stu-id="56635-151">The ASP.NET Core implementation of JSON Patch is provided in the [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet package.</span></span>

## <a name="action-method-code"></a><span data-ttu-id="56635-152">Codice del metodo di azione</span><span class="sxs-lookup"><span data-stu-id="56635-152">Action method code</span></span>

<span data-ttu-id="56635-153">In un controller API un metodo di azione per JSON Patch:</span><span class="sxs-lookup"><span data-stu-id="56635-153">In an API controller, an action method for JSON Patch:</span></span>

* <span data-ttu-id="56635-154">Viene annotato con l'attributo `HttpPatch`.</span><span class="sxs-lookup"><span data-stu-id="56635-154">Is annotated with the `HttpPatch` attribute.</span></span>
* <span data-ttu-id="56635-155">Accetta una `JsonPatchDocument<T>`, in genere con `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="56635-155">Accepts a `JsonPatchDocument<T>`, typically with `[FromBody]`.</span></span>
* <span data-ttu-id="56635-156">Chiama `ApplyTo` nel documento di patch per applicare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="56635-156">Calls `ApplyTo` on the patch document to apply the changes.</span></span>

<span data-ttu-id="56635-157">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="56635-157">Here's an example:</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

<span data-ttu-id="56635-158">Questo codice dell'app di esempio funziona con il modello `Customer` seguente.</span><span class="sxs-lookup"><span data-stu-id="56635-158">This code from the sample app works with the following `Customer` model.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

<span data-ttu-id="56635-159">Il metodo di azione di esempio:</span><span class="sxs-lookup"><span data-stu-id="56635-159">The sample action method:</span></span>

* <span data-ttu-id="56635-160">Costruisce un oggetto `Customer`.</span><span class="sxs-lookup"><span data-stu-id="56635-160">Constructs a `Customer`.</span></span>
* <span data-ttu-id="56635-161">Applica la patch.</span><span class="sxs-lookup"><span data-stu-id="56635-161">Applies the patch.</span></span>
* <span data-ttu-id="56635-162">Restituisce il risultato nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="56635-162">Returns the result in the body of the response.</span></span>

 <span data-ttu-id="56635-163">In un'app reale il codice recupererebbe i dati da un archivio, ad esempio un database, e aggiornerebbe il database dopo aver applicato la patch.</span><span class="sxs-lookup"><span data-stu-id="56635-163">In a real app, the code would retrieve the data from a store such as a database and update the database after applying the patch.</span></span>

### <a name="model-state"></a><span data-ttu-id="56635-164">Stato del modello</span><span class="sxs-lookup"><span data-stu-id="56635-164">Model state</span></span>

<span data-ttu-id="56635-165">L'esempio di metodo di azione precedente chiama un overload di `ApplyTo` che accetta lo stato del modello come uno dei propri parametri.</span><span class="sxs-lookup"><span data-stu-id="56635-165">The preceding action method example calls an overload of `ApplyTo` that takes model state as one of its parameters.</span></span> <span data-ttu-id="56635-166">Con questa opzione, è possibile ottenere messaggi di errore nelle risposte.</span><span class="sxs-lookup"><span data-stu-id="56635-166">With this option, you can get error messages in responses.</span></span> <span data-ttu-id="56635-167">L'esempio seguente mostra il corpo di una risposta 400 Richiesta non valida per un'operazione `test`:</span><span class="sxs-lookup"><span data-stu-id="56635-167">The following example shows the body of a 400 Bad Request response for a `test` operation:</span></span>

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a><span data-ttu-id="56635-168">Oggetti dinamici</span><span class="sxs-lookup"><span data-stu-id="56635-168">Dynamic objects</span></span>

<span data-ttu-id="56635-169">L'esempio di metodo di azione seguente mostra come applicare una patch a un oggetto dinamico.</span><span class="sxs-lookup"><span data-stu-id="56635-169">The following action method example shows how to apply a patch to a dynamic object.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a><span data-ttu-id="56635-170">Operazione add</span><span class="sxs-lookup"><span data-stu-id="56635-170">The add operation</span></span>

* <span data-ttu-id="56635-171">Se `path` punta a un elemento della matrice: inserisce un nuovo elemento prima di quello specificato da `path`.</span><span class="sxs-lookup"><span data-stu-id="56635-171">If `path` points to an array element: inserts new element before the one specified by `path`.</span></span>
* <span data-ttu-id="56635-172">Se `path` punta a una proprietà: imposta il valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="56635-172">If `path` points to a property: sets the property value.</span></span>
* <span data-ttu-id="56635-173">Se `path` punta a una posizione che non esiste:</span><span class="sxs-lookup"><span data-stu-id="56635-173">If `path` points to a nonexistent location:</span></span>
  * <span data-ttu-id="56635-174">Se la risorsa cui applicare la patch è un oggetto dinamico: aggiunge una proprietà.</span><span class="sxs-lookup"><span data-stu-id="56635-174">If the resource to patch is a dynamic object: adds a property.</span></span>
  * <span data-ttu-id="56635-175">Se la risorsa cui applicare la patch è un oggetto statico: la richiesta non riesce.</span><span class="sxs-lookup"><span data-stu-id="56635-175">If the resource to patch is a static object: the request fails.</span></span>

<span data-ttu-id="56635-176">Il documento di patch di esempio seguente imposta il valore di `CustomerName` e aggiunge un oggetto `Order` alla fine della matrice `Orders`.</span><span class="sxs-lookup"><span data-stu-id="56635-176">The following sample patch document sets the value of `CustomerName` and adds an `Order` object to the end of the `Orders` array.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a><span data-ttu-id="56635-177">Operazione remove</span><span class="sxs-lookup"><span data-stu-id="56635-177">The remove operation</span></span>

* <span data-ttu-id="56635-178">Se `path` punta a un elemento della matrice: rimuove l'elemento.</span><span class="sxs-lookup"><span data-stu-id="56635-178">If `path` points to an array element: removes the element.</span></span>
* <span data-ttu-id="56635-179">Se `path` punta a una proprietà:</span><span class="sxs-lookup"><span data-stu-id="56635-179">If `path` points to a property:</span></span>
  * <span data-ttu-id="56635-180">Se la risorsa cui applicare la patch è un oggetto dinamico: rimuove la proprietà.</span><span class="sxs-lookup"><span data-stu-id="56635-180">If resource to patch is a dynamic object: removes the property.</span></span>
  * <span data-ttu-id="56635-181">Se la risorsa cui applicare la patch è un oggetto statico:</span><span class="sxs-lookup"><span data-stu-id="56635-181">If resource to patch is a static object:</span></span>
    * <span data-ttu-id="56635-182">Se la proprietà ammette valori Null: la imposta su Null.</span><span class="sxs-lookup"><span data-stu-id="56635-182">If the property is nullable: sets it to null.</span></span>
    * <span data-ttu-id="56635-183">Se la proprietà non ammette valori Null: la imposta su `default<T>`.</span><span class="sxs-lookup"><span data-stu-id="56635-183">If the property is non-nullable, sets it to `default<T>`.</span></span>

<span data-ttu-id="56635-184">Il documento di patch di esempio seguente imposta `CustomerName` su Null ed elimina `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="56635-184">The following sample patch document sets `CustomerName` to null and deletes `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a><span data-ttu-id="56635-185">Operazione replace</span><span class="sxs-lookup"><span data-stu-id="56635-185">The replace operation</span></span>

<span data-ttu-id="56635-186">Questa operazione equivale a livello funzionale all'operazione `remove` seguita da un'operazione `add`.</span><span class="sxs-lookup"><span data-stu-id="56635-186">This operation is functionally the same as a `remove` followed by an `add`.</span></span>

<span data-ttu-id="56635-187">Il documento di patch di esempio seguente imposta il valore di `CustomerName` e sostituisce `Orders[0]` con un nuovo oggetto `Order`.</span><span class="sxs-lookup"><span data-stu-id="56635-187">The following sample patch document sets the value of `CustomerName` and replaces `Orders[0]`with a new `Order` object.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a><span data-ttu-id="56635-188">Operazione move</span><span class="sxs-lookup"><span data-stu-id="56635-188">The move operation</span></span>

* <span data-ttu-id="56635-189">Se `path` punta a un elemento della matrice: copia l'elemento `from` nella posizione dell'elemento `path`, quindi esegue un'operazione `remove` sull'elemento `from`.</span><span class="sxs-lookup"><span data-stu-id="56635-189">If `path` points to an array element: copies `from` element to location of `path` element, then runs a `remove` operation on the `from` element.</span></span>
* <span data-ttu-id="56635-190">Se `path` punta a una proprietà: copia il valore della proprietà `from` nella proprietà `path`, quindi esegue un'operazione `remove` sulla proprietà `from`.</span><span class="sxs-lookup"><span data-stu-id="56635-190">If `path` points to a property: copies value of `from` property to `path` property, then runs a `remove` operation on the `from` property.</span></span>
* <span data-ttu-id="56635-191">Se `path` punta a una proprietà che non esiste:</span><span class="sxs-lookup"><span data-stu-id="56635-191">If `path` points to a nonexistent property:</span></span>
  * <span data-ttu-id="56635-192">Se la risorsa cui applicare la patch è un oggetto statico: la richiesta non riesce.</span><span class="sxs-lookup"><span data-stu-id="56635-192">If the resource to patch is a static object: the request fails.</span></span>
  * <span data-ttu-id="56635-193">Se la risorsa cui applicare la patch è un oggetto dinamico: copia la proprietà `from` nella posizione indicata da `path`, quindi esegue un'operazione `remove` sulla proprietà `from`.</span><span class="sxs-lookup"><span data-stu-id="56635-193">If the resource to patch is a dynamic object: copies `from` property to location indicated by `path`, then runs a `remove` operation on the `from` property.</span></span>

<span data-ttu-id="56635-194">Il documento di patch di esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="56635-194">The following sample patch document:</span></span>

* <span data-ttu-id="56635-195">Copia il valore di `Orders[0].OrderName` in `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="56635-195">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="56635-196">Imposta `Orders[0].OrderName` su Null.</span><span class="sxs-lookup"><span data-stu-id="56635-196">Sets `Orders[0].OrderName` to null.</span></span>
* <span data-ttu-id="56635-197">Sposta `Orders[1]` prima di `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="56635-197">Moves `Orders[1]` to before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a><span data-ttu-id="56635-198">Operazione copy</span><span class="sxs-lookup"><span data-stu-id="56635-198">The copy operation</span></span>

<span data-ttu-id="56635-199">Questa operazione equivale a livello funzionale all'operazione `move` senza il passaggio `remove` finale.</span><span class="sxs-lookup"><span data-stu-id="56635-199">This operation is functionally the same as a `move` operation without the final `remove` step.</span></span>

<span data-ttu-id="56635-200">Il documento di patch di esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="56635-200">The following sample patch document:</span></span>

* <span data-ttu-id="56635-201">Copia il valore di `Orders[0].OrderName` in `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="56635-201">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="56635-202">Inserisce una copia di `Orders[1]` prima di `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="56635-202">Inserts a copy of `Orders[1]` before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a><span data-ttu-id="56635-203">Operazione test</span><span class="sxs-lookup"><span data-stu-id="56635-203">The test operation</span></span>

<span data-ttu-id="56635-204">Se il valore nella posizione indicata da `path` è diverso dal valore fornito in `value`, la richiesta non riesce.</span><span class="sxs-lookup"><span data-stu-id="56635-204">If the value at the location indicated by `path` is different from the value provided in `value`, the request fails.</span></span> <span data-ttu-id="56635-205">In questo caso, l'intera richiesta PATCH non riesce anche se tutte le altre operazioni nel documento di patch potrebbero riuscire.</span><span class="sxs-lookup"><span data-stu-id="56635-205">In that case, the whole PATCH request fails even if all other operations in the patch document would otherwise succeed.</span></span>

<span data-ttu-id="56635-206">L'operazione `test` viene usata in genere per impedire un aggiornamento in caso di conflitto di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="56635-206">The `test` operation is commonly used to prevent an update when there's a concurrency conflict.</span></span>

<span data-ttu-id="56635-207">Il documento di patch di esempio seguente non ha alcun effetto se il valore iniziale di `CustomerName` è "John", perché il test non riesce:</span><span class="sxs-lookup"><span data-stu-id="56635-207">The following sample patch document has no effect if the initial value of `CustomerName` is "John", because the test fails:</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a><span data-ttu-id="56635-208">Ottenere il codice</span><span class="sxs-lookup"><span data-stu-id="56635-208">Get the code</span></span>

<span data-ttu-id="56635-209">[Visualizzare o scaricare il codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span><span class="sxs-lookup"><span data-stu-id="56635-209">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span></span> <span data-ttu-id="56635-210">([Come scaricare un esempio](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="56635-210">([How to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="56635-211">Per testare l'esempio, eseguire l'app e inviare richieste HTTP con le impostazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="56635-211">To test the sample, run the app and send HTTP requests with the following settings:</span></span>

* <span data-ttu-id="56635-212">URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span><span class="sxs-lookup"><span data-stu-id="56635-212">URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span></span>
* <span data-ttu-id="56635-213">Metodo HTTP: `PATCH`</span><span class="sxs-lookup"><span data-stu-id="56635-213">HTTP method: `PATCH`</span></span>
* <span data-ttu-id="56635-214">Intestazione: `Content-Type: application/json-patch+json`</span><span class="sxs-lookup"><span data-stu-id="56635-214">Header: `Content-Type: application/json-patch+json`</span></span>
* <span data-ttu-id="56635-215">Body: copiare e incollare uno degli esempi di documenti patch JSON dalla cartella del progetto *JSON* .</span><span class="sxs-lookup"><span data-stu-id="56635-215">Body: Copy and paste one of the JSON patch document samples from the *JSON* project folder.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="56635-216">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="56635-216">Additional resources</span></span>

* [<span data-ttu-id="56635-217">Specifica del metodo PATCH IETF RFC 5789</span><span class="sxs-lookup"><span data-stu-id="56635-217">IETF RFC 5789 PATCH method specification</span></span>](https://tools.ietf.org/html/rfc5789)
* [<span data-ttu-id="56635-218">Specifica JSON Patch IETF RFC 6902</span><span class="sxs-lookup"><span data-stu-id="56635-218">IETF RFC 6902 JSON Patch specification</span></span>](https://tools.ietf.org/html/rfc6902)
* [<span data-ttu-id="56635-219">Specifica del formato di percorso JSON Patch IETF RFC 6901</span><span class="sxs-lookup"><span data-stu-id="56635-219">IETF RFC 6901 JSON Patch path format spec</span></span>](https://tools.ietf.org/html/rfc6901)
* <span data-ttu-id="56635-220">[Documentazione di JSON Patch](https://jsonpatch.com/).</span><span class="sxs-lookup"><span data-stu-id="56635-220">[JSON Patch documentation](https://jsonpatch.com/).</span></span> <span data-ttu-id="56635-221">Include collegamenti a risorse per la creazione di documenti JSON Patch.</span><span class="sxs-lookup"><span data-stu-id="56635-221">Includes links to resources for creating JSON Patch documents.</span></span>
* [<span data-ttu-id="56635-222">Codice sorgente JSON Patch in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56635-222">ASP.NET Core JSON Patch source code</span></span>](https://github.com/dotnet/AspNetCore/tree/master/src/Features/JsonPatch/src)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="56635-223">Questo articolo descrive come gestire le richieste JSON Patch in un'API Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="56635-223">This article explains how to handle JSON Patch requests in an ASP.NET Core web API.</span></span>

## <a name="patch-http-request-method"></a><span data-ttu-id="56635-224">Metodo di richiesta HTTP PATCH</span><span class="sxs-lookup"><span data-stu-id="56635-224">PATCH HTTP request method</span></span>

<span data-ttu-id="56635-225">I metodi PUT e [PATCH](https://tools.ietf.org/html/rfc5789) vengono usati per aggiornare una risorsa esistente.</span><span class="sxs-lookup"><span data-stu-id="56635-225">The PUT and [PATCH](https://tools.ietf.org/html/rfc5789) methods are used to update an existing resource.</span></span> <span data-ttu-id="56635-226">La differenza tra i due metodi è che PUT sostituisce l'intera risorsa, mentre PATCH specifica solo le modifiche.</span><span class="sxs-lookup"><span data-stu-id="56635-226">The difference between them is that PUT replaces the entire resource, while PATCH specifies only the changes.</span></span>

## <a name="json-patch"></a><span data-ttu-id="56635-227">JSON Patch</span><span class="sxs-lookup"><span data-stu-id="56635-227">JSON Patch</span></span>

<span data-ttu-id="56635-228">[JSON Patch](https://tools.ietf.org/html/rfc6902) è un formato per specificare gli aggiornamenti da applicare a una risorsa.</span><span class="sxs-lookup"><span data-stu-id="56635-228">[JSON Patch](https://tools.ietf.org/html/rfc6902) is a format for specifying updates to be applied to a resource.</span></span> <span data-ttu-id="56635-229">Un documento JSON Patch è una matrice di *operazioni*.</span><span class="sxs-lookup"><span data-stu-id="56635-229">A JSON Patch document has an array of *operations*.</span></span> <span data-ttu-id="56635-230">Ogni operazione identifica un determinato tipo di modifica, ad esempio l'aggiunta di un elemento della matrice o la sostituzione di un valore di proprietà.</span><span class="sxs-lookup"><span data-stu-id="56635-230">Each operation identifies a particular type of change, such as add an array element or replace a property value.</span></span>

<span data-ttu-id="56635-231">Ad esempio, i documenti JSON seguenti rappresentano una risorsa, un documento di patch JSON per la risorsa e il risultato dell'applicazione delle operazioni patch.</span><span class="sxs-lookup"><span data-stu-id="56635-231">For example, the following JSON documents represent a resource, a JSON patch document for the resource, and the result of applying the patch operations.</span></span>

### <a name="resource-example"></a><span data-ttu-id="56635-232">Esempio di risorsa</span><span class="sxs-lookup"><span data-stu-id="56635-232">Resource example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a><span data-ttu-id="56635-233">Esempio di patch JSON</span><span class="sxs-lookup"><span data-stu-id="56635-233">JSON patch example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

<span data-ttu-id="56635-234">Nel codice JSON precedente:</span><span class="sxs-lookup"><span data-stu-id="56635-234">In the preceding JSON:</span></span>

* <span data-ttu-id="56635-235">La proprietà `op` indica il tipo di operazione.</span><span class="sxs-lookup"><span data-stu-id="56635-235">The `op` property indicates the type of operation.</span></span>
* <span data-ttu-id="56635-236">La proprietà `path` indica l'elemento da aggiornare.</span><span class="sxs-lookup"><span data-stu-id="56635-236">The `path` property indicates the element to update.</span></span>
* <span data-ttu-id="56635-237">La proprietà `value` fornisce il nuovo valore.</span><span class="sxs-lookup"><span data-stu-id="56635-237">The `value` property provides the new value.</span></span>

### <a name="resource-after-patch"></a><span data-ttu-id="56635-238">Risorsa dopo la patch</span><span class="sxs-lookup"><span data-stu-id="56635-238">Resource after patch</span></span>

<span data-ttu-id="56635-239">Ecco la risorsa dopo aver applicato il documento JSON Patch precedente:</span><span class="sxs-lookup"><span data-stu-id="56635-239">Here's the resource after applying the preceding JSON Patch document:</span></span>

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

<span data-ttu-id="56635-240">Le modifiche apportate tramite l'applicazione di un documento JSON Patch a una risorsa sono atomiche: se qualsiasi operazione nell'elenco non riesce, non viene applicata alcuna operazione contenuta nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="56635-240">The changes made by applying a JSON Patch document to a resource are atomic: if any operation in the list fails, no operation in the list is applied.</span></span>

## <a name="path-syntax"></a><span data-ttu-id="56635-241">Sintassi di path</span><span class="sxs-lookup"><span data-stu-id="56635-241">Path syntax</span></span>

<span data-ttu-id="56635-242">La proprietà [path](https://tools.ietf.org/html/rfc6901) di un oggetto operazione include barre tra livelli.</span><span class="sxs-lookup"><span data-stu-id="56635-242">The [path](https://tools.ietf.org/html/rfc6901) property of an operation object has slashes between levels.</span></span> <span data-ttu-id="56635-243">Ad esempio: `"/address/zipCode"`.</span><span class="sxs-lookup"><span data-stu-id="56635-243">For example, `"/address/zipCode"`.</span></span>

<span data-ttu-id="56635-244">Vengono usati indici in base zero per specificare gli elementi della matrice.</span><span class="sxs-lookup"><span data-stu-id="56635-244">Zero-based indexes are used to specify array elements.</span></span> <span data-ttu-id="56635-245">Il primo elemento della matrice `addresses` è in corrispondenza di `/addresses/0`.</span><span class="sxs-lookup"><span data-stu-id="56635-245">The first element of the `addresses` array would be at `/addresses/0`.</span></span> <span data-ttu-id="56635-246">Per aggiungere (`add`) un elemento alla fine della matrice, usare un trattino (-) invece di un numero di indice: `/addresses/-`.</span><span class="sxs-lookup"><span data-stu-id="56635-246">To `add` to the end of an array, use a hyphen (-) rather than an index number: `/addresses/-`.</span></span>

### <a name="operations"></a><span data-ttu-id="56635-247">Operazioni</span><span class="sxs-lookup"><span data-stu-id="56635-247">Operations</span></span>

<span data-ttu-id="56635-248">La tabella seguente mostra le operazioni supportate in base a quanto definito nella [specifica JSON Patch](https://tools.ietf.org/html/rfc6902):</span><span class="sxs-lookup"><span data-stu-id="56635-248">The following table shows supported operations as defined in the [JSON Patch specification](https://tools.ietf.org/html/rfc6902):</span></span>

|<span data-ttu-id="56635-249">Operazione</span><span class="sxs-lookup"><span data-stu-id="56635-249">Operation</span></span>  | <span data-ttu-id="56635-250">Note</span><span class="sxs-lookup"><span data-stu-id="56635-250">Notes</span></span> |
|-----------|--------------------------------|
| `add`     | <span data-ttu-id="56635-251">Aggiunge una proprietà o un elemento della matrice.</span><span class="sxs-lookup"><span data-stu-id="56635-251">Add a property or array element.</span></span> <span data-ttu-id="56635-252">Per la proprietà esistente: impostare il valore.</span><span class="sxs-lookup"><span data-stu-id="56635-252">For existing property: set value.</span></span>|
| `remove`  | <span data-ttu-id="56635-253">Rimuove una proprietà o un elemento della matrice.</span><span class="sxs-lookup"><span data-stu-id="56635-253">Remove a property or array element.</span></span> |
| `replace` | <span data-ttu-id="56635-254">Uguale a `remove` seguita da `add` nella stessa posizione.</span><span class="sxs-lookup"><span data-stu-id="56635-254">Same as `remove` followed by `add` at same location.</span></span> |
| `move`    | <span data-ttu-id="56635-255">Uguale a `remove` dall'origine seguita da `add` alla destinazione usando il valore dell'origine.</span><span class="sxs-lookup"><span data-stu-id="56635-255">Same as `remove` from source followed by `add` to destination using value from source.</span></span> |
| `copy`    | <span data-ttu-id="56635-256">Uguale a `add` alla destinazione usando il valore dell'origine.</span><span class="sxs-lookup"><span data-stu-id="56635-256">Same as `add` to destination using value from source.</span></span> |
| `test`    | <span data-ttu-id="56635-257">Restituisce il codice di stato di richiesta riuscita se il valore in `path` = `value` fornito.</span><span class="sxs-lookup"><span data-stu-id="56635-257">Return success status code if value at `path` = provided `value`.</span></span>|

## <a name="jsonpatch-in-aspnet-core"></a><span data-ttu-id="56635-258">JSON Patch in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56635-258">JsonPatch in ASP.NET Core</span></span>

<span data-ttu-id="56635-259">L'implementazione ASP.NET Core di JSON Patch viene fornita nel pacchetto NuGet [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/).</span><span class="sxs-lookup"><span data-stu-id="56635-259">The ASP.NET Core implementation of JSON Patch is provided in the [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet package.</span></span> <span data-ttu-id="56635-260">Il pacchetto è incluso nel metapacchetto [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="56635-260">The package is included in the [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app) metapackage.</span></span>

## <a name="action-method-code"></a><span data-ttu-id="56635-261">Codice del metodo di azione</span><span class="sxs-lookup"><span data-stu-id="56635-261">Action method code</span></span>

<span data-ttu-id="56635-262">In un controller API un metodo di azione per JSON Patch:</span><span class="sxs-lookup"><span data-stu-id="56635-262">In an API controller, an action method for JSON Patch:</span></span>

* <span data-ttu-id="56635-263">Viene annotato con l'attributo `HttpPatch`.</span><span class="sxs-lookup"><span data-stu-id="56635-263">Is annotated with the `HttpPatch` attribute.</span></span>
* <span data-ttu-id="56635-264">Accetta una `JsonPatchDocument<T>`, in genere con `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="56635-264">Accepts a `JsonPatchDocument<T>`, typically with `[FromBody]`.</span></span>
* <span data-ttu-id="56635-265">Chiama `ApplyTo` nel documento di patch per applicare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="56635-265">Calls `ApplyTo` on the patch document to apply the changes.</span></span>

<span data-ttu-id="56635-266">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="56635-266">Here's an example:</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

<span data-ttu-id="56635-267">Questo codice dell'app di esempio funziona con il modello `Customer` seguente.</span><span class="sxs-lookup"><span data-stu-id="56635-267">This code from the sample app works with the following `Customer` model.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

<span data-ttu-id="56635-268">Il metodo di azione di esempio:</span><span class="sxs-lookup"><span data-stu-id="56635-268">The sample action method:</span></span>

* <span data-ttu-id="56635-269">Costruisce un oggetto `Customer`.</span><span class="sxs-lookup"><span data-stu-id="56635-269">Constructs a `Customer`.</span></span>
* <span data-ttu-id="56635-270">Applica la patch.</span><span class="sxs-lookup"><span data-stu-id="56635-270">Applies the patch.</span></span>
* <span data-ttu-id="56635-271">Restituisce il risultato nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="56635-271">Returns the result in the body of the response.</span></span>

 <span data-ttu-id="56635-272">In un'app reale il codice recupererebbe i dati da un archivio, ad esempio un database, e aggiornerebbe il database dopo aver applicato la patch.</span><span class="sxs-lookup"><span data-stu-id="56635-272">In a real app, the code would retrieve the data from a store such as a database and update the database after applying the patch.</span></span>

### <a name="model-state"></a><span data-ttu-id="56635-273">Stato del modello</span><span class="sxs-lookup"><span data-stu-id="56635-273">Model state</span></span>

<span data-ttu-id="56635-274">L'esempio di metodo di azione precedente chiama un overload di `ApplyTo` che accetta lo stato del modello come uno dei propri parametri.</span><span class="sxs-lookup"><span data-stu-id="56635-274">The preceding action method example calls an overload of `ApplyTo` that takes model state as one of its parameters.</span></span> <span data-ttu-id="56635-275">Con questa opzione, è possibile ottenere messaggi di errore nelle risposte.</span><span class="sxs-lookup"><span data-stu-id="56635-275">With this option, you can get error messages in responses.</span></span> <span data-ttu-id="56635-276">L'esempio seguente mostra il corpo di una risposta 400 Richiesta non valida per un'operazione `test`:</span><span class="sxs-lookup"><span data-stu-id="56635-276">The following example shows the body of a 400 Bad Request response for a `test` operation:</span></span>

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a><span data-ttu-id="56635-277">Oggetti dinamici</span><span class="sxs-lookup"><span data-stu-id="56635-277">Dynamic objects</span></span>

<span data-ttu-id="56635-278">L'esempio di metodo di azione seguente mostra come applicare una patch a un oggetto dinamico.</span><span class="sxs-lookup"><span data-stu-id="56635-278">The following action method example shows how to apply a patch to a dynamic object.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a><span data-ttu-id="56635-279">Operazione add</span><span class="sxs-lookup"><span data-stu-id="56635-279">The add operation</span></span>

* <span data-ttu-id="56635-280">Se `path` punta a un elemento della matrice: inserisce un nuovo elemento prima di quello specificato da `path`.</span><span class="sxs-lookup"><span data-stu-id="56635-280">If `path` points to an array element: inserts new element before the one specified by `path`.</span></span>
* <span data-ttu-id="56635-281">Se `path` punta a una proprietà: imposta il valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="56635-281">If `path` points to a property: sets the property value.</span></span>
* <span data-ttu-id="56635-282">Se `path` punta a una posizione che non esiste:</span><span class="sxs-lookup"><span data-stu-id="56635-282">If `path` points to a nonexistent location:</span></span>
  * <span data-ttu-id="56635-283">Se la risorsa cui applicare la patch è un oggetto dinamico: aggiunge una proprietà.</span><span class="sxs-lookup"><span data-stu-id="56635-283">If the resource to patch is a dynamic object: adds a property.</span></span>
  * <span data-ttu-id="56635-284">Se la risorsa cui applicare la patch è un oggetto statico: la richiesta non riesce.</span><span class="sxs-lookup"><span data-stu-id="56635-284">If the resource to patch is a static object: the request fails.</span></span>

<span data-ttu-id="56635-285">Il documento di patch di esempio seguente imposta il valore di `CustomerName` e aggiunge un oggetto `Order` alla fine della matrice `Orders`.</span><span class="sxs-lookup"><span data-stu-id="56635-285">The following sample patch document sets the value of `CustomerName` and adds an `Order` object to the end of the `Orders` array.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a><span data-ttu-id="56635-286">Operazione remove</span><span class="sxs-lookup"><span data-stu-id="56635-286">The remove operation</span></span>

* <span data-ttu-id="56635-287">Se `path` punta a un elemento della matrice: rimuove l'elemento.</span><span class="sxs-lookup"><span data-stu-id="56635-287">If `path` points to an array element: removes the element.</span></span>
* <span data-ttu-id="56635-288">Se `path` punta a una proprietà:</span><span class="sxs-lookup"><span data-stu-id="56635-288">If `path` points to a property:</span></span>
  * <span data-ttu-id="56635-289">Se la risorsa cui applicare la patch è un oggetto dinamico: rimuove la proprietà.</span><span class="sxs-lookup"><span data-stu-id="56635-289">If resource to patch is a dynamic object: removes the property.</span></span>
  * <span data-ttu-id="56635-290">Se la risorsa cui applicare la patch è un oggetto statico:</span><span class="sxs-lookup"><span data-stu-id="56635-290">If resource to patch is a static object:</span></span>
    * <span data-ttu-id="56635-291">Se la proprietà ammette valori Null: la imposta su Null.</span><span class="sxs-lookup"><span data-stu-id="56635-291">If the property is nullable: sets it to null.</span></span>
    * <span data-ttu-id="56635-292">Se la proprietà non ammette valori Null: la imposta su `default<T>`.</span><span class="sxs-lookup"><span data-stu-id="56635-292">If the property is non-nullable, sets it to `default<T>`.</span></span>

<span data-ttu-id="56635-293">Il documento di patch di esempio seguente imposta `CustomerName` su Null ed elimina `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="56635-293">The following sample patch document sets `CustomerName` to null and deletes `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a><span data-ttu-id="56635-294">Operazione replace</span><span class="sxs-lookup"><span data-stu-id="56635-294">The replace operation</span></span>

<span data-ttu-id="56635-295">Questa operazione equivale a livello funzionale all'operazione `remove` seguita da un'operazione `add`.</span><span class="sxs-lookup"><span data-stu-id="56635-295">This operation is functionally the same as a `remove` followed by an `add`.</span></span>

<span data-ttu-id="56635-296">Il documento di patch di esempio seguente imposta il valore di `CustomerName` e sostituisce `Orders[0]` con un nuovo oggetto `Order`.</span><span class="sxs-lookup"><span data-stu-id="56635-296">The following sample patch document sets the value of `CustomerName` and replaces `Orders[0]`with a new `Order` object.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a><span data-ttu-id="56635-297">Operazione move</span><span class="sxs-lookup"><span data-stu-id="56635-297">The move operation</span></span>

* <span data-ttu-id="56635-298">Se `path` punta a un elemento della matrice: copia l'elemento `from` nella posizione dell'elemento `path`, quindi esegue un'operazione `remove` sull'elemento `from`.</span><span class="sxs-lookup"><span data-stu-id="56635-298">If `path` points to an array element: copies `from` element to location of `path` element, then runs a `remove` operation on the `from` element.</span></span>
* <span data-ttu-id="56635-299">Se `path` punta a una proprietà: copia il valore della proprietà `from` nella proprietà `path`, quindi esegue un'operazione `remove` sulla proprietà `from`.</span><span class="sxs-lookup"><span data-stu-id="56635-299">If `path` points to a property: copies value of `from` property to `path` property, then runs a `remove` operation on the `from` property.</span></span>
* <span data-ttu-id="56635-300">Se `path` punta a una proprietà che non esiste:</span><span class="sxs-lookup"><span data-stu-id="56635-300">If `path` points to a nonexistent property:</span></span>
  * <span data-ttu-id="56635-301">Se la risorsa cui applicare la patch è un oggetto statico: la richiesta non riesce.</span><span class="sxs-lookup"><span data-stu-id="56635-301">If the resource to patch is a static object: the request fails.</span></span>
  * <span data-ttu-id="56635-302">Se la risorsa cui applicare la patch è un oggetto dinamico: copia la proprietà `from` nella posizione indicata da `path`, quindi esegue un'operazione `remove` sulla proprietà `from`.</span><span class="sxs-lookup"><span data-stu-id="56635-302">If the resource to patch is a dynamic object: copies `from` property to location indicated by `path`, then runs a `remove` operation on the `from` property.</span></span>

<span data-ttu-id="56635-303">Il documento di patch di esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="56635-303">The following sample patch document:</span></span>

* <span data-ttu-id="56635-304">Copia il valore di `Orders[0].OrderName` in `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="56635-304">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="56635-305">Imposta `Orders[0].OrderName` su Null.</span><span class="sxs-lookup"><span data-stu-id="56635-305">Sets `Orders[0].OrderName` to null.</span></span>
* <span data-ttu-id="56635-306">Sposta `Orders[1]` prima di `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="56635-306">Moves `Orders[1]` to before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a><span data-ttu-id="56635-307">Operazione copy</span><span class="sxs-lookup"><span data-stu-id="56635-307">The copy operation</span></span>

<span data-ttu-id="56635-308">Questa operazione equivale a livello funzionale all'operazione `move` senza il passaggio `remove` finale.</span><span class="sxs-lookup"><span data-stu-id="56635-308">This operation is functionally the same as a `move` operation without the final `remove` step.</span></span>

<span data-ttu-id="56635-309">Il documento di patch di esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="56635-309">The following sample patch document:</span></span>

* <span data-ttu-id="56635-310">Copia il valore di `Orders[0].OrderName` in `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="56635-310">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="56635-311">Inserisce una copia di `Orders[1]` prima di `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="56635-311">Inserts a copy of `Orders[1]` before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a><span data-ttu-id="56635-312">Operazione test</span><span class="sxs-lookup"><span data-stu-id="56635-312">The test operation</span></span>

<span data-ttu-id="56635-313">Se il valore nella posizione indicata da `path` è diverso dal valore fornito in `value`, la richiesta non riesce.</span><span class="sxs-lookup"><span data-stu-id="56635-313">If the value at the location indicated by `path` is different from the value provided in `value`, the request fails.</span></span> <span data-ttu-id="56635-314">In questo caso, l'intera richiesta PATCH non riesce anche se tutte le altre operazioni nel documento di patch potrebbero riuscire.</span><span class="sxs-lookup"><span data-stu-id="56635-314">In that case, the whole PATCH request fails even if all other operations in the patch document would otherwise succeed.</span></span>

<span data-ttu-id="56635-315">L'operazione `test` viene usata in genere per impedire un aggiornamento in caso di conflitto di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="56635-315">The `test` operation is commonly used to prevent an update when there's a concurrency conflict.</span></span>

<span data-ttu-id="56635-316">Il documento di patch di esempio seguente non ha alcun effetto se il valore iniziale di `CustomerName` è "John", perché il test non riesce:</span><span class="sxs-lookup"><span data-stu-id="56635-316">The following sample patch document has no effect if the initial value of `CustomerName` is "John", because the test fails:</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a><span data-ttu-id="56635-317">Ottenere il codice</span><span class="sxs-lookup"><span data-stu-id="56635-317">Get the code</span></span>

<span data-ttu-id="56635-318">[Visualizzare o scaricare il codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span><span class="sxs-lookup"><span data-stu-id="56635-318">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span></span> <span data-ttu-id="56635-319">([Come scaricare un esempio](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="56635-319">([How to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="56635-320">Per testare l'esempio, eseguire l'app e inviare richieste HTTP con le impostazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="56635-320">To test the sample, run the app and send HTTP requests with the following settings:</span></span>

* <span data-ttu-id="56635-321">URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span><span class="sxs-lookup"><span data-stu-id="56635-321">URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span></span>
* <span data-ttu-id="56635-322">Metodo HTTP: `PATCH`</span><span class="sxs-lookup"><span data-stu-id="56635-322">HTTP method: `PATCH`</span></span>
* <span data-ttu-id="56635-323">Intestazione: `Content-Type: application/json-patch+json`</span><span class="sxs-lookup"><span data-stu-id="56635-323">Header: `Content-Type: application/json-patch+json`</span></span>
* <span data-ttu-id="56635-324">Body: copiare e incollare uno degli esempi di documenti patch JSON dalla cartella del progetto *JSON* .</span><span class="sxs-lookup"><span data-stu-id="56635-324">Body: Copy and paste one of the JSON patch document samples from the *JSON* project folder.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="56635-325">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="56635-325">Additional resources</span></span>

* [<span data-ttu-id="56635-326">Specifica del metodo PATCH IETF RFC 5789</span><span class="sxs-lookup"><span data-stu-id="56635-326">IETF RFC 5789 PATCH method specification</span></span>](https://tools.ietf.org/html/rfc5789)
* [<span data-ttu-id="56635-327">Specifica JSON Patch IETF RFC 6902</span><span class="sxs-lookup"><span data-stu-id="56635-327">IETF RFC 6902 JSON Patch specification</span></span>](https://tools.ietf.org/html/rfc6902)
* [<span data-ttu-id="56635-328">Specifica del formato di percorso JSON Patch IETF RFC 6901</span><span class="sxs-lookup"><span data-stu-id="56635-328">IETF RFC 6901 JSON Patch path format spec</span></span>](https://tools.ietf.org/html/rfc6901)
* <span data-ttu-id="56635-329">[Documentazione di JSON Patch](https://jsonpatch.com/).</span><span class="sxs-lookup"><span data-stu-id="56635-329">[JSON Patch documentation](https://jsonpatch.com/).</span></span> <span data-ttu-id="56635-330">Include collegamenti a risorse per la creazione di documenti JSON Patch.</span><span class="sxs-lookup"><span data-stu-id="56635-330">Includes links to resources for creating JSON Patch documents.</span></span>
* [<span data-ttu-id="56635-331">Codice sorgente JSON Patch in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56635-331">ASP.NET Core JSON Patch source code</span></span>](https://github.com/dotnet/AspNetCore/tree/master/src/Features/JsonPatch/src)

::: moniker-end
