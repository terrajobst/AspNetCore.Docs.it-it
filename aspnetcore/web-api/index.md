---
title: Creare API Web con ASP.NET Core
author: scottaddie
description: Informazioni di base sulla creazione di un'API Web in ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 05/07/2019
uid: web-api/index
ms.openlocfilehash: 593fd33babc81cddfc4db2150a37e5ec3bc1a0be
ms.sourcegitcommit: a3926eae3f687013027a2828830c12a89add701f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2019
ms.locfileid: "65450844"
---
# <a name="create-web-apis-with-aspnet-core"></a>Creare API Web con ASP.NET Core

Di [Scott Addie](https://github.com/scottaddie) e [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core supporta la creazione di servizi RESTful, noti anche come API Web, con C#. Per gestire le richieste, un'API Web usa i controller. I *controller* in un'API Web sono classi che derivano da `ControllerBase`. Questo articolo illustra come usare i controller per gestire le richieste API.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples). ([Come scaricare un esempio](xref:index#how-to-download-a-sample)).

## <a name="controllerbase-class"></a>Classe ControllerBase

Un'API Web include una o più classi controller che derivano da <xref:Microsoft.AspNetCore.Mvc.ControllerBase>. Ad esempio, il modello di progetto API Web crea un controller Values:

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=3)]

Non creare un controller API Web tramite derivazione dalla classe di base <xref:Microsoft.AspNetCore.Mvc.Controller>. La classe `Controller` deriva da `ControllerBase` e aggiunge il supporto per le visualizzazioni, pertanto è progettata per la gestione delle pagine Web e non per le richieste di API Web.  Esiste un'eccezione a questa regola: quando si prevede di usare lo stesso controller sia per le API che per le visualizzazioni, derivarlo da `Controller`.

La classe `ControllerBase` offre molti metodi e proprietà utili per la gestione delle richieste HTTP. Ad esempio, `ControllerBase.CreatedAtAction` restituisce un codice di stato 201:

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=8-9)]

 Di seguito sono elencati alcuni esempi di metodi forniti da `ControllerBase`.

|Metodo  |Note  |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>| Restituisce il codice di stato 400.|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> |Restituisce il codice di stato 404.|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile*>|Restituisce un file.|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>|Richiama l'[associazione di modelli](xref:mvc/models/model-binding).|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel*>|Richiama la [convalida dei modelli](xref:mvc/models/validation).|

Per un elenco di tutti i metodi e proprietà disponibili, vedere <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.

## <a name="attributes"></a>Attributi

Lo spazio dei nomi <xref:Microsoft.AspNetCore.Mvc> include attributi che possono essere usati per configurare il comportamento del controller di API Web e i metodi di azione. L'esempio seguente usa gli attributi per specificare il metodo HTTP accettato e i codici di stato restituiti:

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

Di seguito sono riportati altri esempi degli attributi disponibili.

|Attributo|Note|
|---------|-----|
|[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)      |Specifica il modello di URL per un controller o un'azione.|
|[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)        |Specifica il prefisso e le proprietà da includere per l'associazione di modelli.|
|[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)  |Identifica un'azione che supporta il metodo HTTP GET.|
|[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)|Specifica i tipi di dati accettati da un'azione.|
|[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)|Specifica i tipi di dati restituiti da un'azione.|

Per un elenco che include gli attributi disponibili, vedere lo spazio dei nomi <xref:Microsoft.AspNetCore.Mvc>.

## <a name="apicontroller-attribute"></a>Attributo ApiController

L'attributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) può essere applicato a una classe di controller per abilitare i comportamenti specifici dell'API:

