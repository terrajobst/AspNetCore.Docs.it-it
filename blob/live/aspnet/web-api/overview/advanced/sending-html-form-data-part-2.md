---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Invia i dati di Form HTML in ASP.NET Web API: Multipart MIME e il caricamento del File | Documenti Microsoft'
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 3df59aab2a0c43f4a4f5c59530b0655f68d95cc7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a><span data-ttu-id="42ead-102">Invia i dati di Form HTML in ASP.NET Web API: Multipart MIME e il caricamento di File</span><span class="sxs-lookup"><span data-stu-id="42ead-102">Sending HTML Form Data in ASP.NET Web API: File Upload and Multipart MIME</span></span>
====================
<span data-ttu-id="42ead-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="42ead-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-2-file-upload-and-multipart-mime"></a><span data-ttu-id="42ead-104">Parte 2: Caricamento del File e in più parti MIME</span><span class="sxs-lookup"><span data-stu-id="42ead-104">Part 2: File Upload and Multipart MIME</span></span>

<span data-ttu-id="42ead-105">In questa esercitazione viene illustrato come caricare i file in un'API web.</span><span class="sxs-lookup"><span data-stu-id="42ead-105">This tutorial shows how to upload files to a web API.</span></span> <span data-ttu-id="42ead-106">Viene inoltre descritto come elaborare i dati MIME multipart.</span><span class="sxs-lookup"><span data-stu-id="42ead-106">It also describes how to process multipart MIME data.</span></span>

