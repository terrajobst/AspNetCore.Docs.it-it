---
title: Chiamare i metodi .NET da funzioni JavaScript in ASP.NET Core Blazor
author: guardrex
description: Informazioni su come richiamare i metodi .NET dalle funzioni JavaScript nelle app Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/19/2020
no-loc:
- Blazor
- SignalR
uid: blazor/call-dotnet-from-javascript
ms.openlocfilehash: f4964341e261c65269eedafbbd6e676c1054f427
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659446"
---
# <a name="call-net-methods-from-javascript-functions-in-aspnet-core-opno-locblazor"></a><span data-ttu-id="c91d9-103">Chiamare i metodi .NET da funzioni JavaScript in ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="c91d9-103">Call .NET methods from JavaScript functions in ASP.NET Core Blazor</span></span>

<span data-ttu-id="c91d9-104">Di [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c91d9-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="c91d9-105">Un'app Blazor può richiamare funzioni JavaScript da metodi .NET e metodi .NET da funzioni JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c91d9-105">A Blazor app can invoke JavaScript functions from .NET methods and .NET methods from JavaScript functions.</span></span> <span data-ttu-id="c91d9-106">Questi scenari sono detti *interoperabilità JavaScript (interoperabilità* *JS*).</span><span class="sxs-lookup"><span data-stu-id="c91d9-106">These scenarios are called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="c91d9-107">Questo articolo descrive come richiamare i metodi .NET da JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c91d9-107">This article covers invoking .NET methods from JavaScript.</span></span> <span data-ttu-id="c91d9-108">Per informazioni su come chiamare funzioni JavaScript da .NET, vedere <xref:blazor/call-javascript-from-dotnet>.</span><span class="sxs-lookup"><span data-stu-id="c91d9-108">For information on how to call JavaScript functions from .NET, see <xref:blazor/call-javascript-from-dotnet>.</span></span>

<span data-ttu-id="c91d9-109">[Visualizzare o scaricare il codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c91d9-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="static-net-method-call"></a><span data-ttu-id="c91d9-110">Chiamata al metodo .NET statico</span><span class="sxs-lookup"><span data-stu-id="c91d9-110">Static .NET method call</span></span>

<span data-ttu-id="c91d9-111">Per richiamare un metodo .NET statico da JavaScript, usare le funzioni `DotNet.invokeMethod` o `DotNet.invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="c91d9-111">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="c91d9-112">Passare l'identificatore del metodo statico che si desidera chiamare, il nome dell'assembly che contiene la funzione e gli eventuali argomenti.</span><span class="sxs-lookup"><span data-stu-id="c91d9-112">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="c91d9-113">La versione asincrona è preferibile per supportare Blazor scenari server.</span><span class="sxs-lookup"><span data-stu-id="c91d9-113">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="c91d9-114">Il metodo .NET deve essere pubblico, statico e avere l'attributo `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="c91d9-114">The .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="c91d9-115">La chiamata ai metodi generici aperti non è attualmente supportata.</span><span class="sxs-lookup"><span data-stu-id="c91d9-115">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="c91d9-116">L'app di esempio include C# un metodo per restituire una matrice di `int`.</span><span class="sxs-lookup"><span data-stu-id="c91d9-116">The sample app includes a C# method to return an `int` array.</span></span> <span data-ttu-id="c91d9-117">L'attributo `JSInvokable` viene applicato al metodo.</span><span class="sxs-lookup"><span data-stu-id="c91d9-117">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="c91d9-118">*Pages/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="c91d9-118">*Pages/JsInterop.razor*:</span></span>

```razor
<button type="button" class="btn btn-primary"
        onclick="exampleJsFunctions.returnArrayAsyncJs()">
    Trigger .NET static method ReturnArrayAsync
</button>

@code {
    [JSInvokable]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

<span data-ttu-id="c91d9-119">JavaScript servito al client richiama il C# metodo .NET.</span><span class="sxs-lookup"><span data-stu-id="c91d9-119">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="c91d9-120">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="c91d9-120">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="c91d9-121">Quando si seleziona il pulsante **trigger .net static method ReturnArrayAsync** , esaminare l'output della console negli strumenti di sviluppo Web del browser.</span><span class="sxs-lookup"><span data-stu-id="c91d9-121">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="c91d9-122">L'output della console è:</span><span class="sxs-lookup"><span data-stu-id="c91d9-122">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="c91d9-123">Il quarto valore della matrice viene inserito nella matrice (`data.push(4);`) restituito da `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="c91d9-123">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

<span data-ttu-id="c91d9-124">Per impostazione predefinita, l'identificatore del metodo è il nome del metodo, ma è possibile specificare un identificatore diverso usando il costruttore `JSInvokableAttribute`:</span><span class="sxs-lookup"><span data-stu-id="c91d9-124">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor:</span></span>

```csharp
@code {
    [JSInvokable("DifferentMethodName")]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

<span data-ttu-id="c91d9-125">Nel file JavaScript sul lato client:</span><span class="sxs-lookup"><span data-stu-id="c91d9-125">In the client-side JavaScript file:</span></span>

```javascript
returnArrayAsyncJs: function () {
  DotNet.invokeMethodAsync('BlazorSample', 'DifferentMethodName')
    .then(data => {
      data.push(4);
      console.log(data);
    });
}
```

## <a name="instance-method-call"></a><span data-ttu-id="c91d9-126">Chiamata al metodo di istanza</span><span class="sxs-lookup"><span data-stu-id="c91d9-126">Instance method call</span></span>

<span data-ttu-id="c91d9-127">È anche possibile chiamare i metodi di istanza .NET da JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c91d9-127">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="c91d9-128">Per richiamare un metodo di istanza .NET da JavaScript:</span><span class="sxs-lookup"><span data-stu-id="c91d9-128">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="c91d9-129">Passa l'istanza .NET per riferimento a JavaScript:</span><span class="sxs-lookup"><span data-stu-id="c91d9-129">Pass the .NET instance by reference to JavaScript:</span></span>
  * <span data-ttu-id="c91d9-130">Eseguire una chiamata statica a `DotNetObjectReference.Create`.</span><span class="sxs-lookup"><span data-stu-id="c91d9-130">Make a static call to `DotNetObjectReference.Create`.</span></span>
  * <span data-ttu-id="c91d9-131">Eseguire il wrapping dell'istanza in un'istanza di `DotNetObjectReference` e chiamare `Create` nell'istanza di `DotNetObjectReference`.</span><span class="sxs-lookup"><span data-stu-id="c91d9-131">Wrap the instance in a `DotNetObjectReference` instance and call `Create` on the `DotNetObjectReference` instance.</span></span> <span data-ttu-id="c91d9-132">Eliminare gli oggetti `DotNetObjectReference` (un esempio viene visualizzato più avanti in questa sezione).</span><span class="sxs-lookup"><span data-stu-id="c91d9-132">Dispose of `DotNetObjectReference` objects (an example appears later in this section).</span></span>
* <span data-ttu-id="c91d9-133">Richiamare i metodi di istanza .NET sull'istanza utilizzando le funzioni `invokeMethod` o `invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="c91d9-133">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="c91d9-134">L'istanza di .NET può essere passata anche come argomento quando si richiamano altri metodi .NET da JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c91d9-134">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="c91d9-135">L'app di esempio consente di registrare i messaggi nella console lato client.</span><span class="sxs-lookup"><span data-stu-id="c91d9-135">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="c91d9-136">Per gli esempi seguenti illustrati dall'app di esempio, esaminare l'output della console del browser negli strumenti di sviluppo del browser.</span><span class="sxs-lookup"><span data-stu-id="c91d9-136">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="c91d9-137">Quando il pulsante del **metodo di istanza .NET del trigger HelloHelper. sayHello** è selezionato, `ExampleJsInterop.CallHelloHelperSayHello` viene chiamato e passa un nome, `Blazor`, al metodo.</span><span class="sxs-lookup"><span data-stu-id="c91d9-137">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="c91d9-138">*Pages/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="c91d9-138">*Pages/JsInterop.razor*:</span></span>

```razor
<button type="button" class="btn btn-primary" @onclick="TriggerNetInstanceMethod">
    Trigger .NET instance method HelloHelper.SayHello
</button>

@code {
    public async Task TriggerNetInstanceMethod()
    {
        var exampleJsInterop = new ExampleJsInterop(JSRuntime);
        await exampleJsInterop.CallHelloHelperSayHello("Blazor");
    }
}
```

<span data-ttu-id="c91d9-139">`CallHelloHelperSayHello` richiama la funzione JavaScript `sayHello` con una nuova istanza di `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="c91d9-139">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="c91d9-140">*JsInteropClasses/ExampleJsInterop. cs*:</span><span class="sxs-lookup"><span data-stu-id="c91d9-140">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="c91d9-141">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="c91d9-141">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="c91d9-142">Il nome viene passato al costruttore `HelloHelper`, che imposta la proprietà `HelloHelper.Name`.</span><span class="sxs-lookup"><span data-stu-id="c91d9-142">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="c91d9-143">Quando viene eseguita la funzione JavaScript `sayHello`, `HelloHelper.SayHello` restituisce il messaggio `Hello, {Name}!`, che viene scritto nella console dalla funzione JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c91d9-143">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="c91d9-144">*JsInteropClasses/HelloHelper. cs*:</span><span class="sxs-lookup"><span data-stu-id="c91d9-144">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="c91d9-145">Output della console negli strumenti di sviluppo Web del browser:</span><span class="sxs-lookup"><span data-stu-id="c91d9-145">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

<span data-ttu-id="c91d9-146">Per evitare una perdita di memoria e consentire Garbage Collection su un componente che crea un `DotNetObjectReference`, eliminare l'oggetto nella classe che ha creato l'istanza di `DotNetObjectReference`:</span><span class="sxs-lookup"><span data-stu-id="c91d9-146">To avoid a memory leak and allow garbage collection on a component that creates a `DotNetObjectReference`, dispose of the object in the class that created the `DotNetObjectReference` instance:</span></span>

```csharp
public class ExampleJsInterop : IDisposable
{
    private readonly IJSRuntime _jsRuntime;
    private DotNetObjectReference<HelloHelper> _objRef;

    public ExampleJsInterop(IJSRuntime jsRuntime)
    {
        _jsRuntime = jsRuntime;
    }

    public ValueTask<string> CallHelloHelperSayHello(string name)
    {
        _objRef = DotNetObjectReference.Create(new HelloHelper(name));

        return _jsRuntime.InvokeAsync<string>(
            "exampleJsFunctions.sayHello",
            _objRef);
    }

    public void Dispose()
    {
        _objRef?.Dispose();
    }
}
```
  
<span data-ttu-id="c91d9-147">Il modello precedente illustrato nella classe `ExampleJsInterop` può anche essere implementato in un componente:</span><span class="sxs-lookup"><span data-stu-id="c91d9-147">The preceding pattern shown in the `ExampleJsInterop` class can also be implemented in a component:</span></span>
  
```razor
@page "/JSInteropComponent"
@using BlazorSample.JsInteropClasses
@implements IDisposable
@inject IJSRuntime JSRuntime

<h1>JavaScript Interop</h1>

<button type="button" class="btn btn-primary" @onclick="TriggerNetInstanceMethod">
    Trigger .NET instance method HelloHelper.SayHello
</button>

@code {
    private DotNetObjectReference<HelloHelper> _objRef;

    public async Task TriggerNetInstanceMethod()
    {
        _objRef = DotNetObjectReference.Create(new HelloHelper("Blazor"));

        await JSRuntime.InvokeAsync<string>(
            "exampleJsFunctions.sayHello",
            _objRef);
    }

    public void Dispose()
    {
        _objRef?.Dispose();
    }
}
```

[!INCLUDE[Share interop code in a class library](~/includes/blazor-share-interop-code.md)]

## <a name="additional-resources"></a><span data-ttu-id="c91d9-148">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c91d9-148">Additional resources</span></span>

* <xref:blazor/call-javascript-from-dotnet>
* [<span data-ttu-id="c91d9-149">Esempio di InteropComponent. Razor (repository GitHub DotNet/AspNetCore, Branch versione 3,1)</span><span class="sxs-lookup"><span data-stu-id="c91d9-149">InteropComponent.razor example (dotnet/AspNetCore GitHub repository, 3.1 release branch)</span></span>](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
* <span data-ttu-id="c91d9-150">[Eseguire trasferimenti di dati di grandi dimensioni nelle app Blazor server](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)</span><span class="sxs-lookup"><span data-stu-id="c91d9-150">[Perform large data transfers in Blazor Server apps](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)</span></span>