* [Requisiti del routing degli attributi](#attribute-routing-requirement)
* [Risposte HTTP 400 automatiche](#automatic-http-400-responses)
* [Inferenza del parametro di origine di associazione](#binding-source-parameter-inference)
* [Inferenza di richieste multipart/form-data](#multipartform-data-request-inference)
* [Dettagli del problema per i codici di stato di errore](#problem-details-for-error-status-codes)

Queste funzionalità richiedono la [versione di compatibilità](<xref:mvc/compatibility-version>) 2.1 o successiva.

### <a name="apicontroller-on-specific-controllers"></a>ApiController per controller specifici

L'attributo `[ApiController]` può essere applicato a controller specifici, come illustrato nell'esempio seguente dal modello di progetto:

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=2)]

### <a name="apicontroller-on-multiple-controllers"></a>ApiController per più controller

Un approccio per usare l'attributo su più di un controller consiste nel creare una classe di controller di base personalizzata annotata con l'attributo `[ApiController]`. Ecco un esempio che illustra una classe di base personalizzata e un controller che deriva da tale classe:

[!code-csharp[](index/samples/2.x/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_Inherit)]

### <a name="apicontroller-on-an-assembly"></a>ApiController per un assembly

Se la [versione di compatibilità](<xref:mvc/compatibility-version>) è impostata su 2.2 o una versione successiva, l'attributo `[ApiController]` può essere applicato a un assembly. L'uso delle annotazioni in questo modo applica il comportamento delle API Web a tutti i controller nell'assembly. Non esiste alcun modo per rifiutare esplicitamente singoli controller. Applicare l'attributo a livello di assembly alla classe `Startup`, come illustrato nell'esempio seguente:

```csharp
[assembly: ApiController]
namespace WebApiSample
{
    public class Startup
    {
        ...
    }
}
```

## <a name="attribute-routing-requirement"></a>Requisiti del routing degli attributi

Con l'attributo `ApiController` il routing degli attributi è un requisito. Ad esempio:

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=1)]

Le azioni non sono accessibili tramite le [route convenzionali](xref:mvc/controllers/routing#conventional-routing) definite da <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> o <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.

## <a name="automatic-http-400-responses"></a>Risposte HTTP 400 automatiche

Con l'attributo `ApiController` gli errori di convalida del modello attivano automaticamente una risposta HTTP 400. Di conseguenza è necessario il codice seguente in un metodo di azione:

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

### <a name="default-badrequest-response"></a>Risposta BadRequest predefinita 

Con la versione di compatibilità 2.2 o successiva, il tipo di risposta predefinito per le risposte HTTP 400 è <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>. Il tipo `ValidationProblemDetails` è conforme alla [specifica RFC 7807](https://tools.ietf.org/html/rfc7807).

Per modificare la risposta predefinita per <xref:Microsoft.AspNetCore.Mvc.SerializableError>, impostare la proprietà `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` su `true` in `Startup.ConfigureServices`, come illustrato nell'esempio seguente:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,9)]

### <a name="customize-badrequest-response"></a>Personalizzare la risposta BadRequest

Per personalizzare la risposta risultante da un errore di convalida, usare <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>. Aggiungere il codice evidenziato seguente dopo `services.AddMvc().SetCompatibilityVersion`:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureBadRequestResponse&highlight=3-20)]

### <a name="log-automatic-400-responses"></a>Registrare le risposte 400 automatiche

Vedere [How to log automatic 400 responses on model validation errors (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157) (Come registrare le risposte 400 automatiche negli errori di convalida del modello - aspnet/AspNetCore.Docs 12157).

### <a name="disable-automatic-400"></a>Disabilitare il comportamento automatico per gli errori 400

Per disabilitare il comportamento automatico per gli errori 400, impostare la proprietà <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> su `true`. Aggiungere il codice evidenziato seguente in `Startup.ConfigureServices` dopo `services.AddMvc().SetCompatibilityVersion`:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

## <a name="binding-source-parameter-inference"></a>Inferenza del parametro di origine di associazione

Un attributo di origine di associazione definisce la posizione in cui viene trovato il valore del parametro di un'azione. Esistono gli attributi di origine di associazione seguente:

|Attributo|Origine di associazione |
|---------|---------|
|[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)     | Corpo della richiesta |
|[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)     | Dati di modulo nel corpo della richiesta |
|[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) | Intestazione della richiesta |
|[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)   | Parametri della stringa di query della richiesta |
|[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)   | Dati della route della richiesta corrente |
|[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) | Il servizio richiesta inserito come parametro di azione |

> [!WARNING]
> Non usare `[FromRoute]` quando i valori potrebbero contenere `%2f` (vale a dire `/`). `%2f` non sarà convertito in `/` rimuovendo i caratteri di escape. Usare `[FromQuery]` se il valore potrebbe contenere `%2f`.

Senza l'attributo `[ApiController]` o altri attributi di origine di associazione come `[FromQuery]`, il runtime di ASP.NET Core tenta di usare lo strumento di associazione di modelli a oggetti complesso. Lo strumento di associazione di modelli a oggetti complesso estrae i dati dal provider di valori in un ordine definito.

Nell'esempio seguente, l'attributo `[FromQuery]` indica che il valore del parametro `discontinuedOnly` è specificato nella stringa di query dell'URL della richiesta:

[!code-csharp[](index/samples/2.x/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

L'attributo `[ApiController]` applica le regole di inferenza per le origini dati predefinite dei parametri di azione. Queste regole consentono di evitare di dover identificare le origini di associazione manualmente applicando attributi ai parametri di azione. Il comportamento delle regole di inferenza per le origini di associazione è il seguente:

* `[FromBody]` viene dedotto per i parametri di tipo complesso. Un'eccezione alla regola di inferenza `[FromBody]` è costituita dai tipi predefiniti complessi con un significato speciale, ad esempio <xref:Microsoft.AspNetCore.Http.IFormCollection> e <xref:System.Threading.CancellationToken>. Il codice di inferenza di origine di associazione ignora tali tipi speciali. 
* `[FromForm]` viene dedotto per i parametri di azione di tipo <xref:Microsoft.AspNetCore.Http.IFormFile> e <xref:Microsoft.AspNetCore.Http.IFormFileCollection>. Non viene dedotto per i tipi semplici o definiti dall'utente.
* `[FromRoute]` viene dedotto per i nomi di parametro di azione corrispondenti a un parametro nel modello di route. Quando più di una route corrisponde a un parametro di azione, tutti i valori di route vengono considerati `[FromRoute]`.
* `[FromQuery]` viene dedotto per tutti gli altri parametri di azione.

### <a name="frombody-inference-notes"></a>Note sulla regola di inferenza FromBody

`[FromBody]` non viene dedotto per i tipi semplici, ad esempio `string` o `int`. Pertanto, l'attributo `[FromBody]` deve essere usato per i tipi semplici se è necessaria tale funzionalità.

Quando a un'azione sono associati più parametri dal corpo della richiesta, viene generata un'eccezione. Tutte le firme di metodo di azione seguenti, ad esempio, causano un'eccezione:

* `[FromBody]` dedotto per entrambi, perché si tratta di tipi complessi.

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* `[FromBody]` attributo per uno, dedotto per l'altro perché è un tipo complesso.

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* `[FromBody]` attributo per entrambi.

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

> [!NOTE]
> In ASP.NET Core 2.1, i parametri di tipo raccolta, come elenchi e matrici, vengono dedotti in modo errato come `[FromQuery]`. È necessario usare l'attributo `[FromBody]` per questi parametri, se devono essere associati dal corpo della richiesta. Questo comportamento è stato corretto in ASP.NET Core 2.2 o versioni successive, in cui i parametri di tipo raccolta vengono dedotti come associati dal corpo per impostazione predefinita.

### <a name="disable-inference-rules"></a>Disabilitare le regole di inferenza

Per disabilitare l'inferenza delle origini di associazione, impostare <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> su `true`. Aggiungere il codice seguente in `Startup.ConfigureServices` dopo `services.AddMvc().SetCompatibilityVersion`:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

## <a name="multipartform-data-request-inference"></a>Inferenza di richieste multipart/form-data

L'attributo `[ApiController]` applica una regola di inferenza quando un parametro di azione è annotato con l'attributo [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute): viene dedotto il tipo di contenuto della richiesta `multipart/form-data`.

Per disabilitare il comportamento predefinito, impostare <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> su `true` in `Startup.ConfigureServices`, come illustrato nell'esempio seguente:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

## <a name="problem-details-for-error-status-codes"></a>Dettagli del problema per i codici di stato di errore

Con la versione di compatibilità 2.2 o successiva, MVC consente di trasformare un risultato di errore (un risultato con codice di stato 400 o superiore) in un risultato con <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>. Il tipo `ProblemDetails` si basa sulla [specifica RFC 7807](https://tools.ietf.org/html/rfc7807) per fornire dettagli sull'errore leggibili dal computer in una risposta HTTP.

Si consideri il codice seguente in un'azione del controller:

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

La risposta HTTP per `NotFound` ha un codice di stato 404 con corpo `ProblemDetails`. Ad esempio:

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="customize-problemdetails-response"></a>Personalizzare la risposta ProblemDetails

Usare la proprietà `ClientErrorMapping` per configurare il contenuto della risposta `ProblemDetails`. Ad esempio, il codice seguente aggiorna la proprietà `type` per le risposte 404:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

### <a name="disable-problemdetails-response"></a>Disabilitare la risposta ProblemDetails

La creazione automatica di `ProblemDetails` è disabilitata quando la proprietà `SuppressMapClientErrors` è impostata su `true`. Aggiungere il codice seguente a `Startup.ConfigureServices`:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

## <a name="additional-resources"></a>Risorse aggiuntive 

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
