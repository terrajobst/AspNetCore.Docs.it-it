---
title: Creare API Web con ASP.NET Core
author: scottaddie
description: Informazioni sulle funzionalità disponibili per la creazione di un'API Web in ASP.NET Core e su quando è appropriato usare ciascuna funzionalità.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/06/2018
uid: web-api/index
ms.openlocfilehash: ccee4f7bae0abe1b36088d58e5c1e1362d8de9f0
ms.sourcegitcommit: 5338b1ed9e2ef225ab565d6cba072b474fd9324d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39243097"
---
# <a name="build-web-apis-with-aspnet-core"></a>Creare API Web con ASP.NET Core

Di [Scott Addie](https://github.com/scottaddie)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

Questo documento illustra come creare un'API Web in ASP.NET Core e indica quando è più appropriato usare ciascuna funzionalità.

## <a name="derive-class-from-controllerbase"></a>Derivare una classe da ControllerBase

Ereditare dalla classe [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) in un controller progettato per svolgere la funzione di API Web. Ad esempio:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

La classe `ControllerBase` consente l'accesso a diverse proprietà e a svariati metodi. Nel codice precedente, gli esempi comprendono [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) e [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction). Questi metodi vengono chiamati all'interno di metodi di azioni per restituire rispettivamente i codici di stato HTTP 400 e HTTP 201. Alla proprietà [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate), fornita anche questa da `ControllerBase`, si accede per gestire la convalida del modello di richiesta.

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a>Annotare una classe con l'attributo ApiController

ASP.NET Core 2.1 introduce l'attributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) per indicare una classe controller API Web. Ad esempio:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

Per usare questo attributo è necessaria una versione di compatibilità di 2.1 o versioni successive, impostata tramite [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion). Ad esempio, il codice evidenziato in *Startup.ConfigureServices* imposta il flag di compatibilità 2.1:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

L'attributo `[ApiController]` è in genere associato a `ControllerBase` per abilitare il comportamento specifico di REST per i controller. `ControllerBase` consente l'accesso a metodi quali [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) e [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).

Un altro approccio consiste nel creare una classe controller di base personalizzata annotata con l'attributo `[ApiController]`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

Le sezioni seguenti descrivono le funzionalità aggiunte dall'attributo che favoriscono una maggiore praticità.

### <a name="automatic-http-400-responses"></a>Risposte HTTP 400 automatiche

Gli errori di convalida attivano automaticamente una risposta HTTP 400. Il codice seguente non è più necessario nelle azioni:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

Il comportamento predefinito viene disabilitato quando la proprietà [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) è impostata su `true`. Aggiungere il codice seguente in *Startup.ConfigureServices* dopo `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a>Inferenza del parametro di origine di associazione

Un attributo di origine di associazione definisce la posizione in cui viene trovato il valore del parametro di un'azione. Esistono gli attributi di origine di associazione seguente:

|Attributo|Origine di associazione |
|---------|---------|
|**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**     | Corpo della richiesta |
|**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**     | Dati di modulo nel corpo della richiesta |
|**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)** | Intestazione della richiesta |
|**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**   | Parametri della stringa di query della richiesta |
|**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**   | Dati della route della richiesta corrente |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | Il servizio richiesta inserito come parametro di azione |

> [!WARNING]
> Non usare `[FromRoute]` quando i valori potrebbero contenere `%2f` (vale a dire `/`). `%2f` non sarà convertito in `/` rimuovendo i caratteri di escape. Usare `[FromQuery]` se il valore potrebbe contenere `%2f`.

Senza l'attributo `[ApiController]`, gli attributi di origine di associazione vengono definiti in modo esplicito. Nell'esempio seguente, l'attributo `[FromQuery]` indica che il valore del parametro `discontinuedOnly` è specificato nella stringa di query dell'URL della richiesta:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

Le regole di inferenza vengono applicate per le origini dati predefinite dei parametri di azione. Queste regole configurano le origini di associazione altrimenti applicate manualmente con ogni probabilità ai parametri di azione. Gli attributi di origine di associazione si comportano nel modo seguente:

* **[FromBody]**  viene dedotto per i parametri di tipo complesso. Un'eccezione a questa regola è costituita dai tipi incorporati complessi con un significato speciale, ad esempio [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) e [CancellationToken](/dotnet/api/system.threading.cancellationtoken). Il codice di inferenza di origine di associazione ignora tali tipi speciali. Quando per un'azione esistono più parametri, specificati in modo esplicito (tramite `[FromBody]`) o dedotti in quanto associati dal corpo della richiesta, viene generata un'eccezione. Le firme di azione seguenti, ad esempio, causano un'eccezione:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* **[FromForm]** viene dedotto per i parametri di azione di tipo [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) e [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection). Non viene dedotto per i tipi semplici o definiti dall'utente.
* **[FromRoute]**  viene dedotto per i nomi di parametro di azione corrispondenti a un parametro nel modello di route. Quando più di una route corrisponde a un parametro di azione, tutti i valori di route vengono considerati `[FromRoute]`.
* **[FromQuery]**  viene dedotto per tutti gli altri parametri di azione.

Le regole di inferenza predefinite vengono disabilitate quando la proprietà [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) è impostata su `true`. Aggiungere il codice seguente in *Startup.ConfigureServices* dopo `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>Inferenza di richieste multipart/form-data

Quando un parametro di azione è annotato con l'attributo [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute), viene dedotto il tipo di contenuto `multipart/form-data` per la richiesta.

Il comportamento predefinito viene disabilitato quando la proprietà [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) è impostata su `true`. Aggiungere il codice seguente in *Startup.ConfigureServices* dopo `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>Requisiti del routing degli attributi

Il routing degli attributi diventa un requisito. Ad esempio:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

Le azioni non sono accessibili tramite le [route convenzionali](xref:mvc/controllers/routing#conventional-routing) definite in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) o da [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.

::: moniker-end

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
