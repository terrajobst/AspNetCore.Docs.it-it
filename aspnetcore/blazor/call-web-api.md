---
title: Chiamare un'API Web da ASP.NET Core Blazer
author: guardrex
description: Informazioni su come chiamare un'API Web da un'app Blazer usando Helper JSON, inclusa la creazione di richieste di condivisione di risorse tra le origini (CORS).
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/call-web-api
ms.openlocfilehash: 60ebd01bc07da22cd1dcd0b16297ee54c97867fc
ms.sourcegitcommit: f5f0ff65d4e2a961939762fb00e654491a2c772a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030384"
---
# <a name="call-a-web-api-from-aspnet-core-blazor"></a><span data-ttu-id="b9957-103">Chiamare un'API Web da ASP.NET Core Blazer</span><span class="sxs-lookup"><span data-stu-id="b9957-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="b9957-104">Di [Luke Latham](https://github.com/guardrex) e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="b9957-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="b9957-105">Le app sul lato client di Blazer chiamano API Web usando un `HttpClient` servizio preconfigurato.</span><span class="sxs-lookup"><span data-stu-id="b9957-105">Blazor client-side apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="b9957-106">Comporre richieste, che possono includere le opzioni dell' [API di recupero](https://developer.mozilla.org/docs/Web/API/Fetch_API) JavaScript, usando Helper JSON Blazer <xref:System.Net.Http.HttpRequestMessage>o con.</span><span class="sxs-lookup"><span data-stu-id="b9957-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span>

<span data-ttu-id="b9957-107">Le app sul lato server di Blazer chiamano API <xref:System.Net.Http.HttpClient> Web usando le istanze <xref:System.Net.Http.IHttpClientFactory>di in genere create tramite.</span><span class="sxs-lookup"><span data-stu-id="b9957-107">Blazor server-side apps call web APIs using <xref:System.Net.Http.HttpClient> instances typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="b9957-108">Per altre informazioni, vedere <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="b9957-108">For more information, see <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="b9957-109">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b9957-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b9957-110">Per esempi sul lato client di Blazer, vedere i componenti seguenti nell'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="b9957-110">For Blazor client-side examples, see the following components in the sample app:</span></span>

* <span data-ttu-id="b9957-111">Chiama API Web (*pages/CallWebAPI. Razor*)</span><span class="sxs-lookup"><span data-stu-id="b9957-111">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="b9957-112">Tester richieste HTTP (*Components/HTTPRequestTester. Razor*)</span><span class="sxs-lookup"><span data-stu-id="b9957-112">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="b9957-113">Helper HttpClient e JSON</span><span class="sxs-lookup"><span data-stu-id="b9957-113">HttpClient and JSON helpers</span></span>

