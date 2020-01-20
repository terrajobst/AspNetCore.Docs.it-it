---
title: Chiamare un'API Web da ASP.NET Core Blazor
author: guardrex
description: Informazioni su come chiamare un'API Web da un'app Blazor usando Helper JSON, inclusa la creazione di richieste di condivisione di risorse tra le origini (CORS).
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/call-web-api
ms.openlocfilehash: 66605f38a6fcaedebc92b0946dca1e5f28b593c6
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160067"
---
# <a name="call-a-web-api-from-aspnet-core-opno-locblazor"></a><span data-ttu-id="68cc5-103">Chiamare un'API Web da ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="68cc5-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="68cc5-104">Di [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27)e [Juan de la Cruz](https://github.com/juandelacruz23)</span><span class="sxs-lookup"><span data-stu-id="68cc5-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Juan De la Cruz](https://github.com/juandelacruz23)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="68cc5-105">[Blazor le app webassembly](xref:blazor/hosting-models#blazor-webassembly) chiamano API Web usando un servizio di `HttpClient` preconfigurato.</span><span class="sxs-lookup"><span data-stu-id="68cc5-105">[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="68cc5-106">Comporre richieste, che possono includere opzioni [API di recupero](https://developer.mozilla.org/docs/Web/API/Fetch_API) JavaScript, usando Blazor Helper JSON o con <xref:System.Net.Http.HttpRequestMessage>.</span><span class="sxs-lookup"><span data-stu-id="68cc5-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span>

