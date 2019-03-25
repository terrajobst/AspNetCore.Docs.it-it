---
title: Formattare i dati di risposta nell'API Web ASP.NET Core
author: ardalis
description: Informazioni su come formattare i dati di risposta nell'API Web ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
uid: web-api/advanced/formatting
ms.openlocfilehash: b0fce0632fd2d885cb8e9a056923ec365d2f327d
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209986"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a>Formattare i dati di risposta nell'API Web ASP.NET Core

Di [Steve Smith](https://ardalis.com/)

ASP.NET Core MVC include il supporto incorporato per la formattazione dei dati di risposta, tramite formati fissi o basati sulle specifiche dei client.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="format-specific-action-results"></a>Risultati di azioni specifici per il formato

Alcuni tipi di risultati di azioni sono specifici di un particolare formato, ad esempio `JsonResult` e `ContentResult`. Le azioni possono restituire risultati specifici che vengono sempre formattati in modo particolare. Con `JsonResult`, ad esempio, vengono restituiti dati in formato JSON, indipendentemente dalle preferenze del client. Analogamente, con `ContentResult` vengono restituiti dati stringa in formato testo normale (ovvero vengono semplicemente restituite stringhe).

> [!NOTE]
> Un'azione non deve necessariamente restituire un tipo specifico. MVC supporta qualsiasi valore restituito oggetto. Se un'azione restituisce un'implementazione `IActionResult` e il controller eredita da `Controller`, gli sviluppatori hanno a disposizione molti metodi helper, corrispondenti a molte delle scelte. I risultati di azioni che restituiscono oggetti che non corrispondono a tipi `IActionResult` vengono serializzati tramite l'implementazione `IOutputFormatter` appropriata.

Per restituire i dati in un formato specifico da un controller che eredita dalla classe di base `Controller`, usare il metodo helper predefinito `Json` per restituire JSON e `Content` per restituire testo normale. Il metodo di azione usato deve restituire il tipo di risultato specifico (ad esempio, `JsonResult`) o `IActionResult`.

Restituzione di dati in formato JSON:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

Risposta di esempio da questa azione:

![Scheda Rete di Strumenti di sviluppo in Microsoft Edge che mostra che il tipo di contenuto della risposta è application/json](formatting/_static/json-response.png)

Si noti che il tipo di contenuto della risposta, `application/json`, è illustrato sia nell'elenco delle richieste di rete e nella sezione Intestazioni delle risposte. Si noti anche l'elenco delle opzioni visualizzate dal browser (in questo caso, Microsoft Edge) nell'intestazione Accept nella sezione Intestazioni delle risposte. La tecnica corrente ignora questa intestazione. Di seguito viene illustrato come seguirla.

Per restituire dati in formato testo normale, usare `ContentResult` e l'helper `Content`:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

Una risposta da questa azione:

![Scheda Rete di Strumenti di sviluppo in Microsoft Edge che mostra che il tipo di contenuto della risposta è text/plain](formatting/_static/text-response.png)

Si noti che in questo caso il `Content-Type` restituito è `text/plain`. È anche possibile ottenere lo stesso comportamento usando solo un tipo di risposta stringa:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> Per azioni non banali con più tipi restituiti o opzioni, ad esempio codici di stato HTTP diversi a seconda del risultato delle operazioni eseguite, preferire `IActionResult` come tipo restituito.

## <a name="content-negotiation"></a>Negoziazione del contenuto

La negoziazione del contenuto (abbreviata in *conneg*) si verifica quando il client specifica un'[intestazione Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html). Il formato predefinito usato da ASP.NET Core MVC è JSON. La negoziazione del contenuto viene implementata da `ObjectResult`. È anche incorporata nei risultati dell'azione specifica del codice stato restituiti dai metodi helper (tutti basati su `ObjectResult`). È anche possibile restituire un tipo di modello (una classe definita come tipo di trasferimento dei dati personalizzato). Il framework eseguirà automaticamente il wrapping in un `ObjectResult`.

Il metodo di azione seguente usa i metodi helper `Ok` e `NotFound`:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

Verrà restituita una risposta in formato JSON, a meno che non sia stato richiesto un altro formato che il server è in grado di restituire. Per creare una richiesta che includa un'intestazione Accept e specificare un altro formato, è possibile usare uno strumento come [Fiddler](http://www.telerik.com/fiddler). In questo caso, se il server ha un *formattatore* in grado di generare una risposta nel formato richiesto, il risultato viene restituito nel formato preferito del client.

![Console di Fiddler che mostra una richiesta GET creata manualmente con un'intestazione Accept corrispondente ad application/xml](formatting/_static/fiddler-composer.png)

Nello screenshot precedente, è stato usato Fiddler Composer per generare una richiesta, specificando `Accept: application/xml`. Per impostazione predefinita, ASP.NET Core MVC supporta solo il formato JSON. Anche se viene specificato un altro formato, quindi, il risultato restituito è comunque in formato JSON. Nella prossima sezione viene illustrato come aggiungere altri formattatori.

Le azioni del controller possono restituire oggetti POCO (Plain Old CLR Object). In questo caso ASP.NET Core MVC crea automaticamente un `ObjectResult` per il wrapping dell'oggetto. Il client otterrà l'oggetto serializzato formattato. L'impostazione predefinita è il formato JSON. È possibile configurare il formato XML o altri formati. Se l'oggetto restituito è `null`, il framework restituisce una risposta `204 No Content`.

Restituzione di un tipo di oggetto:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

Nell'esempio, la richiesta di un alias autore valido riceve una risposta 200 OK con i dati dell'autore. La richiesta di un alias non valido riceve una risposta 204 Nessun contenuto. Di seguito sono riportati gli screenshot con la risposta nei formati XML e JSON.

### <a name="content-negotiation-process"></a>Processo di negoziazione del contenuto

La *negoziazione* del contenuto avviene solo se nella richiesta è presente un'intestazione `Accept`. Se una richiesta contiene un'intestazione Accept, il framework enumera i tipi di supporti in tale intestazione in ordine di preferenza e tenta di trovare un formattatore in grado di generare una risposta in uno dei formati specificati dall'intestazione Accept. Nel caso in cui non venga trovato alcun formattatore in grado di soddisfare la richiesta del client, il framework tenta di trovare il primo formattatore in grado di generare una risposta, a meno che lo sviluppatore non abbia configurato in `MvcOptions` l'opzione per restituire l'errore 406 Pagina non valida. Se la richiesta specifica il formato XML, ma il formattatore XML non è stato configurato, viene usato il formattatore JSON. Più in generale, se non è configurato alcun formattatore in grado di offrire il formato richiesto, viene usato il primo formattatore che possa formattare l'oggetto. Se non viene specificata alcuna intestazione, viene usato il primo formattatore in grado di gestire l'oggetto da restituire. In questo caso, non avviene alcuna negoziazione. È il server a determinare il formato da usare.

> [!NOTE]
> Se l'intestazione Accept contiene `*/*`, l'intestazione viene tenuta in considerazione solo se l'opzione `RespectBrowserAcceptHeader` è impostata su true in `MvcOptions`.

### <a name="browsers-and-content-negotiation"></a>Browser e negoziazione del contenuto

A differenza dei client API tipici, i Web browser tendono a fornire intestazioni `Accept` che includono un'ampia gamma di formati, inclusi i caratteri jolly. Per impostazione predefinita, se il framework rileva che la richiesta proviene da un browser, ignora l'intestazione `Accept` e restituisce il contenuto nel formato predefinito dell'applicazione, ovvero nel formato JSON, se la configurazione non è stata modificata. Ciò offre un'esperienza più coerente quando si usano API con browser diversi.

Se si preferisce che l'applicazione rispetti le intestazioni Accept del browser, è possibile configurare questo aspetto nell'ambito della configurazione di MVC impostando `RespectBrowserAcceptHeader` su `true` nel metodo `ConfigureServices` in *Startup.cs*.

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a>Configurazione di formattatori

Se oltre al formato JSON predefinito l'applicazione deve supportare formati aggiuntivi, è possibile aggiungere pacchetti NuGet e configurare MVC in modo che possa supportarli. Esistono formattatori separati per input e output. I formattatori di input vengono usati dall'[associazione di modelli](xref:mvc/models/model-binding), mentre i formattatori di output vengono usati per formattare le risposte. È anche possibile configurare [formattatori personalizzati](xref:web-api/advanced/custom-formatters).

### <a name="adding-xml-format-support"></a>Aggiunta del supporto per il formato XML

Per aggiungere il supporto della formattazione XML, installare il pacchetto NuGet `Microsoft.AspNetCore.Mvc.Formatters.Xml`.

Aggiungere XmlSerializerFormatters alla configurazione di MVC in *Startup.cs*:

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

In alternativa, è possibile aggiungere solo il formattatore di output:

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

Questi due approcci serializzano i risultati tramite `System.Xml.Serialization.XmlSerializer`. Se si preferisce, è possibile usare `System.Runtime.Serialization.DataContractSerializer` tramite l'aggiunta del formattatore associato:

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

Dopo aver aggiunto il supporto per la formattazione XML, i metodi del controller devono restituire il formato appropriato in base all'intestazione `Accept` della richiesta, come illustrato da questo esempio in Fiddler:

![Console di Fiddler: la scheda Raw (Non elaborato) relativa alla richiesta mostra che il valore dell'intestazione Accept è application/xml. La scheda Raw (Non elaborato) per la risposta mostra un valore dell'intestazione Content-Type corrispondente ad application/xml.](formatting/_static/xml-response.png)

Nella scheda Inspectors (Controlli) è possibile vedere che per la richiesta GET in Raw (Non elaborato) è impostata un'intestazione `Accept: application/xml`. Il riquadro della risposta mostra l'intestazione `Content-Type: application/xml` e l'oggetto `Author` serializzato in XML.

Usare la scheda Composer (Compositore) per modificare la richiesta, specificando `application/json` nell'intestazione `Accept`. Eseguire la richiesta. La risposta verrà formattata come JSON:

![Console di Fiddler: la scheda Raw (Non elaborato) relativa alla richiesta mostra che il valore dell'intestazione Accept è application/json. La scheda Raw (Non elaborato) per la risposta mostra un valore dell'intestazione Content-Type corrispondente ad application/json.](formatting/_static/json-response-fiddler.png)

In questo screenshot, è possibile vedere che la richiesta imposta un'intestazione corrispondente a `Accept: application/json` e che la risposta specifica lo stesso valore come `Content-Type`. L'oggetto `Author` viene visualizzato nel corpo della risposta, in formato JSON.

### <a name="forcing-a-particular-format"></a>Imposizione di un formato specifico

Se si vogliono limitare i formati di risposta per un'azione specifica, è possibile applicare il filtro `[Produces]`. Il filtro `[Produces]` specifica i formati di risposta per un'azione o per un controller specifico. Come la maggior parte dei [filtri](xref:mvc/controllers/filters), questo può essere applicato all'azione, al controller o all'ambito globale.

```csharp
[Produces("application/json")]
public class AuthorsController
```

Il filtro `[Produces]` impone a tutte le azioni all'interno di `AuthorsController` di restituire risposte in formato JSON, anche se altri formattatori configurati per l'applicazione e il client hanno fornito un'intestazione `Accept` che richiede un formato disponibile diverso. Per altre informazioni, ad esempio su come applicare filtri a livello globale, vedere [Filtri](xref:mvc/controllers/filters).

### <a name="special-case-formatters"></a>Formattatori per casi speciali

Alcuni casi speciali vengono implementati tramite formattatori predefiniti. Per impostazione predefinita, i tipi restituiti `string` vengono formattati come *text/plain* (*text/html* se richiesto tramite intestazione `Accept`). È possibile rimuovere questo comportamento rimuovendo `TextOutputFormatter`. È possibile rimuovere formattatori del metodo `Configure` in *Startup.cs* (illustrato di seguito). Le azioni con un tipo restituito corrispondente a oggetto modello restituiscono una risposta 204 Nessun contenuto quando restituiscono `null`. È possibile rimuovere questo comportamento rimuovendo `HttpNoContentOutputFormatter`. Il codice seguente rimuove `TextOutputFormatter` e `HttpNoContentOutputFormatter`.

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

Senza `TextOutputFormatter`, i tipi restituiti `string` restituiscono, ad esempio, 406 Pagina non valida. Si noti che se `TextOutputFormatter` è stato rimosso ed è presente un formattatore XML, i tipi restituiti `string` vengono formattati da quest'ultimo.

Senza `HttpNoContentOutputFormatter`, gli oggetti Null vengono formattati tramite il formattatore configurato. Ad esempio, il formattatore JSON si limita a restituire una risposta con corpo `null`, mentre il formattatore XML restituisce un elemento XML vuoto con l'attributo `xsi:nil="true"` impostato.

## <a name="response-format-url-mappings"></a>Mapping di URL dei formati di risposta

I client possono richiedere un formato specifico all'interno dell'URL, ad esempio nella stringa di query o in una parte del percorso, oppure tramite un'estensione di file specifica di formato, ad esempio xml o json. Il mapping dal percorso della richiesta deve essere specificato nella route usata dall'API. Ad esempio:

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

Questa route consente di specificare il formato richiesto sotto forma di estensione di file facoltativa. L'attributo `[FormatFilter]` verifica l'esistenza del valore di formato in `RouteData` e quando la risposta viene creata esegue il mapping del formato della risposta al formattatore appropriato.

|           Route            |             Formattatore              |
|----------------------------|------------------------------------|
|   `/products/GetById/5`    |    Formattatore di output predefinito    |
| `/products/GetById/5.json` | Formattatore JSON (se configurato) |
| `/products/GetById/5.xml`  | Formattatore XML (se configurato)  |
