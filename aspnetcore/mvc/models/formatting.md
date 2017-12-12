---
title: Formattazione di dati di risposta in ASP.NET MVC di base
author: ardalis
description: Informazioni su come formattare i dati di risposta in ASP.NET MVC di base.
keywords: ASP.NET Core, dati di risposta, IOutputFormatter, IActionResult
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: c056df45-d013-4826-91a1-4a092bae1ea5
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/formatting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: abc125a093ff2cd5a38a537ecdfc795ff03e23f7
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/19/2017
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a>Introduzione alla formattazione dei dati di risposta in ASP.NET MVC di base

Da [Steve Smith](https://ardalis.com/)

ASP.NET MVC di base include il supporto incorporato per la formattazione di dati di risposta, utilizzando formati a lunghezza fissa o in risposta a specifiche del client.

[Consente di visualizzare o scaricare l'esempio da GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).

## <a name="format-specific-action-results"></a>Risultati dell'azione specifica di formato

Alcuni tipi di risultati di azione sono specifici di un particolare formato, ad esempio `JsonResult` e `ContentResult`. Azioni possono restituire risultati specifici che vengono sempre formattati in modo particolare. Ad esempio, restituendo un `JsonResult` restituisce i dati in formato JSON, indipendentemente dalle preferenze del client. Analogamente, restituendo un `ContentResult` restituirà dati stringa in formato testo normale (come verrà semplicemente che restituisce una stringa).

> [!NOTE]
> Un'azione non è necessario restituire qualsiasi tipo particolare. MVC supporta qualsiasi valore restituito dell'oggetto. Se un'operazione restituisce un `IActionResult` implementazione e il controller eredita da `Controller`, gli sviluppatori dispongono di molti metodi di supporto corrispondente a molte delle opzioni. Risultati di azioni che restituiscono oggetti che non sono `IActionResult` tipi verranno serializzati utilizzando il metodo appropriato `IOutputFormatter` implementazione.

Per restituire i dati in un formato specifico da un controller da cui eredita il `Controller` classe di base, utilizzare il metodo helper predefinite `Json` per restituire JSON e `Content` per testo normale. Il metodo di azione deve restituire il tipo di risultato specifico (ad esempio, `JsonResult`) o `IActionResult`.

Restituzione di dati in formato JSON:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

Risposta di esempio da questa azione:

![Scheda di rete di strumenti di sviluppo in Microsoft Edge che mostra il tipo di contenuto della risposta è application/json](formatting/_static/json-response.png)

Si noti che il tipo di contenuto della risposta è `application/json`, come illustrato nell'elenco di richieste di rete e nella sezione intestazioni di risposta. Si noti inoltre l'elenco di opzioni visualizzate dal browser nell'intestazione Accept nella sezione intestazioni di richiesta (in questo caso, Microsoft Edge). La tecnica corrente Ignora questa intestazione. Seguire il è illustrato di seguito.

Per restituire dati in formato testo normale, utilizzare `ContentResult` e `Content` helper:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

Una risposta da questa azione:

![Scheda di rete di strumenti di sviluppo in Microsoft Edge che mostra il tipo di contenuto della risposta è text/plain.](formatting/_static/text-response.png)

Si noti in questo caso il `Content-Type` restituita è `text/plain`. È anche possibile ottenere lo stesso comportamento utilizzando solo un tipo di risposta di stringa:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> Per le azioni non semplice con più restituire tipi o le opzioni (ad esempio, in base al risultato di operazioni eseguite diversi codici di stato HTTP), preferisce `IActionResult` come tipo restituito.

## <a name="content-negotiation"></a>Negoziazione del contenuto

Negoziazione di contenuto (*conneg* breve) si verifica quando il client specifica un [intestazione Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html). Il formato predefinito utilizzato da ASP.NET MVC di base è JSON. Negoziazione del contenuto viene implementata da `ObjectResult`. Viene generato anche nel codice di stato risultati specifici dell'azione restituiti da metodi di supporto (che sono tutti basati sul `ObjectResult`). È inoltre possibile restituire un tipo di modello (una classe sono stati definiti come tipo di trasferimento di dati) e il framework esegue automaticamente il wrapping in un `ObjectResult` automaticamente.

Usa il metodo di azione seguenti la `Ok` e `NotFound` metodi helper:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

Verrà restituita una risposta in formato JSON a meno che non è stato richiesto un altro formato e il server può restituire il formato richiesto. È possibile utilizzare uno strumento come [Fiddler](http://www.telerik.com/fiddler) per creare una richiesta che include un'intestazione Accept e specificare un altro formato. In questo caso, se il server ha un *formattatore* che può generare una risposta nel formato richiesto, il risultato verrà restituito nel formato preferito di client.

![Console di Fiddler che mostra un oggetto creato manualmente ottenere una richiesta con un valore di intestazione Accept di application/xml](formatting/_static/fiddler-composer.png)

Nella schermata precedente, creazione di Fiddler è stata utilizzata per generare una richiesta, specificare `Accept: application/xml`. Per impostazione predefinita, ASP.NET MVC Core supporta solo JSON, pertanto anche quando viene specificato un altro formato, il risultato restituito è ancora in formato JSON. Si noterà come aggiungere i formattatori aggiuntivi nella sezione successiva.

Le azioni del controller possono restituire POCOs (Plain Old CLR Object), nel qual caso ASP.NET MVC creerà automaticamente un `ObjectResult` per l'utente che esegue il wrapping dell'oggetto. Il client verrà visualizzato l'oggetto serializzato formattato (il valore predefinito è il formato JSON, XML o in altri formati, è possibile configurare). Se l'oggetto restituito è `null`, il framework restituirà un `204 No Content` risposta.

Restituzione di un tipo di oggetto:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

Nell'esempio, una richiesta per un alias valido autore riceverà una risposta 200 OK con i dati dell'autore. Una richiesta di un alias valido riceverà una risposta 204 Nessun contenuto. Screenshot che mostra la risposta in formato XML e JSON è illustrato di seguito.

### <a name="content-negotiation-process"></a>Processo di negoziazione del contenuto

Contenuto *negoziazione* avviene solo se un `Accept` intestazione è presente nella richiesta. Quando una richiesta contiene un'intestazione accept, il framework permette di enumerare i tipi di supporto nell'intestazione accept in ordine di preferenza e tenterà di trovare un formattatore in grado di produrre una risposta in uno dei formati specificati dall'intestazione accept. Nel caso in cui non viene trovato alcun formattatore che può soddisfare la richiesta del client, il framework tenterà di trovare il primo formattatore in grado di produrre una risposta (a meno che lo sviluppatore ha configurato l'opzione su `MvcOptions` per restituire 406 inaccettabile invece). Se la richiesta specifica di XML, ma il formattatore XML non è stato configurato, verrà utilizzato il formattatore JSON. Più in generale, se non è configurato alcun formattatore che può fornire il formato richiesto, quindi viene utilizzato il primo formattatore in grado di formattare l'oggetto. Se non viene specificata alcuna intestazione, il primo formattatore in grado di gestire l'oggetto deve essere restituito da utilizzare per serializzare la risposta. In questo caso, non è eseguita qualsiasi negoziazione - consiste nel determinare quale formato utilizzerà il server.

> [!NOTE]
> Se l'intestazione Accept contiene `*/*`, l'intestazione verrà ignorata a meno che non `RespectBrowserAcceptHeader` è impostata su true in `MvcOptions`.

### <a name="browsers-and-content-negotiation"></a>Browser e negoziazione del contenuto

A differenza delle tipiche client API, web browser tendono a fornire `Accept` le intestazioni che includono un'ampia gamma di formati, inclusi i caratteri jolly. Per impostazione predefinita, quando il framework rileva che la richiesta proviene da un browser, verrà ignorata il `Accept` intestazione e invece restituito il contenuto dell'applicazione configurata per il formato predefinito (JSON se non diversamente configurato). Ciò offre un'esperienza più coerenza quando si utilizza diversi browser per utilizzare le API.

Se si preferisce intestazioni accept browser onore applicazione, è possibile configurare questo come parte della configurazione del MVC impostando `RespectBrowserAcceptHeader` a `true` nel `ConfigureServices` metodo *Startup.cs*.

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a>Configurazione di formattatori

Se l'applicazione deve supportare formati aggiuntivi oltre il valore predefinito di JSON, è possibile aggiungere pacchetti NuGet e configurare MVC per supportarli. Sono disponibili i formattatori separati per l'input e output. Formattatori di input vengono utilizzati da [associazione di modelli](model-binding.md); formattatori di output vengono utilizzati per formattare le risposte. È inoltre possibile configurare [formattatori personalizzato](../advanced/custom-formatters.md).

### <a name="adding-xml-format-support"></a>Aggiunta di supporto del formato XML

Per aggiungere supporto per la formattazione XML, installare il `Microsoft.AspNetCore.Mvc.Formatters.Xml` pacchetto NuGet.

Aggiungere il XmlSerializerFormatters alla configurazione di MVC in *Startup.cs*:

[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

In alternativa, è possibile aggiungere solo il formattatore di output:

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

Questi due approcci eseguirà la serializzazione dei risultati mediante `System.Xml.Serialization.XmlSerializer`. Se si preferisce, è possibile utilizzare il `System.Runtime.Serialization.DataContractSerializer` aggiungendo il formattatore associato:

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

Dopo aver aggiunto il supporto per la formattazione XML, i metodi del controller devono restituire il formato appropriato in base alla richiesta `Accept` intestazione, come questa Fiddler riportato:

![Console Fiddler: scheda The non elaborato per la richiesta viene mostrato il valore dell'intestazione Accept è application/xml. La scheda non elaborata per la risposta Mostra il valore dell'intestazione Content-Type di application/xml.](formatting/_static/xml-response.png)

È possibile visualizzare nella scheda controlli che è stata effettuata la richiesta GET non elaborato con un `Accept: application/xml` intestazione set. Nel riquadro di risposta viene mostrato il `Content-Type: application/xml` , intestazione e il `Author` oggetto è stato serializzato in XML.

Utilizzare la scheda Composer per modificare la richiesta per specificare `application/json` nel `Accept` intestazione. Eseguire la richiesta e risposta verrà formattata come JSON:

![Console Fiddler: scheda The non elaborato per la richiesta viene mostrato il valore dell'intestazione Accept è application/json. La scheda non elaborata per la risposta Mostra il valore dell'intestazione Content-Type di application/json.](formatting/_static/json-response-fiddler.png)

In questa schermata, è possibile visualizzare la richiesta imposta un'intestazione di `Accept: application/json` e la risposta specifica lo stesso come relativo `Content-Type`. Il `Author` oggetto viene visualizzato nel corpo della risposta, in formato JSON.

### <a name="forcing-a-particular-format"></a>Utilizzo forzato di un particolare formato

Se si desidera limitare i formati di risposta per un'azione specifica, è possibile, è possibile applicare il `[Produces]` filtro. Il `[Produces]` filtro specifica i formati di risposta per un'azione specifica (o controller). Come la maggior parte [filtri](../controllers/filters.md), questa può essere applicata l'azione, un controller o un ambito globale.

```csharp
[Produces("application/json")]
public class AuthorsController
```

Il `[Produces]` filtro forzerà tutte le azioni all'interno di `AuthorsController` per restituire le risposte in formato JSON, anche se altri formattatori sono stati configurati per l'applicazione e il client fornito un `Accept` intestazione richiesta di un altro disponibili formato. Vedere [filtri](../controllers/filters.md) per ulteriori informazioni, incluse le modalità applicare filtri a livello globale.

### <a name="special-case-formatters"></a>Formattatori di casi speciali

Alcuni casi speciali vengono implementati mediante i formattatori predefiniti. Per impostazione predefinita, `string` verranno formattati come tipi restituiti *text/plain* (*text/html* se richiesto tramite `Accept` intestazione). Questo comportamento può essere rimossa mediante la rimozione di `TextOutputFormatter`. La rimozione di formattatori nel `Configure` metodo *Startup.cs* (mostrato sotto). Le azioni con un oggetto modello restituiscono tipo non restituirà un 204 Nessun contenuto risposta quando viene restituito `null`. Questo comportamento può essere rimossa mediante la rimozione di `HttpNoContentOutputFormatter`. Nell'esempio di codice rimuove il `TextOutputFormatter` e `HttpNoContentOutputFormatter`.

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

Senza il `TextOutputFormatter`, `string` restituire tipi 406-pagina non valida, ad esempio. Si noti che se esiste un formattatore XML, verrà formattata `string` tipi restituiti, se il `TextOutputFormatter` viene rimosso.

Senza il `HttpNoContentOutputFormatter`, oggetti null vengono formattati mediante il formattatore configurato. Ad esempio, il formattatore JSON limitino a restituire una risposta con un corpo di `null`, mentre il formattatore XML verrà restituito un elemento XML vuoto con l'attributo `xsi:nil="true"` impostato.

## <a name="response-format-url-mappings"></a>Mapping di URL di formato di risposta

I client possono richiedere un formato particolare come parte dell'URL, ad esempio nella stringa di query o parte del percorso, o mediante un'estensione di file di formato, ad esempio XML o JSON. Il mapping da un percorso della richiesta deve essere specificato nella route che utilizza l'API. Ad esempio:

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

Questa route consentirebbe il formato richiesto di specificare come un'estensione di file facoltativo. Il `[FormatFilter]` attributo verifica l'esistenza del valore di formato di `RouteData` e verrà eseguito il mapping il formato della risposta al formattatore appropriato quando viene creata la risposta.

| Route                      | Formattatore                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | Il formattatore di output predefinito       |
| `/products/GetById/5.json` | Il formattatore JSON (se configurata) |
| `/products/GetById/5.xml`  | Il formattatore XML (se configurata)  |
