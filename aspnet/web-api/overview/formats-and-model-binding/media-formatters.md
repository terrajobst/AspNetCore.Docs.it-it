---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Formattatori di Media in ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 824fea5a8837ff8b09af832b7d2a094c7d82907b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368916"
---
<a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="c3b38-102">Formattatori di Media in ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="c3b38-102">Media Formatters in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="c3b38-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c3b38-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c3b38-104">Questa esercitazione illustra come supportare altri formati multimediali nell'API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c3b38-104">This tutorial shows how to support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="c3b38-105">Tipi di supporto Internet</span><span class="sxs-lookup"><span data-stu-id="c3b38-105">Internet Media Types</span></span>

<span data-ttu-id="c3b38-106">Un tipo di supporto, detto anche tipo MIME, identifica il formato di una porzione di dati.</span><span class="sxs-lookup"><span data-stu-id="c3b38-106">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="c3b38-107">Tipi di supporti HTTP, descrivono il formato del corpo del messaggio.</span><span class="sxs-lookup"><span data-stu-id="c3b38-107">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="c3b38-108">Un tipo di supporto è costituito da due stringhe, un tipo e sottotipo.</span><span class="sxs-lookup"><span data-stu-id="c3b38-108">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="c3b38-109">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c3b38-109">For example:</span></span>

- <span data-ttu-id="c3b38-110">testo/html</span><span class="sxs-lookup"><span data-stu-id="c3b38-110">text/html</span></span>
- <span data-ttu-id="c3b38-111">image/png</span><span class="sxs-lookup"><span data-stu-id="c3b38-111">image/png</span></span>
- <span data-ttu-id="c3b38-112">application/json</span><span class="sxs-lookup"><span data-stu-id="c3b38-112">application/json</span></span>

<span data-ttu-id="c3b38-113">Quando un messaggio HTTP contiene un corpo di entità, l'intestazione Content-Type specifica il formato del corpo del messaggio.</span><span class="sxs-lookup"><span data-stu-id="c3b38-113">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="c3b38-114">Il ricevitore indica come analizzare il contenuto del corpo del messaggio.</span><span class="sxs-lookup"><span data-stu-id="c3b38-114">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="c3b38-115">Ad esempio, se una risposta HTTP contiene un'immagine PNG, la risposta contenga le intestazioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="c3b38-115">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="c3b38-116">Quando il client invia un messaggio di richiesta, può includere un'intestazione Accept.</span><span class="sxs-lookup"><span data-stu-id="c3b38-116">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="c3b38-117">L'intestazione Accept indica il server che supporti tipi del client richiede dal server.</span><span class="sxs-lookup"><span data-stu-id="c3b38-117">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="c3b38-118">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c3b38-118">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="c3b38-119">Questa intestazione indica al server che il client richiede HTML, XHTML o XML.</span><span class="sxs-lookup"><span data-stu-id="c3b38-119">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="c3b38-120">Il tipo di supporti determina come API Web serializza e deserializza il corpo del messaggio HTTP.</span><span class="sxs-lookup"><span data-stu-id="c3b38-120">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="c3b38-121">API Web offre supporto predefinito per XML, JSON, BSON e dati form-urlencoded ed è possibile supportare i tipi di supporto aggiuntive per la scrittura di un *formattatore di media*.</span><span class="sxs-lookup"><span data-stu-id="c3b38-121">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="c3b38-122">Per creare un formattatore di media, derivare da una di queste classi:</span><span class="sxs-lookup"><span data-stu-id="c3b38-122">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="c3b38-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="c3b38-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="c3b38-124">Questa lettura asincrona Usa classi e metodi di scrittura.</span><span class="sxs-lookup"><span data-stu-id="c3b38-124">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="c3b38-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="c3b38-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="c3b38-126">Questa classe deriva da **MediaTypeFormatter** ma vengono utilizzati i metodi di lettura/scrittura sychronous.</span><span class="sxs-lookup"><span data-stu-id="c3b38-126">This class derives from **MediaTypeFormatter** but uses sychronous read/write methods.</span></span>

