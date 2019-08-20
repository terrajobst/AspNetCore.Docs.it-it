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
# <a name="call-a-web-api-from-aspnet-core-blazor"></a>Chiamare un'API Web da ASP.NET Core Blazer

Di [Luke Latham](https://github.com/guardrex) e [Daniel Roth](https://github.com/danroth27)

Le app sul lato client di Blazer chiamano API Web usando un `HttpClient` servizio preconfigurato. Comporre richieste, che possono includere le opzioni dell' [API di recupero](https://developer.mozilla.org/docs/Web/API/Fetch_API) JavaScript, usando Helper JSON Blazer <xref:System.Net.Http.HttpRequestMessage>o con.

Le app sul lato server di Blazer chiamano API <xref:System.Net.Http.HttpClient> Web usando le istanze <xref:System.Net.Http.IHttpClientFactory>di in genere create tramite. Per altre informazioni, vedere <xref:fundamentals/http-requests>.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))

Per esempi sul lato client di Blazer, vedere i componenti seguenti nell'app di esempio:

* Chiama API Web (*pages/CallWebAPI. Razor*)
* Tester richieste HTTP (*Components/HTTPRequestTester. Razor*)

## <a name="httpclient-and-json-helpers"></a>Helper HttpClient e JSON

Nelle app del lato client di Blazer, [HttpClient](xref:fundamentals/http-requests) è disponibile come servizio preconfigurato per l'esecuzione delle richieste al server di origine. Per usare `HttpClient` gli helper JSON, aggiungere un riferimento al pacchetto `Microsoft.AspNetCore.Blazor.HttpClient`a. `HttpClient`gli helper JSON e vengono usati anche per chiamare endpoint dell'API Web di terze parti. `HttpClient`viene implementato usando l' [API fetch](https://developer.mozilla.org/docs/Web/API/Fetch_API) del browser ed è soggetto alle limitazioni, inclusa l'applicazione degli stessi criteri di origine.

L'indirizzo di base del client viene impostato sull'indirizzo del server di origine. Inserire un' `HttpClient` istanza usando la `@inject` direttiva:

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

Negli esempi seguenti, un'API Web todo elabora le operazioni di creazione, lettura, aggiornamento ed eliminazione (CRUD). Gli esempi sono basati su una `TodoItem` classe che archivia:

* ID (`Id`, `long`) &ndash; ID univoco dell'elemento.
* Nome (`Name`, `string`) &ndash; nome dell'elemento.
* Status (`IsComplete`, `bool`) &ndash; indica se l'elemento todo è terminato.

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