<span data-ttu-id="68cc5-107">[Blazor](xref:blazor/hosting-models#blazor-server) le app server chiamano API Web usando istanze di <xref:System.Net.Http.HttpClient> in genere create con <xref:System.Net.Http.IHttpClientFactory>.</span><span class="sxs-lookup"><span data-stu-id="68cc5-107">[Blazor Server](xref:blazor/hosting-models#blazor-server) apps call web APIs using <xref:System.Net.Http.HttpClient> instances typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="68cc5-108">Per ulteriori informazioni, vedere <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="68cc5-108">For more information, see <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="68cc5-109">Consente di [visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([come scaricare](xref:index#how-to-download-a-sample)) &ndash; selezionare l'app *BlazorWebAssemblySample* .</span><span class="sxs-lookup"><span data-stu-id="68cc5-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample)) &ndash; Select the *BlazorWebAssemblySample* app.</span></span>

<span data-ttu-id="68cc5-110">Vedere i componenti seguenti nell'app di esempio *BlazorWebAssemblySample* :</span><span class="sxs-lookup"><span data-stu-id="68cc5-110">See the following components in the *BlazorWebAssemblySample* sample app:</span></span>

* <span data-ttu-id="68cc5-111">Chiama API Web (*pages/CallWebAPI. Razor*)</span><span class="sxs-lookup"><span data-stu-id="68cc5-111">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="68cc5-112">Tester richieste HTTP (*Components/HTTPRequestTester. Razor*)</span><span class="sxs-lookup"><span data-stu-id="68cc5-112">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="packages"></a><span data-ttu-id="68cc5-113">Pacchetti</span><span class="sxs-lookup"><span data-stu-id="68cc5-113">Packages</span></span>

<span data-ttu-id="68cc5-114">Fare riferimento all'oggetto *sperimentale* [Microsoft. AspNetCore.Blazor. ](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/)Pacchetto NuGet HttpClient nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="68cc5-114">Reference the *experimental* [Microsoft.AspNetCore.Blazor.HttpClient](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/) NuGet package in the project file.</span></span> <span data-ttu-id="68cc5-115">`Microsoft.AspNetCore.Blazor.HttpClient` si basa su `HttpClient` e [System. Text. JSON](https://www.nuget.org/packages/System.Text.Json/).</span><span class="sxs-lookup"><span data-stu-id="68cc5-115">`Microsoft.AspNetCore.Blazor.HttpClient` is based on `HttpClient` and [System.Text.Json](https://www.nuget.org/packages/System.Text.Json/).</span></span>

<span data-ttu-id="68cc5-116">Per usare un'API stabile, usare il pacchetto [Microsoft. AspNet. WebAPI. client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) , che usa [Newtonsoft. JSON](https://www.nuget.org/packages/Newtonsoft.Json/)/[JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="68cc5-116">To use a stable API, use the [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) package, which uses [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/)/[Json.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span> <span data-ttu-id="68cc5-117">L'uso dell'API stabile in `Microsoft.AspNet.WebApi.Client` non fornisce gli helper JSON descritti in questo argomento, che sono univoci per il pacchetto sperimentale di `Microsoft.AspNetCore.Blazor.HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="68cc5-117">Using the stable API in `Microsoft.AspNet.WebApi.Client` doesn't provide the JSON helpers described in this topic, which are unique to the experimental `Microsoft.AspNetCore.Blazor.HttpClient` package.</span></span>

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="68cc5-118">Helper HttpClient e JSON</span><span class="sxs-lookup"><span data-stu-id="68cc5-118">HttpClient and JSON helpers</span></span>

<span data-ttu-id="68cc5-119">In un'app Blazor webassembly, [HttpClient](xref:fundamentals/http-requests) è disponibile come servizio preconfigurato per l'esecuzione delle richieste al server di origine.</span><span class="sxs-lookup"><span data-stu-id="68cc5-119">In a Blazor WebAssembly app, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span>

<span data-ttu-id="68cc5-120">Per impostazione predefinita, un'app Server Blazor non include un servizio di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="68cc5-120">A Blazor Server app doesn't include an `HttpClient` service by default.</span></span> <span data-ttu-id="68cc5-121">Fornire un `HttpClient` all'app usando l' [infrastruttura di HttpClient Factory](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="68cc5-121">Provide an `HttpClient` to the app using the [HttpClient factory infrastructure](xref:fundamentals/http-requests).</span></span>

<span data-ttu-id="68cc5-122">gli helper `HttpClient` e JSON vengono usati anche per chiamare endpoint dell'API Web di terze parti.</span><span class="sxs-lookup"><span data-stu-id="68cc5-122">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="68cc5-123">`HttpClient` viene implementato usando l' [API fetch](https://developer.mozilla.org/docs/Web/API/Fetch_API) del browser ed è soggetto alle limitazioni, inclusa l'applicazione degli stessi criteri di origine.</span><span class="sxs-lookup"><span data-stu-id="68cc5-123">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="68cc5-124">L'indirizzo di base del client viene impostato sull'indirizzo del server di origine.</span><span class="sxs-lookup"><span data-stu-id="68cc5-124">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="68cc5-125">Inserire un'istanza di `HttpClient` usando la direttiva `@inject`:</span><span class="sxs-lookup"><span data-stu-id="68cc5-125">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```razor
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="68cc5-126">Negli esempi seguenti, un'API Web todo elabora le operazioni di creazione, lettura, aggiornamento ed eliminazione (CRUD).</span><span class="sxs-lookup"><span data-stu-id="68cc5-126">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="68cc5-127">Gli esempi sono basati su una classe `TodoItem` che archivia:</span><span class="sxs-lookup"><span data-stu-id="68cc5-127">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="68cc5-128">ID (`Id`, `long`) &ndash; ID univoco dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="68cc5-128">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="68cc5-129">Nome (`Name`, `string`) &ndash; nome dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="68cc5-129">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="68cc5-130">Stato (`IsComplete`, `bool`) &ndash; indicazione se l'elemento todo è terminato.</span><span class="sxs-lookup"><span data-stu-id="68cc5-130">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="68cc5-131">I metodi helper JSON inviano richieste a un URI (un'API Web negli esempi seguenti) ed elaborano la risposta:</span><span class="sxs-lookup"><span data-stu-id="68cc5-131">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="68cc5-132">`GetJsonAsync` &ndash; invia una richiesta HTTP GET e analizza il corpo della risposta JSON per creare un oggetto.</span><span class="sxs-lookup"><span data-stu-id="68cc5-132">`GetJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="68cc5-133">Nel codice seguente, le `_todoItems` vengono visualizzate dal componente.</span><span class="sxs-lookup"><span data-stu-id="68cc5-133">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="68cc5-134">Il metodo `GetTodoItems` viene attivato al termine del rendering del componente ([OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)).</span><span class="sxs-lookup"><span data-stu-id="68cc5-134">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)).</span></span> <span data-ttu-id="68cc5-135">Per un esempio completo, vedere l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="68cc5-135">See the sample app for a complete example.</span></span>

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* <span data-ttu-id="68cc5-136">`PostJsonAsync` &ndash; invia una richiesta HTTP POST, incluso il contenuto con codifica JSON, e analizza il corpo della risposta JSON per creare un oggetto.</span><span class="sxs-lookup"><span data-stu-id="68cc5-136">`PostJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="68cc5-137">Nel codice seguente `_newItemName` viene fornito da un elemento associato del componente.</span><span class="sxs-lookup"><span data-stu-id="68cc5-137">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="68cc5-138">Il metodo `AddItem` viene attivato selezionando un elemento di `<button>`.</span><span class="sxs-lookup"><span data-stu-id="68cc5-138">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="68cc5-139">Per un esempio completo, vedere l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="68cc5-139">See the sample app for a complete example.</span></span>

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  <input @bind="_newItemName" placeholder="New Todo Item" />
  <button @onclick="@AddItem">Add</button>

  @code {
      private string _newItemName;

      private async Task AddItem()
      {
          var addItem = new TodoItem { Name = _newItemName, IsComplete = false };
          await Http.PostJsonAsync("api/TodoItems", addItem);
      }
  }
  ```

* <span data-ttu-id="68cc5-140">`PutJsonAsync` &ndash; invia una richiesta HTTP PUT, incluso il contenuto con codifica JSON.</span><span class="sxs-lookup"><span data-stu-id="68cc5-140">`PutJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="68cc5-141">Nel codice seguente `_editItem` valori per `Name` e `IsCompleted` vengono forniti dagli elementi associati del componente.</span><span class="sxs-lookup"><span data-stu-id="68cc5-141">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="68cc5-142">Il `Id` dell'elemento viene impostato quando l'elemento viene selezionato in un'altra parte dell'interfaccia utente e viene chiamato `EditItem`.</span><span class="sxs-lookup"><span data-stu-id="68cc5-142">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="68cc5-143">Il metodo `SaveItem` viene attivato selezionando l'elemento Save `<button>`.</span><span class="sxs-lookup"><span data-stu-id="68cc5-143">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="68cc5-144">Per un esempio completo, vedere l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="68cc5-144">See the sample app for a complete example.</span></span>

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  <input type="checkbox" @bind="_editItem.IsComplete" />
  <input @bind="_editItem.Name" />
  <button @onclick="@SaveItem">Save</button>

  @code {
      private TodoItem _editItem = new TodoItem();

      private void EditItem(long id)
      {
          var editItem = _todoItems.Single(i => i.Id == id);
          _editItem = new TodoItem { Id = editItem.Id, Name = editItem.Name, 
              IsComplete = editItem.IsComplete };
      }

      private async Task SaveItem() =>
          await Http.PutJsonAsync($"api/TodoItems/{_editItem.Id}, _editItem);
  }
  ```

<span data-ttu-id="68cc5-145"><xref:System.Net.Http> include metodi di estensione aggiuntivi per l'invio di richieste HTTP e la ricezione di risposte HTTP.</span><span class="sxs-lookup"><span data-stu-id="68cc5-145"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="68cc5-146">[HttpClient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) viene usato per inviare una richiesta HTTP DELETE a un'API Web.</span><span class="sxs-lookup"><span data-stu-id="68cc5-146">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="68cc5-147">Nel codice seguente, l'elemento Delete `<button>` chiama il metodo `DeleteItem`.</span><span class="sxs-lookup"><span data-stu-id="68cc5-147">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="68cc5-148">L'elemento `<input>` associato fornisce il `id` dell'elemento da eliminare.</span><span class="sxs-lookup"><span data-stu-id="68cc5-148">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="68cc5-149">Per un esempio completo, vedere l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="68cc5-149">See the sample app for a complete example.</span></span>

```razor
@using System.Net.Http
@inject HttpClient Http

<input @bind="_id" />
<button @onclick="@DeleteItem">Delete</button>

@code {
    private long _id;

    private async Task DeleteItem() =>
        await Http.DeleteAsync($"api/TodoItems/{_id}");
}
```

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="68cc5-150">Condivisione di risorse tra le origini (CORS)</span><span class="sxs-lookup"><span data-stu-id="68cc5-150">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="68cc5-151">La sicurezza del browser impedisce a una pagina Web di effettuare richieste a un dominio diverso da quello che ha servito la pagina Web.</span><span class="sxs-lookup"><span data-stu-id="68cc5-151">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="68cc5-152">Questa restrizione è detta *criterio della stessa origine*.</span><span class="sxs-lookup"><span data-stu-id="68cc5-152">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="68cc5-153">Il criterio della stessa origine impedisce a un sito dannoso di leggere dati sensibili da un altro sito.</span><span class="sxs-lookup"><span data-stu-id="68cc5-153">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="68cc5-154">Per eseguire richieste dal browser a un endpoint con un'origine diversa, l' *endpoint* deve abilitare la [condivisione di risorse tra le origini (CORS)](https://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="68cc5-154">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="68cc5-155">L' [app di esempioBlazor webassembly (BlazorWebAssemblySample)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) illustra l'uso di CORS nel componente API Web Call (*pages/CallWebAPI. Razor*).</span><span class="sxs-lookup"><span data-stu-id="68cc5-155">The [Blazor WebAssembly sample app (BlazorWebAssemblySample)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="68cc5-156">Per consentire ad altri siti di effettuare richieste di condivisione risorse tra le origini (CORS) per l'app, vedere <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="68cc5-156">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="68cc5-157">HttpClient e HttpRequestMessage con le opzioni di richiesta dell'API fetch</span><span class="sxs-lookup"><span data-stu-id="68cc5-157">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="68cc5-158">Quando si esegue il webassembly in un'app Blazor webassembly, usare [HttpClient](xref:fundamentals/http-requests) e <xref:System.Net.Http.HttpRequestMessage> per personalizzare le richieste.</span><span class="sxs-lookup"><span data-stu-id="68cc5-158">When running on WebAssembly in a Blazor WebAssembly app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="68cc5-159">Ad esempio, è possibile specificare l'URI della richiesta, il metodo HTTP e tutte le intestazioni di richiesta desiderate.</span><span class="sxs-lookup"><span data-stu-id="68cc5-159">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

```razor
@using System.Net.Http
@using System.Net.Http.Headers
@inject HttpClient Http

@code {
    private async Task PostRequest()
    {
        Http.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", "{OAUTH TOKEN}");

        var requestMessage = new HttpRequestMessage()
        {
            Method = new HttpMethod("POST"),
            RequestUri = new Uri("https://localhost:10000/api/TodoItems"),
            Content = 
                new StringContent(
                    @"{""name"":""A New Todo Item"",""isComplete"":false}")
        };

        requestMessage.Content.Headers.ContentType = 
            new System.Net.Http.Headers.MediaTypeHeaderValue(
                "application/json");

        requestMessage.Content.Headers.TryAddWithoutValidation(
            "x-custom-header", "value");

        var response = await Http.SendAsync(requestMessage);
        var responseStatusCode = response.StatusCode;
        var responseBody = await response.Content.ReadAsStringAsync();
    }
}
```

<span data-ttu-id="68cc5-160">Per altre informazioni sulle opzioni di recupero delle API, vedere la pagina relativa alla [documentazione Web MDN: WindowOrWorkerGlobalScope. fetch ():P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span><span class="sxs-lookup"><span data-stu-id="68cc5-160">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="68cc5-161">Quando si inviano le credenziali (cookie/intestazioni di autorizzazione) nelle richieste CORS, l'intestazione `Authorization` deve essere consentita dal criterio CORS.</span><span class="sxs-lookup"><span data-stu-id="68cc5-161">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="68cc5-162">Il criterio seguente include la configurazione per:</span><span class="sxs-lookup"><span data-stu-id="68cc5-162">The following policy includes configuration for:</span></span>

* <span data-ttu-id="68cc5-163">Origin della richiesta (`http://localhost:5000`, `https://localhost:5001`).</span><span class="sxs-lookup"><span data-stu-id="68cc5-163">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="68cc5-164">Qualsiasi metodo (verbo).</span><span class="sxs-lookup"><span data-stu-id="68cc5-164">Any method (verb).</span></span>
* <span data-ttu-id="68cc5-165">intestazioni `Content-Type` e `Authorization`.</span><span class="sxs-lookup"><span data-stu-id="68cc5-165">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="68cc5-166">Per consentire un'intestazione personalizzata (ad esempio, `x-custom-header`), elencare l'intestazione quando si chiama <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span><span class="sxs-lookup"><span data-stu-id="68cc5-166">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="68cc5-167">Credenziali impostate dal codice JavaScript lato client (`credentials` proprietà impostata su `include`).</span><span class="sxs-lookup"><span data-stu-id="68cc5-167">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="68cc5-168">Per altre informazioni, vedere <xref:security/cors> e il componente del tester di richiesta HTTP dell'app di esempio (*Components/HTTPRequestTester. Razor*).</span><span class="sxs-lookup"><span data-stu-id="68cc5-168">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="68cc5-169">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="68cc5-169">Additional resources</span></span>

* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [<span data-ttu-id="68cc5-170">Configurazione dell'endpoint HTTPS di gheppio</span><span class="sxs-lookup"><span data-stu-id="68cc5-170">Kestrel HTTPS endpoint configuration</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [<span data-ttu-id="68cc5-171">Condivisione di risorse tra le origini (CORS) in W3C</span><span class="sxs-lookup"><span data-stu-id="68cc5-171">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