<span data-ttu-id="c3b38-127">Che deriva da **BufferedMediaTypeFormatter** è più semplice, perché non è presente codice asincrono, ma significa anche durante il / o può bloccare il thread chiamante.</span><span class="sxs-lookup"><span data-stu-id="c3b38-127">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="c3b38-128">Esempio: Creazione di un formattatore di Media CSV</span><span class="sxs-lookup"><span data-stu-id="c3b38-128">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="c3b38-129">L'esempio seguente illustra un formattatore di media type che può serializzare un oggetto prodotto da un formato con valori delimitati da virgole (CSV).</span><span class="sxs-lookup"><span data-stu-id="c3b38-129">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="c3b38-130">Questo esempio viene usato il tipo di prodotto definito nell'esercitazione [creazione di un'API Web che supporta le operazioni CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span><span class="sxs-lookup"><span data-stu-id="c3b38-130">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="c3b38-131">Ecco la definizione dell'oggetto prodotto:</span><span class="sxs-lookup"><span data-stu-id="c3b38-131">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="c3b38-132">Per implementare un formattatore CSV, definire una classe che deriva da **BufferedMediaTypeFormater**:</span><span class="sxs-lookup"><span data-stu-id="c3b38-132">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormater**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="c3b38-133">Nel costruttore, aggiungere i tipi di supporti che supporta il formattatore.</span><span class="sxs-lookup"><span data-stu-id="c3b38-133">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="c3b38-134">In questo esempio, il formattatore supporta un tipo di supporto &quot;testo/csv&quot;:</span><span class="sxs-lookup"><span data-stu-id="c3b38-134">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="c3b38-135">Eseguire l'override di **CanWriteType** metodo per indicare quali tipi di formattatore in grado di serializzare:</span><span class="sxs-lookup"><span data-stu-id="c3b38-135">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="c3b38-136">In questo esempio, il formattatore in grado di serializzare single `Product` oggetti, oltre a raccolte di `Product` oggetti.</span><span class="sxs-lookup"><span data-stu-id="c3b38-136">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="c3b38-137">Analogamente, eseguire l'override di **CanReadType** metodo per indicare quali tipi di formattatore può deserializzare.</span><span class="sxs-lookup"><span data-stu-id="c3b38-137">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="c3b38-138">In questo esempio, il formattatore non supporta la deserializzazione, quindi il metodo restituisce semplicemente **false**.</span><span class="sxs-lookup"><span data-stu-id="c3b38-138">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="c3b38-139">Infine, eseguire l'override di **WriteToStream** (metodo).</span><span class="sxs-lookup"><span data-stu-id="c3b38-139">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="c3b38-140">Questo metodo serializza un tipo scrivendola su un flusso.</span><span class="sxs-lookup"><span data-stu-id="c3b38-140">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="c3b38-141">Se il formattatore supporta la deserializzazione, anche l'override di **ReadFromStream** (metodo).</span><span class="sxs-lookup"><span data-stu-id="c3b38-141">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="c3b38-142">Aggiunta di un formattatore di Media alla Pipeline API Web</span><span class="sxs-lookup"><span data-stu-id="c3b38-142">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="c3b38-143">Per aggiungere un tipo di supporto formattatore in modo che la pipeline di Web API, usare il **formattatori** proprietà di **HttpConfiguration** oggetto.</span><span class="sxs-lookup"><span data-stu-id="c3b38-143">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="c3b38-144">Codifiche dei caratteri</span><span class="sxs-lookup"><span data-stu-id="c3b38-144">Character Encodings</span></span>

<span data-ttu-id="c3b38-145">Facoltativamente, un formattatore di media può supportare più codifiche di carattere, ad esempio UTF-8 o ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="c3b38-145">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="c3b38-146">Nel costruttore, aggiungere uno o più [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) tipi per il **SupportedEncodings** raccolta.</span><span class="sxs-lookup"><span data-stu-id="c3b38-146">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="c3b38-147">Inserire il valore predefinito prima di codifica.</span><span class="sxs-lookup"><span data-stu-id="c3b38-147">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="c3b38-148">Nel **WriteToStream** e **ReadFromStream** metodi, chiamare [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) per selezionare la codifica dei caratteri preferita.</span><span class="sxs-lookup"><span data-stu-id="c3b38-148">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="c3b38-149">Questo metodo associa le intestazioni della richiesta rispetto all'elenco di codifiche supportate.</span><span class="sxs-lookup"><span data-stu-id="c3b38-149">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="c3b38-150">Utilizzare l'oggetto restituito **Encoding** quando è leggere o scrivere dal flusso:</span><span class="sxs-lookup"><span data-stu-id="c3b38-150">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
