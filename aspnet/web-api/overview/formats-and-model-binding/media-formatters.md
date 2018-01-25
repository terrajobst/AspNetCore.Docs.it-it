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
<a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="71719-102">Formattatori di Media in ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="71719-102">Media Formatters in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="71719-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="71719-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="71719-104">Questa esercitazione illustra come supporta altri formati multimediali in ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="71719-104">This tutorial shows how support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="71719-105">Tipi di supporto Internet</span><span class="sxs-lookup"><span data-stu-id="71719-105">Internet Media Types</span></span>

<span data-ttu-id="71719-106">Un tipo di supporto, detto anche un tipo MIME, identifica il formato di una porzione di dati.</span><span class="sxs-lookup"><span data-stu-id="71719-106">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="71719-107">In HTTP, i tipi di supporto vengono descritti il formato del corpo del messaggio.</span><span class="sxs-lookup"><span data-stu-id="71719-107">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="71719-108">Un tipo di supporto è costituito da due stringhe, un tipo e sottotipo.</span><span class="sxs-lookup"><span data-stu-id="71719-108">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="71719-109">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="71719-109">For example:</span></span>

- <span data-ttu-id="71719-110">testo/html</span><span class="sxs-lookup"><span data-stu-id="71719-110">text/html</span></span>
- <span data-ttu-id="71719-111">image/png</span><span class="sxs-lookup"><span data-stu-id="71719-111">image/png</span></span>
- <span data-ttu-id="71719-112">application/json</span><span class="sxs-lookup"><span data-stu-id="71719-112">application/json</span></span>

<span data-ttu-id="71719-113">Quando un messaggio HTTP contiene un corpo dell'entità, l'intestazione Content-Type: Specifica il formato del corpo del messaggio.</span><span class="sxs-lookup"><span data-stu-id="71719-113">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="71719-114">Questo valore indica il destinatario come analizzare il contenuto del corpo del messaggio.</span><span class="sxs-lookup"><span data-stu-id="71719-114">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="71719-115">Ad esempio, se una risposta HTTP contiene un'immagine PNG, la risposta potrebbe essere le intestazioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="71719-115">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="71719-116">Quando il client invia un messaggio di richiesta, è possibile includere un'intestazione Accept.</span><span class="sxs-lookup"><span data-stu-id="71719-116">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="71719-117">L'intestazione Accept indica il server che supporti tipi il client eseguirà dal server.</span><span class="sxs-lookup"><span data-stu-id="71719-117">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="71719-118">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="71719-118">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="71719-119">Questa intestazione indica al server che il client richiede HTML, XHTML o XML.</span><span class="sxs-lookup"><span data-stu-id="71719-119">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="71719-120">Il tipo di supporto determina come API Web serializza e deserializza il corpo del messaggio HTTP.</span><span class="sxs-lookup"><span data-stu-id="71719-120">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="71719-121">API Web include il supporto incorporato per XML, JSON, BSON e dati di form-urlencoded e si possono supportare i tipi di supporto aggiuntive scrivendo un *formattatore di media*.</span><span class="sxs-lookup"><span data-stu-id="71719-121">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="71719-122">Per creare un formattatore di media, derivare da una di queste classi:</span><span class="sxs-lookup"><span data-stu-id="71719-122">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="71719-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="71719-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="71719-124">Questa classe viene utilizzata della lettura asincrona e metodi di scrittura.</span><span class="sxs-lookup"><span data-stu-id="71719-124">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="71719-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="71719-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="71719-126">Questa classe deriva da **MediaTypeFormatter** ma vengono utilizzati i metodi di lettura/scrittura sychronous.</span><span class="sxs-lookup"><span data-stu-id="71719-126">This class derives from **MediaTypeFormatter** but uses sychronous read/write methods.</span></span>

