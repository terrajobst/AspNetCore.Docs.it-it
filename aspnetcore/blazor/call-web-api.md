---
title: Chiamare un'API Web da ASP.NET Core Blazor
author: guardrex
description: Informazioni su come chiamare un'API Web da un'app Blazor usando Helper JSON, inclusa la creazione di richieste di condivisione di risorse tra le origini (CORS).
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/call-web-api
ms.openlocfilehash: b5c57317005d0072410542bad322458b1cb3f5ee
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962719"
---
# <a name="call-a-web-api-from-aspnet-core-opno-locblazor"></a><span data-ttu-id="225eb-103">Chiamare un'API Web da ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="225eb-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="225eb-104">Di [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27)e [Juan de la Cruz](https://github.com/juandelacruz23)</span><span class="sxs-lookup"><span data-stu-id="225eb-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Juan De la Cruz](https://github.com/juandelacruz23)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="225eb-105"> le app webassembly chiamano API Web usando un servizio di `HttpClient` preconfigurato.</span><span class="sxs-lookup"><span data-stu-id="225eb-105"> WebAssembly apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="225eb-106">Comporre richieste, che possono includere opzioni [API di recupero](https://developer.mozilla.org/docs/Web/API/Fetch_API) JavaScript, usando Blazor Helper JSON o con <xref:System.Net.Http.HttpRequestMessage>.</span><span class="sxs-lookup"><span data-stu-id="225eb-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span>

Blazor<span data-ttu-id="225eb-107"> le app server chiamano API Web usando istanze di <xref:System.Net.Http.HttpClient> in genere create con <xref:System.Net.Http.IHttpClientFactory>.</span><span class="sxs-lookup"><span data-stu-id="225eb-107"> Server apps call web APIs using <xref:System.Net.Http.HttpClient> instances typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="225eb-108">Per ulteriori informazioni, vedere <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="225eb-108">For more information, see <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="225eb-109">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="225eb-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="225eb-110">Per Blazor esempi di webassembly, vedere i componenti seguenti nell'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="225eb-110">For Blazor WebAssembly examples, see the following components in the sample app:</span></span>

* <span data-ttu-id="225eb-111">Chiama API Web (*pages/CallWebAPI. Razor*)</span><span class="sxs-lookup"><span data-stu-id="225eb-111">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="225eb-112">Tester richieste HTTP (*Components/HTTPRequestTester. Razor*)</span><span class="sxs-lookup"><span data-stu-id="225eb-112">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="225eb-113">Helper HttpClient e JSON</span><span class="sxs-lookup"><span data-stu-id="225eb-113">HttpClient and JSON helpers</span></span>

