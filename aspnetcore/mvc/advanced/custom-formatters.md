---
title: Formattatori personalizzati nell'API web ASP.NET MVC di base
author: tdykstra
description: Informazioni su come creare e utilizzare i formattatori personalizzati per il web API in ASP.NET Core.
keywords: ASP.NET Core, web api, formattatori personalizzati
ms.author: tdykstra
manager: wpickett
ms.date: 02/08/2017
ms.topic: article
ms.assetid: 1fb6fdc2-e199-4469-9012-b909d1913422
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/custom-formatters
ms.openlocfilehash: 5e665abe10fd7444c3fd5f20cfeca3ef0a5f79d3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="custom-formatters-in-aspnet-core-mvc-web-apis"></a>Formattatori personalizzati nell'API web ASP.NET MVC di base

Da [Tom Dykstra](https://github.com/tdykstra)

ASP.NET MVC di base dispone di supporto integrato per lo scambio di dati nell'API web usando i formati di JSON, XML o testo normale. In questo articolo viene illustrato come aggiungere il supporto per formati aggiuntivi tramite la creazione di formattatori personalizzati.

[Consente di visualizzare o scaricare l'esempio da GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).

## <a name="when-to-use-custom-formatters"></a>Quando utilizzare formattatori personalizzati

Utilizzare un formattatore personalizzato quando si desidera che il [negoziazione del contenuto](xref:mvc/models/formatting) processo per supportare un tipo di contenuto che non è supportato dai formattatori incorporati (JSON, XML e testo normale).

Ad esempio, se alcuni dei client per l'API web in grado di gestire il [Protobuf](https://github.com/google/protobuf) formato, si potrebbe voler usare Protobuf con i client perché risulta più efficiente.  È necessario che l'API web per inviare i nomi dei contatti e indirizzi in [vCard](https://wikipedia.org/wiki/VCard) formato, un formato di uso comune per lo scambio di dati di contatto. L'app di esempio fornito in questo articolo implementa un formattatore vCard semplice.

## <a name="overview-of-how-to-use-a-custom-formatter"></a>Panoramica di come utilizzare un formattatore personalizzato

Ecco i passaggi per creare e utilizzare un formattatore personalizzato:

* Se si desidera serializzare i dati da inviare al client, creare una classe di output del formattatore.
* Se si desidera deserializzare i dati ricevuti dal client, creare una classe formattatore di input. 
* Aggiungere istanze dei formattatori dal `InputFormatters` e `OutputFormatters` raccolte in [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).

Le sezioni seguenti forniscono istruzioni ed esempi di codice per ognuno di questi passaggi.

## <a name="how-to-create-a-custom-formatter-class"></a>Come creare una classe formattatore personalizzato

Per creare un formattatore:

* Derivare la classe dalla classe di base appropriata.
* Specificare i tipi di supporto valido e le codifiche nel costruttore.
* Eseguire l'override `CanReadType` / `CanWriteType` metodi
* Eseguire l'override `ReadRequestBodyAsync` / `WriteResponseBodyAsync` metodi
  
### <a name="derive-from-the-appropriate-base-class"></a>Derivare dalla classe di base appropriata

Per tipi di file di testo (ad esempio, vCard), derivare la [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) o [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) classe di base.

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

Per i tipi binari, derivare la [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) o [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) classe di base.

### <a name="specify-valid-media-types-and-encodings"></a>Specificare le codifiche e tipi di supporto valido

Nel costruttore, specificare tipi di supporto valido e codifiche aggiungendo il `SupportedMediaTypes` e `SupportedEncodings` raccolte.

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]  
> È possibile eseguire l'inserimento di dipendenze di costruttore in una classe del formattatore. Ad esempio, è possibile ottenere un logger mediante l'aggiunta di un parametro di logger al costruttore. Per accedere ai servizi, è necessario utilizzare l'oggetto di contesto passato ai metodi di. Un esempio di codice [seguito](#read-write) viene illustrato come eseguire questa operazione.

### <a name="override-canreadtypecanwritetype"></a>Eseguire l'override CanReadType/CanWriteType 

Specificare il tipo è possibile deserializzare in o serializzare da eseguendo l'override di `CanReadType` o `CanWriteType` metodi. Ad esempio, potrebbe essere solo in grado di creare testo vCard da un `Contact` tipo e viceversa.

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a>Il metodo CanWriteResult

In alcuni scenari è necessario eseguire l'override `CanWriteResult` anziché `CanWriteType`. Utilizzare `CanWriteResult` se vengono soddisfatte le condizioni seguenti:

  * Il metodo di azione restituisce una classe di modello.
  * Sono disponibili le classi derivate che potrebbero essere restituite in fase di esecuzione.
  * È necessario conoscere in fase di esecuzione che derivata a classe è stata restituita dall'azione.  

Ad esempio, si supponga che la firma del metodo di azione restituisce un `Person` tipo, ma può restituire un `Student` o `Instructor` tipo che deriva da `Person`. Se si desidera che il formattatore di gestire solo `Student` oggetti, controllare il tipo di [oggetto](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) nell'oggetto di contesto fornito per il `CanWriteResult` metodo. Si noti che non è necessario utilizzare `CanWriteResult` quando il metodo di azione restituisce `IActionResult`; in tal caso, il `CanWriteType` metodo riceve il tipo di runtime.

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a>Eseguire l'override ReadRequestBodyAsync/WriteResponseBodyAsync 

Eseguire il lavoro effettivo del deserializzazione o la serializzazione `ReadRequestBodyAsync` o `WriteResponseBodyAsync`.  Le righe evidenziate nell'esempio seguente viene illustrato come ottenere servizi dal contenitore dell'inserimento di dipendenza (non è possibile ottenere tali da parametri del costruttore).

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a>Come configurare MVC per utilizzare un formattatore personalizzato
 
Per utilizzare un formattatore personalizzato, aggiungere un'istanza della classe del formattatore per la `InputFormatters` o `OutputFormatters` insieme.

[!code-csharp[Main](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

Formattatori vengono valutati nell'ordine che inserirli. Il primo ha la precedenza. 

## <a name="next-steps"></a>Passaggi successivi

Vedere il [applicazione di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), che implementa vCard semplice input e output formattatori.  L'applicazione legge e scrive vCard che come illustrato nell'esempio seguente:

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

Per visualizzare vCard output, eseguire l'applicazione, inviare una richiesta Get con Accept intestazione "text/vcard" per `http://localhost:63313/api/contacts/` (se eseguito da Visual Studio) o `http://localhost:5000/api/contacts/` (durante l'esecuzione dalla riga di comando).

Per aggiungere un vCard alla raccolta in memoria dei contatti, inviare una richiesta Post all'URL stesso, con l'intestazione Content-Type "text/vcard" e con vCard testo nel corpo, formattato come illustrato nell'esempio precedente.