I metodi helper JSON inviano richieste a un URI (un'API Web negli esempi seguenti) ed elaborano la risposta:

* `GetJsonAsync`&ndash; Invia una richiesta HTTP GET e analizza il corpo della risposta JSON per creare un oggetto.

  Nel codice seguente, l'oggetto `_todoItems` viene visualizzato dal componente. Il `GetTodoItems` metodo viene attivato al termine del rendering del componente ([OnInitializedAsync](xref:blazor/components#lifecycle-methods)). Per un esempio completo, vedere l'app di esempio.

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/todo");
  }
  ```

* `PostJsonAsync`&ndash; Invia una richiesta HTTP post, incluso il contenuto con codifica JSON, e analizza il corpo della risposta JSON per creare un oggetto.

  Nel codice seguente, `_newItemName` viene fornito da un elemento associato del componente. Il `AddItem` metodo viene attivato selezionando un `<button>` elemento. Per un esempio completo, vedere l'app di esempio.

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

* `PutJsonAsync`&ndash; Invia una richiesta HTTP PUT, incluso il contenuto con codifica JSON.

  Nel codice seguente, `_editItem` i valori per `Name` e `IsCompleted` vengono forniti dagli elementi associati del componente. L'elemento `Id` viene impostato quando l'elemento viene selezionato in un'altra parte dell'interfaccia utente e `EditItem` viene chiamato. Il `SaveItem` metodo viene attivato selezionando l'elemento Save `<button>` . Per un esempio completo, vedere l'app di esempio.

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

<xref:System.Net.Http>include metodi di estensione aggiuntivi per l'invio di richieste HTTP e la ricezione di risposte HTTP. [HttpClient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) viene usato per inviare una richiesta HTTP DELETE a un'API Web.

Nel codice seguente, l'elemento Delete `<button>` chiama il `DeleteItem` metodo. L'elemento `<input>` associato fornisce l' `id` oggetto dell'elemento da eliminare. Per un esempio completo, vedere l'app di esempio.

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

## <a name="cross-origin-resource-sharing-cors"></a>Condivisione di risorse tra le origini (CORS)

La sicurezza del browser impedisce a una pagina Web di effettuare richieste a un dominio diverso da quello che ha servito la pagina Web. Questa restrizione è detta *criterio della stessa origine*. Il criterio della stessa origine impedisce a un sito dannoso di leggere dati sensibili da un altro sito. Per eseguire richieste dal browser a un endpoint con un'origine diversa, l' *endpoint* deve abilitare la [condivisione di risorse tra le origini (CORS)](https://www.w3.org/TR/cors/).

L'app di esempio illustra l'uso di CORS nel componente API Web Call (*pages/CallWebAPI. Razor*).

Per consentire ad altri siti di effettuare richieste di condivisione di risorse tra le origini (CORS) per l' <xref:security/cors>app, vedere.

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a>HttpClient e HttpRequestMessage con le opzioni di richiesta dell'API fetch

Quando è in esecuzione su webassembly in un'app Blazer sul lato client , usare [HttpClient](xref:fundamentals/http-requests) e <xref:System.Net.Http.HttpRequestMessage> per personalizzare le richieste. Ad esempio, è possibile specificare l'URI della richiesta, il metodo HTTP e tutte le intestazioni di richiesta desiderate.

Specificare le opzioni di richiesta per l' [API di recupero](https://developer.mozilla.org/docs/Web/API/Fetch_API) JavaScript `WebAssemblyHttpMessageHandler.FetchArgs` sottostante usando la proprietà nella richiesta. Come illustrato nell'esempio seguente, la `credentials` proprietà viene impostata su uno dei valori seguenti:

* `FetchCredentialsOption.Include`("Includi") &ndash; Consiglia al browser di inviare credenziali, ad esempio cookie o intestazioni di autenticazione HTTP, anche per le richieste tra origini. Consentito solo quando il criterio CORS è configurato per consentire le credenziali.
* `FetchCredentialsOption.Omit`("omettere") &ndash; Informa il browser di non inviare mai le credenziali (ad esempio cookie o intestazioni di autenticazione HTTP).
* `FetchCredentialsOption.SameOrigin`("stessa origine") &ndash; Indica al browser di inviare credenziali, ad esempio cookie o intestazioni di autenticazione HTTP, solo se l'URL di destinazione si trova nella stessa origine dell'applicazione chiamante.

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

Per ulteriori informazioni sulle opzioni di recupero delle API [, vedere la pagina relativa alla documentazione Web MDN: WindowOrWorkerGlobalScope. fetch ():P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).

Quando si inviano le credenziali (cookie/intestazioni di autorizzazione) nelle `Authorization` richieste CORS, l'intestazione deve essere consentita dal criterio CORS.

Il criterio seguente include la configurazione per:

* Origin della richiesta`http://localhost:5000`( `https://localhost:5001`,).
* Qualsiasi metodo (verbo).
* `Content-Type`e `Authorization` intestazioni. Per consentire un'intestazione personalizzata (ad esempio, `x-custom-header`), elencare l'intestazione quando <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>si chiama.
* Credenziali impostate dal codice JavaScript lato client (`credentials` proprietà impostata su `include`).

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

Per ulteriori informazioni, vedere <xref:security/cors> e il componente tester richiesta HTTP dell'app di esempio (*Components/HTTPRequestTester. Razor*).

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/http-requests>
* [Condivisione di risorse tra le origini (CORS) in W3C](https://www.w3.org/TR/cors/)