<span data-ttu-id="225eb-114">Nelle app Blazor webassembly, [HttpClient](xref:fundamentals/http-requests) è disponibile come servizio preconfigurato per l'esecuzione delle richieste al server di origine.</span><span class="sxs-lookup"><span data-stu-id="225eb-114">In Blazor WebAssembly apps, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span> <span data-ttu-id="225eb-115">Per usare `HttpClient` Helper JSON, aggiungere un riferimento al pacchetto `Microsoft.AspNetCore.Blazor.HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="225eb-115">To use `HttpClient` JSON helpers, add a package reference to `Microsoft.AspNetCore.Blazor.HttpClient`.</span></span> <span data-ttu-id="225eb-116">gli helper `HttpClient` e JSON vengono usati anche per chiamare endpoint dell'API Web di terze parti.</span><span class="sxs-lookup"><span data-stu-id="225eb-116">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="225eb-117">`HttpClient` viene implementato usando l' [API fetch](https://developer.mozilla.org/docs/Web/API/Fetch_API) del browser ed è soggetto alle limitazioni, inclusa l'applicazione degli stessi criteri di origine.</span><span class="sxs-lookup"><span data-stu-id="225eb-117">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="225eb-118">L'indirizzo di base del client viene impostato sull'indirizzo del server di origine.</span><span class="sxs-lookup"><span data-stu-id="225eb-118">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="225eb-119">Inserire un'istanza `HttpClient` usando la direttiva `@inject`:</span><span class="sxs-lookup"><span data-stu-id="225eb-119">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="225eb-120">Negli esempi seguenti, un'API Web todo elabora le operazioni di creazione, lettura, aggiornamento ed eliminazione (CRUD).</span><span class="sxs-lookup"><span data-stu-id="225eb-120">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="225eb-121">Gli esempi sono basati su una classe `TodoItem` che archivia:</span><span class="sxs-lookup"><span data-stu-id="225eb-121">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="225eb-122">ID (`Id`, `long`) &ndash; ID univoco dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="225eb-122">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="225eb-123">Nome (`Name`, `string`) &ndash; nome dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="225eb-123">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="225eb-124">Status (`IsComplete`, `bool`) &ndash; indica se l'elemento todo è terminato.</span><span class="sxs-lookup"><span data-stu-id="225eb-124">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="225eb-125">I metodi helper JSON inviano richieste a un URI (un'API Web negli esempi seguenti) ed elaborano la risposta:</span><span class="sxs-lookup"><span data-stu-id="225eb-125">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="225eb-126">`GetJsonAsync` &ndash; invia una richiesta HTTP GET e analizza il corpo della risposta JSON per creare un oggetto.</span><span class="sxs-lookup"><span data-stu-id="225eb-126">`GetJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="225eb-127">Nel codice seguente, le `_todoItems` vengono visualizzate dal componente.</span><span class="sxs-lookup"><span data-stu-id="225eb-127">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="225eb-128">Il metodo `GetTodoItems` viene attivato quando il componente ha terminato il rendering ([OnInitializedAsync](xref:blazor/components#lifecycle-methods)).</span><span class="sxs-lookup"><span data-stu-id="225eb-128">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitializedAsync](xref:blazor/components#lifecycle-methods)).</span></span> <span data-ttu-id="225eb-129">Per un esempio completo, vedere l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="225eb-129">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* <span data-ttu-id="225eb-130">`PostJsonAsync` &ndash; invia una richiesta HTTP POST, incluso il contenuto con codifica JSON, e analizza il corpo della risposta JSON per creare un oggetto.</span><span class="sxs-lookup"><span data-stu-id="225eb-130">`PostJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="225eb-131">Nel codice seguente `_newItemName` viene fornito da un elemento associato del componente.</span><span class="sxs-lookup"><span data-stu-id="225eb-131">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="225eb-132">Il metodo `AddItem` viene attivato selezionando un elemento `<button>`.</span><span class="sxs-lookup"><span data-stu-id="225eb-132">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="225eb-133">Per un esempio completo, vedere l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="225eb-133">See the sample app for a complete example.</span></span>

  ```cshtml
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

* <span data-ttu-id="225eb-134">`PutJsonAsync` &ndash; invia una richiesta HTTP PUT, incluso il contenuto con codifica JSON.</span><span class="sxs-lookup"><span data-stu-id="225eb-134">`PutJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="225eb-135">Nel codice seguente `_editItem` valori per `Name` e `IsCompleted` vengono forniti dagli elementi associati del componente.</span><span class="sxs-lookup"><span data-stu-id="225eb-135">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="225eb-136">Il `Id` dell'elemento viene impostato quando l'elemento è selezionato in un'altra parte dell'interfaccia utente e viene chiamato `EditItem`.</span><span class="sxs-lookup"><span data-stu-id="225eb-136">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="225eb-137">Il metodo `SaveItem` viene attivato selezionando l'elemento Save `<button>`.</span><span class="sxs-lookup"><span data-stu-id="225eb-137">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="225eb-138">Per un esempio completo, vedere l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="225eb-138">See the sample app for a complete example.</span></span>

  ```cshtml
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

<span data-ttu-id="225eb-139"><xref:System.Net.Http> include metodi di estensione aggiuntivi per l'invio di richieste HTTP e la ricezione di risposte HTTP.</span><span class="sxs-lookup"><span data-stu-id="225eb-139"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="225eb-140">[HttpClient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) viene usato per inviare una richiesta HTTP DELETE a un'API Web.</span><span class="sxs-lookup"><span data-stu-id="225eb-140">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="225eb-141">Nel codice seguente, l'elemento Delete `<button>` chiama il metodo `DeleteItem`.</span><span class="sxs-lookup"><span data-stu-id="225eb-141">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="225eb-142">L'elemento `<input>` associato fornisce il `id` dell'elemento da eliminare.</span><span class="sxs-lookup"><span data-stu-id="225eb-142">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="225eb-143">Per un esempio completo, vedere l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="225eb-143">See the sample app for a complete example.</span></span>

```cshtml
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

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="225eb-144">Condivisione di risorse tra le origini (CORS)</span><span class="sxs-lookup"><span data-stu-id="225eb-144">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="225eb-145">La sicurezza del browser impedisce a una pagina Web di effettuare richieste a un dominio diverso da quello che ha servito la pagina Web.</span><span class="sxs-lookup"><span data-stu-id="225eb-145">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="225eb-146">Questa restrizione è detta *criterio della stessa origine*.</span><span class="sxs-lookup"><span data-stu-id="225eb-146">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="225eb-147">Il criterio della stessa origine impedisce a un sito dannoso di leggere dati sensibili da un altro sito.</span><span class="sxs-lookup"><span data-stu-id="225eb-147">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="225eb-148">Per eseguire richieste dal browser a un endpoint con un'origine diversa, l' *endpoint* deve abilitare la [condivisione di risorse tra le origini (CORS)](https://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="225eb-148">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="225eb-149">L'app di esempio illustra l'uso di CORS nel componente API Web Call (*pages/CallWebAPI. Razor*).</span><span class="sxs-lookup"><span data-stu-id="225eb-149">The sample app demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="225eb-150">Per consentire ad altri siti di effettuare richieste di condivisione risorse tra le origini (CORS) per l'app, vedere <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="225eb-150">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="225eb-151">HttpClient e HttpRequestMessage con le opzioni di richiesta dell'API fetch</span><span class="sxs-lookup"><span data-stu-id="225eb-151">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="225eb-152">Quando si esegue il webassembly in un'app Blazor webassembly, usare [HttpClient](xref:fundamentals/http-requests) e <xref:System.Net.Http.HttpRequestMessage> per personalizzare le richieste.</span><span class="sxs-lookup"><span data-stu-id="225eb-152">When running on WebAssembly in a Blazor WebAssembly app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="225eb-153">Ad esempio, è possibile specificare l'URI della richiesta, il metodo HTTP e tutte le intestazioni di richiesta desiderate.</span><span class="sxs-lookup"><span data-stu-id="225eb-153">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

<span data-ttu-id="225eb-154">Specificare le opzioni di richiesta per l' [API di recupero](https://developer.mozilla.org/docs/Web/API/Fetch_API) JavaScript sottostante usando la proprietà `WebAssemblyHttpMessageHandler.FetchArgs` della richiesta.</span><span class="sxs-lookup"><span data-stu-id="225eb-154">Supply request options to the underlying JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) using the `WebAssemblyHttpMessageHandler.FetchArgs` property on the request.</span></span> <span data-ttu-id="225eb-155">Come illustrato nell'esempio seguente, la proprietà `credentials` è impostata su uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="225eb-155">As shown in the following example, the `credentials` property is set to any of the following values:</span></span>

* <span data-ttu-id="225eb-156">`FetchCredentialsOption.Include` ("include") &ndash; indica al browser di inviare le credenziali (ad esempio, cookie o intestazioni di autenticazione HTTP) anche per le richieste tra origini.</span><span class="sxs-lookup"><span data-stu-id="225eb-156">`FetchCredentialsOption.Include` ("include") &ndash; Advises the browser to send credentials (such as cookies or HTTP authentication headers) even for cross-origin requests.</span></span> <span data-ttu-id="225eb-157">Consentito solo quando il criterio CORS è configurato per consentire le credenziali.</span><span class="sxs-lookup"><span data-stu-id="225eb-157">Only allowed when the CORS policy is configured to allow credentials.</span></span>
* <span data-ttu-id="225eb-158">`FetchCredentialsOption.Omit` ("omette") &ndash; indica al browser di non inviare mai le credenziali (ad esempio, cookie o intestazioni di autenticazione HTTP).</span><span class="sxs-lookup"><span data-stu-id="225eb-158">`FetchCredentialsOption.Omit` ("omit") &ndash; Advises the browser never to send credentials (such as cookies or HTTP auth headers).</span></span>
* <span data-ttu-id="225eb-159">`FetchCredentialsOption.SameOrigin` ("stessa origine") &ndash; indica al browser di inviare credenziali, ad esempio cookie o intestazioni di autenticazione HTTP, solo se l'URL di destinazione si trova nella stessa origine dell'applicazione chiamante.</span><span class="sxs-lookup"><span data-stu-id="225eb-159">`FetchCredentialsOption.SameOrigin` ("same-origin") &ndash; Advises the browser to send credentials (such as cookies or HTTP auth headers) only if the target URL is on the same origin as the calling application.</span></span>

```cshtml
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
        
        requestMessage.Properties[WebAssemblyHttpMessageHandler.FetchArgs] = new
        { 
            credentials = FetchCredentialsOption.Include
        };

        var response = await Http.SendAsync(requestMessage);
        var responseStatusCode = response.StatusCode;
        var responseBody = await response.Content.ReadAsStringAsync();
    }
}
```

<span data-ttu-id="225eb-160">Per altre informazioni sulle opzioni di recupero delle API, vedere la pagina relativa alla [documentazione Web MDN: WindowOrWorkerGlobalScope. fetch ():P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span><span class="sxs-lookup"><span data-stu-id="225eb-160">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="225eb-161">Quando si inviano le credenziali (cookie/intestazioni di autorizzazione) nelle richieste CORS, l'intestazione `Authorization` deve essere consentita dal criterio CORS.</span><span class="sxs-lookup"><span data-stu-id="225eb-161">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="225eb-162">Il criterio seguente include la configurazione per:</span><span class="sxs-lookup"><span data-stu-id="225eb-162">The following policy includes configuration for:</span></span>

* <span data-ttu-id="225eb-163">Origin della richiesta (`http://localhost:5000`, `https://localhost:5001`).</span><span class="sxs-lookup"><span data-stu-id="225eb-163">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="225eb-164">Qualsiasi metodo (verbo).</span><span class="sxs-lookup"><span data-stu-id="225eb-164">Any method (verb).</span></span>
* <span data-ttu-id="225eb-165">intestazioni `Content-Type` e `Authorization`.</span><span class="sxs-lookup"><span data-stu-id="225eb-165">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="225eb-166">Per consentire un'intestazione personalizzata (ad esempio, `x-custom-header`), elencare l'intestazione quando si chiama <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span><span class="sxs-lookup"><span data-stu-id="225eb-166">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="225eb-167">Credenziali impostate dal codice JavaScript lato client (la proprietà `credentials` è impostata su `include`).</span><span class="sxs-lookup"><span data-stu-id="225eb-167">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="225eb-168">Per altre informazioni, vedere <xref:security/cors> e il componente del tester di richiesta HTTP dell'app di esempio (*Components/HTTPRequestTester. Razor*).</span><span class="sxs-lookup"><span data-stu-id="225eb-168">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="225eb-169">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="225eb-169">Additional resources</span></span>

* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [<span data-ttu-id="225eb-170">Configurazione dell'endpoint HTTPS di gheppio</span><span class="sxs-lookup"><span data-stu-id="225eb-170">Kestrel HTTPS endpoint configuration</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [<span data-ttu-id="225eb-171">Condivisione di risorse tra le origini (CORS) in W3C</span><span class="sxs-lookup"><span data-stu-id="225eb-171">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