<span data-ttu-id="b9957-114">Nelle app del lato client di Blazer, [HttpClient](xref:fundamentals/http-requests) è disponibile come servizio preconfigurato per l'esecuzione delle richieste al server di origine.</span><span class="sxs-lookup"><span data-stu-id="b9957-114">In Blazor client-side apps, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span> <span data-ttu-id="b9957-115">Per usare `HttpClient` gli helper JSON, aggiungere un riferimento al pacchetto `Microsoft.AspNetCore.Blazor.HttpClient`a.</span><span class="sxs-lookup"><span data-stu-id="b9957-115">To use `HttpClient` JSON helpers, add a package reference to `Microsoft.AspNetCore.Blazor.HttpClient`.</span></span> <span data-ttu-id="b9957-116">`HttpClient`gli helper JSON e vengono usati anche per chiamare endpoint dell'API Web di terze parti.</span><span class="sxs-lookup"><span data-stu-id="b9957-116">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="b9957-117">`HttpClient`viene implementato usando l' [API fetch](https://developer.mozilla.org/docs/Web/API/Fetch_API) del browser ed è soggetto alle limitazioni, inclusa l'applicazione degli stessi criteri di origine.</span><span class="sxs-lookup"><span data-stu-id="b9957-117">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="b9957-118">L'indirizzo di base del client viene impostato sull'indirizzo del server di origine.</span><span class="sxs-lookup"><span data-stu-id="b9957-118">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="b9957-119">Inserire un' `HttpClient` istanza usando la `@inject` direttiva:</span><span class="sxs-lookup"><span data-stu-id="b9957-119">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="b9957-120">Negli esempi seguenti, un'API Web todo elabora le operazioni di creazione, lettura, aggiornamento ed eliminazione (CRUD).</span><span class="sxs-lookup"><span data-stu-id="b9957-120">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="b9957-121">Gli esempi sono basati su una `TodoItem` classe che archivia:</span><span class="sxs-lookup"><span data-stu-id="b9957-121">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="b9957-122">ID (`Id`, `long`) &ndash; ID univoco dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="b9957-122">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="b9957-123">Nome (`Name`, `string`) &ndash; nome dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="b9957-123">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="b9957-124">Status (`IsComplete`, `bool`) &ndash; indica se l'elemento todo è terminato.</span><span class="sxs-lookup"><span data-stu-id="b9957-124">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="b9957-125">I metodi helper JSON inviano richieste a un URI (un'API Web negli esempi seguenti) ed elaborano la risposta:</span><span class="sxs-lookup"><span data-stu-id="b9957-125">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="b9957-126">`GetJsonAsync`&ndash; Invia una richiesta HTTP GET e analizza il corpo della risposta JSON per creare un oggetto.</span><span class="sxs-lookup"><span data-stu-id="b9957-126">`GetJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="b9957-127">Nel codice seguente, l'oggetto `_todoItems` viene visualizzato dal componente.</span><span class="sxs-lookup"><span data-stu-id="b9957-127">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="b9957-128">Il `GetTodoItems` metodo viene attivato al termine del rendering del componente ([OnInitializedAsync](xref:blazor/components#lifecycle-methods)).</span><span class="sxs-lookup"><span data-stu-id="b9957-128">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitializedAsync](xref:blazor/components#lifecycle-methods)).</span></span> <span data-ttu-id="b9957-129">Per un esempio completo, vedere l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="b9957-129">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/todo");
  }
  ```

* <span data-ttu-id="b9957-130">`PostJsonAsync`&ndash; Invia una richiesta HTTP post, incluso il contenuto con codifica JSON, e analizza il corpo della risposta JSON per creare un oggetto.</span><span class="sxs-lookup"><span data-stu-id="b9957-130">`PostJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="b9957-131">Nel codice seguente, `_newItemName` viene fornito da un elemento associato del componente.</span><span class="sxs-lookup"><span data-stu-id="b9957-131">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="b9957-132">Il `AddItem` metodo viene attivato selezionando un `<button>` elemento.</span><span class="sxs-lookup"><span data-stu-id="b9957-132">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="b9957-133">Per un esempio completo, vedere l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="b9957-133">See the sample app for a complete example.</span></span>

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
          await Http.PostJsonAsync("api/todo", addItem);
      }
  }
  ```

* <span data-ttu-id="b9957-134">`PutJsonAsync`&ndash; Invia una richiesta HTTP PUT, incluso il contenuto con codifica JSON.</span><span class="sxs-lookup"><span data-stu-id="b9957-134">`PutJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="b9957-135">Nel codice seguente, `_editItem` i valori per `Name` e `IsCompleted` vengono forniti dagli elementi associati del componente.</span><span class="sxs-lookup"><span data-stu-id="b9957-135">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="b9957-136">L'elemento `Id` viene impostato quando l'elemento viene selezionato in un'altra parte dell'interfaccia utente e `EditItem` viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="b9957-136">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="b9957-137">Il `SaveItem` metodo viene attivato selezionando l'elemento Save `<button>` .</span><span class="sxs-lookup"><span data-stu-id="b9957-137">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="b9957-138">Per un esempio completo, vedere l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="b9957-138">See the sample app for a complete example.</span></span>

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
          await Http.PutJsonAsync($"api/todo/{_editItem.Id}, _editItem);
  }
  ```

