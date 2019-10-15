---
title: Formattare i dati di risposta nell'API Web ASP.NET Core
author: ardalis
description: Informazioni su come formattare i dati di risposta nell'API Web ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 8/22/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: 0dd8b3b5ec58a199db086c4c0b0f057d26afd589
ms.sourcegitcommit: 7a2c820692f04bfba04398641b70f27fa15391f5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/12/2019
ms.locfileid: "72290626"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a>Formattare i dati di risposta nell'API Web ASP.NET Core

[Rick Anderson](https://twitter.com/RickAndMSFT) e [Steve Smith](https://ardalis.com/)

ASP.NET Core MVC supporta la formattazione dei dati di risposta. I dati di risposta possono essere formattati usando formati specifici o in risposta al formato richiesto dal client.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="format-specific-action-results"></a>Risultati azione specifici del formato

Alcuni tipi di risultati di azioni sono specifici di un particolare formato, ad esempio <xref:Microsoft.AspNetCore.Mvc.JsonResult> e <xref:Microsoft.AspNetCore.Mvc.ContentResult>. Le azioni possono restituire risultati formattati in un formato specifico, indipendentemente dalle preferenze dei client. Ad esempio, la restituzione di `JsonResult` restituisce dati in formato JSON. La restituzione di `ContentResult` o una stringa restituisce dati di stringa in formato testo normale.

Non è necessario eseguire un'azione per restituire un tipo specifico. ASP.NET Core supporta qualsiasi valore restituito da un oggetto.  I risultati di azioni che restituiscono oggetti che non sono tipi <xref:Microsoft.AspNetCore.Mvc.IActionResult> vengono serializzati usando l'implementazione <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> appropriata. Per altre informazioni, vedere <xref:web-api/action-return-types>.

Il metodo helper predefinito <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> restituisce dati in formato JSON: [!code-csharp @ no__t-2

Il download di esempio restituisce l'elenco degli autori. Utilizzando gli strumenti di sviluppo del browser [F12 o l'](https://www.getpostman.com/tools) autore del codice precedente:

* Viene visualizzata l'intestazione della risposta contenente **Content-Type:** `application/json; charset=utf-8`.
* Verranno visualizzate le intestazioni della richiesta. Ad esempio, l'intestazione `Accept`. L'intestazione `Accept` viene ignorata dal codice precedente.

Per restituire dati in formato testo normale, usare <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> e l'helper <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content>:

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_about)]

Nel codice precedente, il `Content-Type` restituito è `text/plain`. La restituzione di una stringa fornisce `Content-Type` di `text/plain`:

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_string)]

Per le azioni con più tipi restituiti, restituire `IActionResult`. Ad esempio, la restituzione di codici di stato HTTP diversi in base al risultato delle operazioni eseguite.

## <a name="content-negotiation"></a>Negoziazione del contenuto

La negoziazione del contenuto si verifica quando il client specifica un' [intestazione Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html). Il formato predefinito usato da ASP.NET Core è [JSON](https://json.org/). Negoziazione del contenuto:

* Implementato da <xref:Microsoft.AspNetCore.Mvc.ObjectResult>.
* Incorporata nei risultati dell'azione specifici del codice di stato restituiti dai metodi helper. I metodi helper dei risultati delle azioni sono basati su `ObjectResult`.

Quando viene restituito un tipo di modello, il tipo restituito è `ObjectResult`.

Il metodo di azione seguente usa i metodi helper `Ok` e `NotFound`:

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_search)]

Per impostazione predefinita, ASP.NET Core supporta i tipi di supporto `application/json`, `text/json` e `text/plain`. Gli strumenti, ad esempio [Fiddler](https://www.telerik.com/fiddler) o [post](https://www.getpostman.com/tools) , possono impostare l'intestazione della richiesta `Accept` per specificare il formato restituito. Quando l'intestazione `Accept` contiene un tipo supportato dal server, viene restituito tale tipo. Nella sezione successiva viene illustrato come aggiungere formattatori aggiuntivi.

Le azioni del controller possono restituire POCO (Plain Old CLR Objects). Quando viene restituito un oggetto POCO, il runtime crea automaticamente un `ObjectResult` che esegue il wrapping dell'oggetto. Il client ottiene l'oggetto serializzato formattato. Se l'oggetto restituito è `null`, viene restituita una risposta `204 No Content`.

Restituzione di un tipo di oggetto:

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_alias)]

Nel codice precedente, una richiesta di un alias autore valido restituisce una risposta `200 OK` con i dati dell'autore. Una richiesta di un alias non valido restituisce una risposta `204 No Content`.

### <a name="the-accept-header"></a>Intestazione Accept