<span data-ttu-id="71719-127">Derivazione da **BufferedMediaTypeFormatter** è più semplice, perché non è presente codice asincrono, ma significa inoltre possibile bloccare il thread chiamante durante il / o.</span><span class="sxs-lookup"><span data-stu-id="71719-127">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="71719-128">Esempio: Creazione di un formattatore di Media CSV</span><span class="sxs-lookup"><span data-stu-id="71719-128">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="71719-129">Nell'esempio seguente viene illustrato un formattatore di media type che può serializzare un oggetto di prodotto in un formato con valori delimitati da virgole (CSV).</span><span class="sxs-lookup"><span data-stu-id="71719-129">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="71719-130">Questo esempio viene utilizzato il tipo di prodotto definito in questa esercitazione [la creazione di un'API Web che supporta le operazioni CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span><span class="sxs-lookup"><span data-stu-id="71719-130">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="71719-131">Di seguito è riportata la definizione dell'oggetto prodotto:</span><span class="sxs-lookup"><span data-stu-id="71719-131">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="71719-132">Per implementare un formattatore CSV, definire una classe che deriva da **BufferedMediaTypeFormater**:</span><span class="sxs-lookup"><span data-stu-id="71719-132">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormater**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="71719-133">Nel costruttore, aggiungere i tipi di supporto che supporta il formattatore.</span><span class="sxs-lookup"><span data-stu-id="71719-133">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="71719-134">In questo esempio, il formattatore supporta un solo tipo di supporto, &quot;testo/csv&quot;:</span><span class="sxs-lookup"><span data-stu-id="71719-134">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="71719-135">Eseguire l'override di **CanWriteType** metodo per indicare quali tipi al formattatore in grado di serializzare:</span><span class="sxs-lookup"><span data-stu-id="71719-135">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="71719-136">In questo esempio, il formattatore in grado di serializzare solo `Product` oggetti e raccolte di `Product` oggetti.</span><span class="sxs-lookup"><span data-stu-id="71719-136">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="71719-137">Analogamente, eseguire l'override di **CanReadType** può deserializzare il metodo per indicare quali tipi al formattatore.</span><span class="sxs-lookup"><span data-stu-id="71719-137">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="71719-138">In questo esempio, il formattatore supporta la deserializzazione, il metodo restituisce semplicemente **false**.</span><span class="sxs-lookup"><span data-stu-id="71719-138">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="71719-139">Infine, eseguire l'override di **WriteToStream** metodo.</span><span class="sxs-lookup"><span data-stu-id="71719-139">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="71719-140">Questo metodo serializza un tipo scrivendolo in un flusso.</span><span class="sxs-lookup"><span data-stu-id="71719-140">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="71719-141">Se il formattatore supporta la deserializzazione, anche l'override di **ReadFromStream** metodo.</span><span class="sxs-lookup"><span data-stu-id="71719-141">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="71719-142">Aggiunta di un formattatore di Media per la Pipeline di Web API</span><span class="sxs-lookup"><span data-stu-id="71719-142">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="71719-143">Per aggiungere un tipo di supporto del formattatore per la pipeline di Web API, utilizzare il **formattatori** proprietà il **HttpConfiguration** oggetto.</span><span class="sxs-lookup"><span data-stu-id="71719-143">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="71719-144">Codifiche di caratteri</span><span class="sxs-lookup"><span data-stu-id="71719-144">Character Encodings</span></span>

<span data-ttu-id="71719-145">Facoltativamente, un formattatore di media può supportare più codifiche di caratteri, ad esempio UTF-8 o ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="71719-145">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="71719-146">Nel costruttore, aggiungere uno o più [Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) i tipi di **SupportedEncodings** insieme.</span><span class="sxs-lookup"><span data-stu-id="71719-146">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="71719-147">Inserire il valore predefinito prima di codifica.</span><span class="sxs-lookup"><span data-stu-id="71719-147">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="71719-148">Nel **WriteToStream** e **ReadFromStream** chiamare metodi, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) per selezionare la codifica dei caratteri preferita.</span><span class="sxs-lookup"><span data-stu-id="71719-148">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="71719-149">Questo metodo consente di ricercare le intestazioni della richiesta rispetto all'elenco di codifiche supportate.</span><span class="sxs-lookup"><span data-stu-id="71719-149">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="71719-150">Utilizzare l'oggetto restituito **codifica** quando si leggere o scrivere dal flusso:</span><span class="sxs-lookup"><span data-stu-id="71719-150">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
