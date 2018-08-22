---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Formattatori di Media in ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 7b7ba2fb3f1bba0447e700c84a017266cba305e6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826186"
---
<a name="media-formatters-in-aspnet-web-api-2"></a>Formattatori di Media in ASP.NET Web API 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

Questa esercitazione illustra come supportare altri formati multimediali nell'API Web ASP.NET.

## <a name="internet-media-types"></a>Tipi di supporto Internet

Un tipo di supporto, detto anche tipo MIME, identifica il formato di una porzione di dati. Tipi di supporti HTTP, descrivono il formato del corpo del messaggio. Un tipo di supporto è costituito da due stringhe, un tipo e sottotipo. Ad esempio:

- testo/html
- image/png
- application/json

Quando un messaggio HTTP contiene un corpo di entità, l'intestazione Content-Type specifica il formato del corpo del messaggio. Il ricevitore indica come analizzare il contenuto del corpo del messaggio.

Ad esempio, se una risposta HTTP contiene un'immagine PNG, la risposta contenga le intestazioni seguenti.

[!code-console[Main](media-formatters/samples/sample1.cmd)]

Quando il client invia un messaggio di richiesta, può includere un'intestazione Accept. L'intestazione Accept indica il server che supporti tipi del client richiede dal server. Ad esempio:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

Questa intestazione indica al server che il client richiede HTML, XHTML o XML.

Il tipo di supporti determina come API Web serializza e deserializza il corpo del messaggio HTTP. API Web offre supporto predefinito per XML, JSON, BSON e dati form-urlencoded ed è possibile supportare i tipi di supporto aggiuntive per la scrittura di un *formattatore di media*.

Per creare un formattatore di media, derivare da una di queste classi:

- [MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx). Questa lettura asincrona Usa classi e metodi di scrittura.
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). Questa classe deriva da **MediaTypeFormatter** ma vengono utilizzati i metodi di lettura/scrittura sychronous.

Che deriva da **BufferedMediaTypeFormatter** è più semplice, perché non è presente codice asincrono, ma significa anche durante il / o può bloccare il thread chiamante.

## <a name="example-creating-a-csv-media-formatter"></a>Esempio: Creazione di un formattatore di Media CSV

L'esempio seguente illustra un formattatore di media type che può serializzare un oggetto prodotto da un formato con valori delimitati da virgole (CSV). Questo esempio viene usato il tipo di prodotto definito nell'esercitazione [creazione di un'API Web che supporta le operazioni CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md). Ecco la definizione dell'oggetto prodotto:

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

Per implementare un formattatore CSV, definire una classe che deriva da **BufferedMediaTypeFormater**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

Nel costruttore, aggiungere i tipi di supporti che supporta il formattatore. In questo esempio, il formattatore supporta un tipo di supporto &quot;testo/csv&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

Eseguire l'override di **CanWriteType** metodo per indicare quali tipi di formattatore in grado di serializzare:

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

In questo esempio, il formattatore in grado di serializzare single `Product` oggetti, oltre a raccolte di `Product` oggetti.

Analogamente, eseguire l'override di **CanReadType** metodo per indicare quali tipi di formattatore può deserializzare. In questo esempio, il formattatore non supporta la deserializzazione, quindi il metodo restituisce semplicemente **false**.

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

Infine, eseguire l'override di **WriteToStream** (metodo). Questo metodo serializza un tipo scrivendola su un flusso. Se il formattatore supporta la deserializzazione, anche l'override di **ReadFromStream** (metodo).

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Aggiunta di un formattatore di Media alla Pipeline API Web

Per aggiungere un tipo di supporto formattatore in modo che la pipeline di Web API, usare il **formattatori** proprietà di **HttpConfiguration** oggetto.

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>Codifiche dei caratteri

Facoltativamente, un formattatore di media può supportare più codifiche di carattere, ad esempio UTF-8 o ISO 8859-1.

Nel costruttore, aggiungere uno o più [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) tipi per il **SupportedEncodings** raccolta. Inserire il valore predefinito prima di codifica.

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

Nel **WriteToStream** e **ReadFromStream** metodi, chiamare [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) per selezionare la codifica dei caratteri preferita. Questo metodo associa le intestazioni della richiesta rispetto all'elenco di codifiche supportate. Utilizzare l'oggetto restituito **Encoding** quando è leggere o scrivere dal flusso:

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