La *negoziazione* del contenuto viene verificata quando viene visualizzata un'intestazione `Accept` nella richiesta. Quando una richiesta contiene un'intestazione Accept, ASP.NET Core:

* Enumera i tipi di supporto nell'intestazione Accept in ordine di preferenza.
* Tenta di trovare un formattatore in grado di generare una risposta in uno dei formati specificati.

Se non viene trovato alcun formattatore in grado di soddisfare la richiesta del client, ASP.NET Core:

* Restituisce `406 Not Acceptable` se è stato impostato <xref:Microsoft.AspNetCore.Mvc.MvcOptions> oppure-
* Tenta di trovare il primo formattatore in grado di generare una risposta.

Se non è configurato alcun formattatore per il formato richiesto, viene utilizzato il primo formattatore in grado di formattare l'oggetto. Se nella richiesta non è presente alcuna intestazione `Accept`:

* Il primo formattatore in grado di gestire l'oggetto viene utilizzato per serializzare la risposta.
* Non si verifica alcuna negoziazione. Il server sta determinando il formato da restituire.

Se l'intestazione Accept contiene `*/*`, l'intestazione viene ignorata a meno che `RespectBrowserAcceptHeader` non sia impostato su true in <xref:Microsoft.AspNetCore.Mvc.MvcOptions>.

### <a name="browsers-and-content-negotiation"></a>Browser e negoziazione del contenuto

A differenza dei client API tipici, i Web browser forniscono intestazioni `Accept`. Web browser specificare molti formati, inclusi i caratteri jolly. Per impostazione predefinita, quando il Framework rileva che la richiesta viene ricevuta da un browser:

* L'intestazione `Accept` viene ignorata.
* Il contenuto viene restituito in JSON, a meno che non sia configurato diversamente.

Ciò offre un'esperienza più coerente nei browser quando si utilizzano le API.

Per configurare un'app in modo da rispettare le intestazioni Accept del browser, impostare <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> su `true`:

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end

### <a name="configure-formatters"></a>Configura formattatori

Le app che devono supportare formati aggiuntivi possono aggiungere i pacchetti NuGet appropriati e configurare il supporto. Esistono formattatori separati per input e output. I formattatori di input vengono utilizzati dall' [associazione di modelli](xref:mvc/models/model-binding). I formattatori di output vengono utilizzati per formattare le risposte. Per informazioni sulla creazione di un formattatore personalizzato, vedere [formattatori personalizzati](xref:web-api/advanced/custom-formatters).

::: moniker range=">= aspnetcore-3.0"

### <a name="add-xml-format-support"></a>Aggiungere il supporto per il formato XML

I formattatori XML implementati con <xref:System.Xml.Serialization.XmlSerializer> sono configurati chiamando <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:

[!code-csharp[](./formatting/3.0sample/Startup.cs?name=snippet)]

Il codice precedente serializza i risultati usando `XmlSerializer`.

Quando si usa il codice precedente, i metodi del controller devono restituire il formato appropriato in base all'intestazione `Accept` della richiesta.

### <a name="configure-systemtextjson-based-formatters"></a>Configurare i formattatori basati su System.Text.Json

Le funzionalità per i formattatori basati su `System.Text.Json` possono essere configurate tramite `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`.

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    // Use the default property (Pascal) casing.
    options.SerializerOptions.PropertyNamingPolicy = null;

    // Configure a custom converter.
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

È possibile configurare le opzioni di serializzazione dell'output, in base alle singole azioni, usando `JsonResult`. Esempio:

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerOptions
    {
        options.WriteIndented = true,
    });
}
```

### <a name="add-newtonsoftjson-based-json-format-support"></a>Aggiungere il supporto del formato JSON basato su Newtonsoft.Json

Prima del ASP.NET Core 3,0, i formattatori JSON usati per impostazione predefinita sono stati implementati usando il pacchetto `Newtonsoft.Json`. In ASP.NET Core 3.0 o versione successiva, i formattatori JSON predefiniti sono basati su `System.Text.Json`. Il supporto per formattatori e funzionalità basati su `Newtonsoft.Json` è disponibile installando il pacchetto NuGet [Microsoft. AspNetCore. Mvc. NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) e configurarlo in `Startup.ConfigureServices`.

[!code-csharp[](./formatting/3.0sample/StartupNewtonsoftJson.cs?name=snippet)]

Alcune funzionalità potrebbero non funzionare correttamente con i formattatori basati su `System.Text.Json` e richiedono un riferimento ai formattatori basati su @no__t 1. Continuare a usare i formattatori basati su `Newtonsoft.Json` se l'app:

* USA gli attributi `Newtonsoft.Json`. Ad esempio, `[JsonProperty]` o `[JsonIgnore]`.
* Personalizza le impostazioni di serializzazione.
* Si basa sulle funzionalità fornite da `Newtonsoft.Json`.
* Configura `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`. Prima di ASP.NET Core 3.0 `JsonResult.SerializerSettings` accetta un'istanza di `JsonSerializerSettings` specifica di `Newtonsoft.Json`.
* Genera documentazione [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>).

È possibile configurare le funzionalità per i formattatori basati su @no__t 0 con `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:

```csharp
services.AddControllers().AddNewtonsoftJson(options =>
{
    // Use the default property (Pascal) casing
    options.SerializerSettings.ContractResolver = new DefaultContractResolver();

    // Configure a custom converter
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

È possibile configurare le opzioni di serializzazione dell'output, in base alle singole azioni, usando `JsonResult`. Esempio:

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerSettings
    {
        options.Formatting = Formatting.Indented,
    });
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

### <a name="add-xml-format-support"></a>Aggiungere il supporto per il formato XML

Per la formattazione XML è necessario il pacchetto NuGet [Microsoft. AspNetCore. Mvc. Formatters. XML](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) .

I formattatori XML implementati con <xref:System.Xml.Serialization.XmlSerializer> sono configurati chiamando <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet)]

Il codice precedente serializza i risultati usando `XmlSerializer`.

Quando si usa il codice precedente, i metodi del controller devono restituire il formato appropriato in base all'intestazione `Accept` della richiesta.

::: moniker-end

### <a name="specify-a-format"></a>Specificare un formato

Per limitare i formati di risposta, applicare il filtro [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) . Come la maggior parte `[Produces]` dei [filtri](xref:mvc/controllers/filters), può essere applicato all'azione, al controller o all'ambito globale:

[!code-csharp[](./formatting/3.0sample/Controllers/WeatherForecastController.cs?name=snippet)]

Il filtro [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) precedente:

* Impone a tutte le azioni all'interno del controller di restituire risposte in formato JSON.
* Se sono configurati altri formattatori e il client specifica un formato diverso, viene restituito JSON.

Per ulteriori informazioni, vedere [filtri](xref:mvc/controllers/filters).

### <a name="special-case-formatters"></a>Formattatori case speciali

Alcuni casi speciali vengono implementati tramite formattatori predefiniti. Per impostazione predefinita, i tipi restituiti `string` vengono formattati come *text/plain* (*text/html* se richiesto tramite l'intestazione `Accept`). Questo comportamento può essere eliminato rimuovendo il <xref:Microsoft.AspNetCore.Mvc.Formatters.TextOutputFormatter>. I formattatori vengono rimossi nel metodo `Configure`. Le azioni che hanno un tipo restituito da un oggetto modello restituiscono `204 No Content` quando restituisce `null`. Questo comportamento può essere eliminato rimuovendo il <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>. Il codice seguente rimuove `TextOutputFormatter` e `HttpNoContentOutputFormatter`.

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupTextOutputFormatter.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupTextOutputFormatter.cs?name=snippet)]
::: moniker-end

Se non si `TextOutputFormatter`, i tipi restituiti `string` restituiscono `406 Not Acceptable`. Se esiste un formattatore XML, formatta i tipi restituiti `string` se il `TextOutputFormatter` è stato rimosso.

Senza `HttpNoContentOutputFormatter`, gli oggetti Null vengono formattati tramite il formattatore configurato. Esempio:

* Il formattatore JSON restituisce una risposta con un corpo di `null`.
* Il formattatore XML restituisce un elemento XML vuoto con l'attributo @no__t set-0.

## <a name="response-format-url-mappings"></a>Mapping URL formato risposta

I client possono richiedere un particolare formato come parte dell'URL, ad esempio:

* Nella stringa di query o in una parte del percorso.
* Utilizzando un'estensione di file specifica del formato, ad esempio XML o JSON.

Il mapping dal percorso della richiesta deve essere specificato nella route usata dall'API. Esempio:

[!code-csharp[](./formatting/sample/Controllers/ProductsController.cs?name=snippet)]

La route precedente consente di specificare il formato richiesto come estensione di file facoltativa. Con l'attributo [`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) viene verificata l'esistenza del valore del formato nel `RouteData` e viene eseguito il mapping del formato della risposta al formattatore appropriato al momento della creazione della risposta.

|           Route        |             Formattatore              |
|------------------------|------------------------------------|
|   `/api/products/5`    |    Formattatore di output predefinito    |
| `/api/products/5.json` | Formattatore JSON (se configurato) |
| `/api/products/5.xml`  | Formattatore XML (se configurato)  |