> [!NOTE]
> <span data-ttu-id="42ead-107">[Scaricare il progetto completato](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span><span class="sxs-lookup"><span data-stu-id="42ead-107">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span></span>


<span data-ttu-id="42ead-108">Di seguito è riportato un esempio di un form HTML per il caricamento di un file:</span><span class="sxs-lookup"><span data-stu-id="42ead-108">Here is an example of an HTML form for uploading a file:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

<span data-ttu-id="42ead-109">Questo modulo contiene un controllo input di testo e un controllo input di file.</span><span class="sxs-lookup"><span data-stu-id="42ead-109">This form contains a text input control and a file input control.</span></span> <span data-ttu-id="42ead-110">Quando un modulo contiene un controllo input di file, il **enctype** attributo deve essere sempre &quot;multipart/form-data&quot;, che specifica che il modulo verrà inviato come messaggio MIME multiparte.</span><span class="sxs-lookup"><span data-stu-id="42ead-110">When a form contains a file input control, the **enctype** attribute should always be &quot;multipart/form-data&quot;, which specifies that the form will be sent as a multipart MIME message.</span></span>

<span data-ttu-id="42ead-111">Il formato di un messaggio MIME multiparte è più facile da comprendere esaminando un esempio di richiesta:</span><span class="sxs-lookup"><span data-stu-id="42ead-111">The format of a multipart MIME message is easiest to understand by looking at an example request:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

<span data-ttu-id="42ead-112">Questo messaggio viene diviso in due *parti*, uno per ogni controllo del form.</span><span class="sxs-lookup"><span data-stu-id="42ead-112">This message is divided into two *parts*, one for each form control.</span></span> <span data-ttu-id="42ead-113">Per le righe che iniziano con trattini sono indicati i contorni della parte.</span><span class="sxs-lookup"><span data-stu-id="42ead-113">Part boundaries are indicated by the lines that start with dashes.</span></span>

> [!NOTE]
> <span data-ttu-id="42ead-114">Il limite di parte include un componente casuale (&quot;41184676334&quot;) per garantire che la stringa di delimitazione viene accidentalmente visualizzato all'interno di una parte del messaggio.</span><span class="sxs-lookup"><span data-stu-id="42ead-114">The part boundary includes a random component (&quot;41184676334&quot;) to ensure that the boundary string does not accidentally appear inside a message part.</span></span>


<span data-ttu-id="42ead-115">Ogni parte del messaggio contiene una o più intestazioni, seguite dal contenuto di parte.</span><span class="sxs-lookup"><span data-stu-id="42ead-115">Each message part contains one or more headers, followed by the part contents.</span></span>

- <span data-ttu-id="42ead-116">L'intestazione Content-Disposition include il nome del controllo.</span><span class="sxs-lookup"><span data-stu-id="42ead-116">The Content-Disposition header includes the name of the control.</span></span> <span data-ttu-id="42ead-117">Per i file, nonché il nome del file.</span><span class="sxs-lookup"><span data-stu-id="42ead-117">For files, it also contains the file name.</span></span>
- <span data-ttu-id="42ead-118">L'intestazione Content-Type descrive i dati nella parte.</span><span class="sxs-lookup"><span data-stu-id="42ead-118">The Content-Type header describes the data in the part.</span></span> <span data-ttu-id="42ead-119">Se questa intestazione viene omesso, il valore predefinito è text/plain.</span><span class="sxs-lookup"><span data-stu-id="42ead-119">If this header is omitted, the default is text/plain.</span></span>

<span data-ttu-id="42ead-120">Nell'esempio precedente, l'utente è caricato un file denominato GrandCanyon.jpg, con tipo di contenuto image/jpeg; e il valore del testo di input è &quot;ferie estate&quot;.</span><span class="sxs-lookup"><span data-stu-id="42ead-120">In the previous example, the user uploaded a file named GrandCanyon.jpg, with content type image/jpeg; and the value of the text input was &quot;Summer Vacation&quot;.</span></span>

## <a name="file-upload"></a><span data-ttu-id="42ead-121">Caricamento del file</span><span class="sxs-lookup"><span data-stu-id="42ead-121">File Upload</span></span>

<span data-ttu-id="42ead-122">Ora esaminiamo controller API Web che legge i file da un messaggio MIME multiparte.</span><span class="sxs-lookup"><span data-stu-id="42ead-122">Now let's look at a Web API controller that reads files from a multipart MIME message.</span></span> <span data-ttu-id="42ead-123">Il controller verrà letti i file in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="42ead-123">The controller will read the files asynchronously.</span></span> <span data-ttu-id="42ead-124">API Web supporta azioni asincrone utilizzando il [il modello di programmazione basato su attività](https://msdn.microsoft.com/library/dd460693.aspx).</span><span class="sxs-lookup"><span data-stu-id="42ead-124">Web API supports asynchronous actions using the [task-based programming model](https://msdn.microsoft.com/library/dd460693.aspx).</span></span> <span data-ttu-id="42ead-125">In primo luogo, ecco il codice se la destinazione è .NET Framework 4.5, che supporta il **async** e **await** parole chiave.</span><span class="sxs-lookup"><span data-stu-id="42ead-125">First, here is the code if you are targeting .NET Framework 4.5, which supports the **async** and **await** keywords.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

<span data-ttu-id="42ead-126">Si noti che l'azione del controller non accetta parametri.</span><span class="sxs-lookup"><span data-stu-id="42ead-126">Notice that the controller action does not take any parameters.</span></span> <span data-ttu-id="42ead-127">Ciò avviene perché si elabora il corpo di richiesta all'interno dell'azione, senza richiamare un formattatore di media type.</span><span class="sxs-lookup"><span data-stu-id="42ead-127">That's because we process the request body inside the action, without invoking a media-type formatter.</span></span>

<span data-ttu-id="42ead-128">Il **IsMultipartContent** metodo controlla se la richiesta contiene un messaggio MIME multiparte.</span><span class="sxs-lookup"><span data-stu-id="42ead-128">The **IsMultipartContent** method checks whether the request contains a multipart MIME message.</span></span> <span data-ttu-id="42ead-129">In caso contrario, il controller restituisce il codice di stato HTTP 415 (tipo di supporto non supportato).</span><span class="sxs-lookup"><span data-stu-id="42ead-129">If not, the controller returns HTTP status code 415 (Unsupported Media Type).</span></span>

<span data-ttu-id="42ead-130">Il **MultipartFormDataStreamProvider** classe è un oggetto helper che consente di allocare i flussi di file per i file caricati.</span><span class="sxs-lookup"><span data-stu-id="42ead-130">The **MultipartFormDataStreamProvider** class is a helper object that allocates file streams for uploaded files.</span></span> <span data-ttu-id="42ead-131">Per leggere il messaggio MIME multiparte, chiamare il **ReadAsMultipartAsync** metodo.</span><span class="sxs-lookup"><span data-stu-id="42ead-131">To read the multipart MIME message, call the **ReadAsMultipartAsync** method.</span></span> <span data-ttu-id="42ead-132">Questo metodo estrae tutte le parti del messaggio e li scrive in dei flussi forniti dal **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="42ead-132">This method extracts all of the message parts and writes them into the streams provided by the **MultipartFormDataStreamProvider**.</span></span>

<span data-ttu-id="42ead-133">Quando il metodo viene completato, è possibile ottenere informazioni sui file dal **FileData** proprietà, che è una raccolta di **MultipartFileData** oggetti.</span><span class="sxs-lookup"><span data-stu-id="42ead-133">When the method completes, you can get information about the files from the **FileData** property, which is a collection of **MultipartFileData** objects.</span></span>

- <span data-ttu-id="42ead-134">**MultipartFileData.FileName** è il nome del file locale nel server, in cui è stato salvato il file.</span><span class="sxs-lookup"><span data-stu-id="42ead-134">**MultipartFileData.FileName** is the local file name on the server, where the file was saved.</span></span>
- <span data-ttu-id="42ead-135">**MultipartFileData.Headers** contiene l'intestazione di parte (*non* l'intestazione della richiesta).</span><span class="sxs-lookup"><span data-stu-id="42ead-135">**MultipartFileData.Headers** contains the part header (*not* the request header).</span></span> <span data-ttu-id="42ead-136">Ciò consente di accedere al contenuto\_intestazioni Disposition e Content-Type.</span><span class="sxs-lookup"><span data-stu-id="42ead-136">You can use this to access the Content\_Disposition and Content-Type headers.</span></span>

