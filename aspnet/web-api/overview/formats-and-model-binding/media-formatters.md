---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Formattatori di Media in ASP.NET Web API 2 | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 9103574597df126a22e21a2f51815f608e46f47f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="media-formatters-in-aspnet-web-api-2"></a>Formattatori di Media in ASP.NET Web API 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

Questa esercitazione illustra come supporta altri formati multimediali in ASP.NET Web API.

## <a name="internet-media-types"></a>Tipi di supporto Internet

Un tipo di supporto, detto anche un tipo MIME, identifica il formato di una porzione di dati. In HTTP, i tipi di supporto vengono descritti il formato del corpo del messaggio. Un tipo di supporto è costituito da due stringhe, un tipo e sottotipo. Ad esempio:

- testo/html
- image/png
- application/json

Quando un messaggio HTTP contiene un corpo dell'entità, l'intestazione Content-Type: Specifica il formato del corpo del messaggio. Questo valore indica il destinatario come analizzare il contenuto del corpo del messaggio.

Ad esempio, se una risposta HTTP contiene un'immagine PNG, la risposta potrebbe essere le intestazioni seguenti.

[!code-console[Main](media-formatters/samples/sample1.cmd)]

Quando il client invia un messaggio di richiesta, è possibile includere un'intestazione Accept. L'intestazione Accept indica il server che supporti tipi il client eseguirà dal server. Ad esempio:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

Questa intestazione indica al server che il client richiede HTML, XHTML o XML.

Il tipo di supporto determina come API Web serializza e deserializza il corpo del messaggio HTTP. API Web include il supporto incorporato per XML, JSON, BSON e dati di form-urlencoded e si possono supportare i tipi di supporto aggiuntive scrivendo un *formattatore di media*.

Per creare un formattatore di media, derivare da una di queste classi:

- [MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx). Questa classe viene utilizzata della lettura asincrona e metodi di scrittura.
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). Questa classe deriva da **MediaTypeFormatter** ma vengono utilizzati i metodi di lettura/scrittura sychronous.

Derivazione da **BufferedMediaTypeFormatter** è più semplice, perché non è presente codice asincrono, ma significa inoltre possibile bloccare il thread chiamante durante il / o.

## <a name="example-creating-a-csv-media-formatter"></a>Esempio: Creazione di un formattatore di Media CSV

Nell'esempio seguente viene illustrato un formattatore di media type che può serializzare un oggetto di prodotto in un formato con valori delimitati da virgole (CSV). Questo esempio viene utilizzato il tipo di prodotto definito in questa esercitazione [la creazione di un'API Web che supporta le operazioni CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md). Di seguito è riportata la definizione dell'oggetto prodotto:

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

Per implementare un formattatore CSV, definire una classe che deriva da **BufferedMediaTypeFormater**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

Nel costruttore, aggiungere i tipi di supporto che supporta il formattatore. In questo esempio, il formattatore supporta un solo tipo di supporto, &quot;testo/csv&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

Eseguire l'override di **CanWriteType** metodo per indicare quali tipi al formattatore in grado di serializzare:

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

In questo esempio, il formattatore in grado di serializzare solo `Product` oggetti e raccolte di `Product` oggetti.

Analogamente, eseguire l'override di **CanReadType** può deserializzare il metodo per indicare quali tipi al formattatore. In questo esempio, il formattatore supporta la deserializzazione, il metodo restituisce semplicemente **false**.

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

Infine, eseguire l'override di **WriteToStream** metodo. Questo metodo serializza un tipo scrivendolo in un flusso. Se il formattatore supporta la deserializzazione, anche l'override di **ReadFromStream** metodo.

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Aggiunta di un formattatore di Media per la Pipeline di Web API

Per aggiungere un tipo di supporto del formattatore per la pipeline di Web API, utilizzare il **formattatori** proprietà il **HttpConfiguration** oggetto.

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>Codifiche di caratteri

Facoltativamente, un formattatore di media può supportare più codifiche di caratteri, ad esempio UTF-8 o ISO 8859-1.

Nel costruttore, aggiungere uno o più [Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) i tipi di **SupportedEncodings** insieme. Inserire il valore predefinito prima di codifica.

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

Nel **WriteToStream** e **ReadFromStream** chiamare metodi, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) per selezionare la codifica dei caratteri preferita. Questo metodo consente di ricercare le intestazioni della richiesta rispetto all'elenco di codifiche supportate. Utilizzare l'oggetto restituito **codifica** quando si leggere o scrivere dal flusso:

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