<span data-ttu-id="b9957-139"><xref:System.Net.Http>include metodi di estensione aggiuntivi per l'invio di richieste HTTP e la ricezione di risposte HTTP.</span><span class="sxs-lookup"><span data-stu-id="b9957-139"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="b9957-140">[HttpClient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) viene usato per inviare una richiesta HTTP DELETE a un'API Web.</span><span class="sxs-lookup"><span data-stu-id="b9957-140">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="b9957-141">Nel codice seguente, l'elemento Delete `<button>` chiama il `DeleteItem` metodo.</span><span class="sxs-lookup"><span data-stu-id="b9957-141">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="b9957-142">L'elemento `<input>` associato fornisce l' `id` oggetto dell'elemento da eliminare.</span><span class="sxs-lookup"><span data-stu-id="b9957-142">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="b9957-143">Per un esempio completo, vedere l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="b9957-143">See the sample app for a complete example.</span></span>

```cshtml
@using System.Net.Http
@inject HttpClient Http

<input @bind="_id" />
<button @onclick="@DeleteItem">Delete</button>

@code {
    private long _id;

    private async Task DeleteItem() =>
        await Http.DeleteAsync($"api/todo/{_id}");
}
```

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="b9957-144">Condivisione di risorse tra le origini (CORS)</span><span class="sxs-lookup"><span data-stu-id="b9957-144">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="b9957-145">La sicurezza del browser impedisce a una pagina Web di effettuare richieste a un dominio diverso da quello che ha servito la pagina Web.</span><span class="sxs-lookup"><span data-stu-id="b9957-145">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="b9957-146">Questa restrizione è detta *criterio della stessa origine*.</span><span class="sxs-lookup"><span data-stu-id="b9957-146">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="b9957-147">Il criterio della stessa origine impedisce a un sito dannoso di leggere dati sensibili da un altro sito.</span><span class="sxs-lookup"><span data-stu-id="b9957-147">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="b9957-148">Per eseguire richieste dal browser a un endpoint con un'origine diversa, l' *endpoint* deve abilitare la [condivisione di risorse tra le origini (CORS)](https://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="b9957-148">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="b9957-149">L'app di esempio illustra l'uso di CORS nel componente API Web Call (*pages/CallWebAPI. Razor*).</span><span class="sxs-lookup"><span data-stu-id="b9957-149">The sample app demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="b9957-150">Per consentire ad altri siti di effettuare richieste di condivisione di risorse tra le origini (CORS) per l' <xref:security/cors>app, vedere.</span><span class="sxs-lookup"><span data-stu-id="b9957-150">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="b9957-151">HttpClient e HttpRequestMessage con le opzioni di richiesta dell'API fetch</span><span class="sxs-lookup"><span data-stu-id="b9957-151">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="b9957-152">Quando è in esecuzione su webassembly in un'app Blazer sul lato client , usare [HttpClient](xref:fundamentals/http-requests) e <xref:System.Net.Http.HttpRequestMessage> per personalizzare le richieste.</span><span class="sxs-lookup"><span data-stu-id="b9957-152">When running on WebAssembly in a Blazor client-side app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="b9957-153">Ad esempio, è possibile specificare l'URI della richiesta, il metodo HTTP e tutte le intestazioni di richiesta desiderate.</span><span class="sxs-lookup"><span data-stu-id="b9957-153">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

<span data-ttu-id="b9957-154">Specificare le opzioni di richiesta per l' [API di recupero](https://developer.mozilla.org/docs/Web/API/Fetch_API) JavaScript `WebAssemblyHttpMessageHandler.FetchArgs` sottostante usando la proprietà nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="b9957-154">Supply request options to the underlying JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) using the `WebAssemblyHttpMessageHandler.FetchArgs` property on the request.</span></span> <span data-ttu-id="b9957-155">Come illustrato nell'esempio seguente, la `credentials` proprietà viene impostata su uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="b9957-155">As shown in the following example, the `credentials` property is set to any of the following values:</span></span>

* <span data-ttu-id="b9957-156">`FetchCredentialsOption.Include`("Includi") &ndash; Consiglia al browser di inviare credenziali, ad esempio cookie o intestazioni di autenticazione HTTP, anche per le richieste tra origini.</span><span class="sxs-lookup"><span data-stu-id="b9957-156">`FetchCredentialsOption.Include` ("include") &ndash; Advises the browser to send credentials (such as cookies or HTTP authentication headers) even for cross-origin requests.</span></span> <span data-ttu-id="b9957-157">Consentito solo quando il criterio CORS è configurato per consentire le credenziali.</span><span class="sxs-lookup"><span data-stu-id="b9957-157">Only allowed when the CORS policy is configured to allow credentials.</span></span>
* <span data-ttu-id="b9957-158">`FetchCredentialsOption.Omit`("omettere") &ndash; Informa il browser di non inviare mai le credenziali (ad esempio cookie o intestazioni di autenticazione HTTP).</span><span class="sxs-lookup"><span data-stu-id="b9957-158">`FetchCredentialsOption.Omit` ("omit") &ndash; Advises the browser never to send credentials (such as cookies or HTTP auth headers).</span></span>
* <span data-ttu-id="b9957-159">`FetchCredentialsOption.SameOrigin`("stessa origine") &ndash; Indica al browser di inviare credenziali, ad esempio cookie o intestazioni di autenticazione HTTP, solo se l'URL di destinazione si trova nella stessa origine dell'applicazione chiamante.</span><span class="sxs-lookup"><span data-stu-id="b9957-159">`FetchCredentialsOption.SameOrigin` ("same-origin") &ndash; Advises the browser to send credentials (such as cookies or HTTP auth headers) only if the target URL is on the same origin as the calling application.</span></span>

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
            RequestUri = new Uri("https://localhost:10000/api/todo"),
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

<span data-ttu-id="b9957-160">Per ulteriori informazioni sulle opzioni di recupero delle API [, vedere la pagina relativa alla documentazione Web MDN: WindowOrWorkerGlobalScope. fetch ():P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span><span class="sxs-lookup"><span data-stu-id="b9957-160">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="b9957-161">Quando si inviano le credenziali (cookie/intestazioni di autorizzazione) nelle `Authorization` richieste CORS, l'intestazione deve essere consentita dal criterio CORS.</span><span class="sxs-lookup"><span data-stu-id="b9957-161">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="b9957-162">Il criterio seguente include la configurazione per:</span><span class="sxs-lookup"><span data-stu-id="b9957-162">The following policy includes configuration for:</span></span>

* <span data-ttu-id="b9957-163">Origin della richiesta`http://localhost:5000`( `https://localhost:5001`,).</span><span class="sxs-lookup"><span data-stu-id="b9957-163">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="b9957-164">Qualsiasi metodo (verbo).</span><span class="sxs-lookup"><span data-stu-id="b9957-164">Any method (verb).</span></span>
* <span data-ttu-id="b9957-165">`Content-Type`e `Authorization` intestazioni.</span><span class="sxs-lookup"><span data-stu-id="b9957-165">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="b9957-166">Per consentire un'intestazione personalizzata (ad esempio, `x-custom-header`), elencare l'intestazione quando <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>si chiama.</span><span class="sxs-lookup"><span data-stu-id="b9957-166">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="b9957-167">Credenziali impostate dal codice JavaScript lato client (`credentials` proprietà impostata su `include`).</span><span class="sxs-lookup"><span data-stu-id="b9957-167">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="b9957-168">Per ulteriori informazioni, vedere <xref:security/cors> e il componente tester richiesta HTTP dell'app di esempio (*Components/HTTPRequestTester. Razor*).</span><span class="sxs-lookup"><span data-stu-id="b9957-168">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b9957-169">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b9957-169">Additional resources</span></span>

* <xref:fundamentals/http-requests>
* [<span data-ttu-id="b9957-170">Condivisione di risorse tra le origini (CORS) in W3C</span><span class="sxs-lookup"><span data-stu-id="b9957-170">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