<span data-ttu-id="42ead-137">Come suggerisce il nome, **ReadAsMultipartAsync** è un metodo asincrono.</span><span class="sxs-lookup"><span data-stu-id="42ead-137">As the name suggests, **ReadAsMultipartAsync** is an asynchronous method.</span></span> <span data-ttu-id="42ead-138">Per eseguire operazioni dopo il completamento del metodo, utilizzare un [attività di continuazione](https://msdn.microsoft.com/en-us/library/ee372288.aspx) (.NET 4.0) o **await** (parola chiave) (.NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="42ead-138">To perform work after the method completes, use a [continuation task](https://msdn.microsoft.com/en-us/library/ee372288.aspx) (.NET 4.0) or the **await** keyword (.NET 4.5).</span></span>

<span data-ttu-id="42ead-139">Di seguito è la versione di .NET Framework 4.0 del codice precedente:</span><span class="sxs-lookup"><span data-stu-id="42ead-139">Here is the .NET Framework 4.0 version of the previous code:</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a><span data-ttu-id="42ead-140">Lettura di dati di controllo Form</span><span class="sxs-lookup"><span data-stu-id="42ead-140">Reading Form Control Data</span></span>

<span data-ttu-id="42ead-141">Il form HTML che illustrato in precedenza era un controllo input di testo.</span><span class="sxs-lookup"><span data-stu-id="42ead-141">The HTML form that I showed earlier had a text input control.</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

<span data-ttu-id="42ead-142">È possibile ottenere il valore del controllo dal **FormData** proprietà del **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="42ead-142">You can get the value of the control from the **FormData** property of the **MultipartFormDataStreamProvider**.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

<span data-ttu-id="42ead-143">**FormData** è un **NameValueCollection** che contiene coppie nome/valore per i controlli del form.</span><span class="sxs-lookup"><span data-stu-id="42ead-143">**FormData** is a **NameValueCollection** that contains name/value pairs for the form controls.</span></span> <span data-ttu-id="42ead-144">La raccolta può contenere chiavi duplicate.</span><span class="sxs-lookup"><span data-stu-id="42ead-144">The collection can contain duplicate keys.</span></span> <span data-ttu-id="42ead-145">Si consideri questo formato:</span><span class="sxs-lookup"><span data-stu-id="42ead-145">Consider this form:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

<span data-ttu-id="42ead-146">Il corpo della richiesta potrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="42ead-146">The request body might look like this:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

<span data-ttu-id="42ead-147">In tal caso, il **FormData** insieme contiene le coppie chiave/valore seguenti:</span><span class="sxs-lookup"><span data-stu-id="42ead-147">In that case, the **FormData** collection would contain the following key/value pairs:</span></span>

- <span data-ttu-id="42ead-148">viaggi: round trip</span><span class="sxs-lookup"><span data-stu-id="42ead-148">trip: round-trip</span></span>
- <span data-ttu-id="42ead-149">opzioni: fino</span><span class="sxs-lookup"><span data-stu-id="42ead-149">options: nonstop</span></span>
- <span data-ttu-id="42ead-150">opzioni: date</span><span class="sxs-lookup"><span data-stu-id="42ead-150">options: dates</span></span>
- <span data-ttu-id="42ead-151">postazione: finestra</span><span class="sxs-lookup"><span data-stu-id="42ead-151">seat: window</span></span>
