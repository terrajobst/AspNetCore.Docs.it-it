---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: "L'invio di dati di Form HTML nell'API Web ASP.NET: caricamento di File e MIME Multipart | Microsoft Docs"
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 7c1c85f462141daf747e23aa4215d47f2d263140
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386708"
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a><span data-ttu-id="b0528-102">L'invio di dati di Form HTML nell'API Web ASP.NET: caricamento di File e MIME Multipart</span><span class="sxs-lookup"><span data-stu-id="b0528-102">Sending HTML Form Data in ASP.NET Web API: File Upload and Multipart MIME</span></span>
====================
<span data-ttu-id="b0528-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b0528-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-2-file-upload-and-multipart-mime"></a><span data-ttu-id="b0528-104">Parte 2: Caricamento di File e MIME Multipart</span><span class="sxs-lookup"><span data-stu-id="b0528-104">Part 2: File Upload and Multipart MIME</span></span>

<span data-ttu-id="b0528-105">Questa esercitazione illustra come caricare i file a un'API web.</span><span class="sxs-lookup"><span data-stu-id="b0528-105">This tutorial shows how to upload files to a web API.</span></span> <span data-ttu-id="b0528-106">Viene anche descritto come elaborare i dati MIME multipart.</span><span class="sxs-lookup"><span data-stu-id="b0528-106">It also describes how to process multipart MIME data.</span></span>

> [!NOTE]
> <span data-ttu-id="b0528-107">[Scaricare il progetto completato](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span><span class="sxs-lookup"><span data-stu-id="b0528-107">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span></span>


<span data-ttu-id="b0528-108">Ecco un esempio di un form HTML per il caricamento di un file:</span><span class="sxs-lookup"><span data-stu-id="b0528-108">Here is an example of an HTML form for uploading a file:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

<span data-ttu-id="b0528-109">Questo modulo contiene un controllo input di testo e un controllo input di file.</span><span class="sxs-lookup"><span data-stu-id="b0528-109">This form contains a text input control and a file input control.</span></span> <span data-ttu-id="b0528-110">Quando un modulo contiene un controllo input di file, il **enctype** attributo deve essere sempre &quot;multipart/form-data&quot;, che specifica che verrà inviato il form come un messaggio MIME multiparte.</span><span class="sxs-lookup"><span data-stu-id="b0528-110">When a form contains a file input control, the **enctype** attribute should always be &quot;multipart/form-data&quot;, which specifies that the form will be sent as a multipart MIME message.</span></span>

<span data-ttu-id="b0528-111">Il formato di un messaggio MIME multiparte è più facile da comprendere osservando un esempio di richiesta:</span><span class="sxs-lookup"><span data-stu-id="b0528-111">The format of a multipart MIME message is easiest to understand by looking at an example request:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

<span data-ttu-id="b0528-112">Questo messaggio viene diviso in due *parti*, uno per ogni controllo del modulo.</span><span class="sxs-lookup"><span data-stu-id="b0528-112">This message is divided into two *parts*, one for each form control.</span></span> <span data-ttu-id="b0528-113">Sono indicati i limiti di parte dalle righe che iniziano con i trattini.</span><span class="sxs-lookup"><span data-stu-id="b0528-113">Part boundaries are indicated by the lines that start with dashes.</span></span>

> [!NOTE]
> <span data-ttu-id="b0528-114">Il limite di ambito include un componente casuale (&quot;41184676334&quot;) per garantire che la stringa di delimitazione non viene visualizzata per errore all'interno di una parte del messaggio.</span><span class="sxs-lookup"><span data-stu-id="b0528-114">The part boundary includes a random component (&quot;41184676334&quot;) to ensure that the boundary string does not accidentally appear inside a message part.</span></span>


<span data-ttu-id="b0528-115">Ogni parte del messaggio contiene una o più intestazioni, seguite dal contenuto di parte.</span><span class="sxs-lookup"><span data-stu-id="b0528-115">Each message part contains one or more headers, followed by the part contents.</span></span>

- <span data-ttu-id="b0528-116">L'intestazione Content-Disposition include il nome del controllo.</span><span class="sxs-lookup"><span data-stu-id="b0528-116">The Content-Disposition header includes the name of the control.</span></span> <span data-ttu-id="b0528-117">Per i file, nonché il nome del file.</span><span class="sxs-lookup"><span data-stu-id="b0528-117">For files, it also contains the file name.</span></span>
- <span data-ttu-id="b0528-118">L'intestazione Content-Type descrive i dati nella parte.</span><span class="sxs-lookup"><span data-stu-id="b0528-118">The Content-Type header describes the data in the part.</span></span> <span data-ttu-id="b0528-119">Se questa intestazione viene omesso, il valore predefinito è text/plain.</span><span class="sxs-lookup"><span data-stu-id="b0528-119">If this header is omitted, the default is text/plain.</span></span>

<span data-ttu-id="b0528-120">Nell'esempio precedente, l'utente ha caricato un file denominato GrandCanyon.jpg, con tipo di contenuto image/jpeg; e il valore del testo di input è stata &quot;estive&quot;.</span><span class="sxs-lookup"><span data-stu-id="b0528-120">In the previous example, the user uploaded a file named GrandCanyon.jpg, with content type image/jpeg; and the value of the text input was &quot;Summer Vacation&quot;.</span></span>

## <a name="file-upload"></a><span data-ttu-id="b0528-121">Caricamento di file</span><span class="sxs-lookup"><span data-stu-id="b0528-121">File Upload</span></span>

<span data-ttu-id="b0528-122">A questo punto verrà ora esaminato un controller API Web che legge i file da un messaggio MIME multiparte.</span><span class="sxs-lookup"><span data-stu-id="b0528-122">Now let's look at a Web API controller that reads files from a multipart MIME message.</span></span> <span data-ttu-id="b0528-123">Il controller leggerà i file in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="b0528-123">The controller will read the files asynchronously.</span></span> <span data-ttu-id="b0528-124">API Web supporta le azioni asincrone usando il [modello di programmazione basato su attività](https://msdn.microsoft.com/library/dd460693.aspx).</span><span class="sxs-lookup"><span data-stu-id="b0528-124">Web API supports asynchronous actions using the [task-based programming model](https://msdn.microsoft.com/library/dd460693.aspx).</span></span> <span data-ttu-id="b0528-125">In primo luogo, ecco il codice se la destinazione è .NET Framework 4.5, che supporta il **async** e **await** parole chiave.</span><span class="sxs-lookup"><span data-stu-id="b0528-125">First, here is the code if you are targeting .NET Framework 4.5, which supports the **async** and **await** keywords.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

<span data-ttu-id="b0528-126">Si noti che l'azione del controller non accetta alcun parametro.</span><span class="sxs-lookup"><span data-stu-id="b0528-126">Notice that the controller action does not take any parameters.</span></span> <span data-ttu-id="b0528-127">Ciò avviene perché si elabora il corpo di richiesta nell'azione, senza richiamare un formattatore di media type.</span><span class="sxs-lookup"><span data-stu-id="b0528-127">That's because we process the request body inside the action, without invoking a media-type formatter.</span></span>

<span data-ttu-id="b0528-128">Il **IsMultipartContent** metodo controlla se la richiesta contiene un messaggio MIME multiparte.</span><span class="sxs-lookup"><span data-stu-id="b0528-128">The **IsMultipartContent** method checks whether the request contains a multipart MIME message.</span></span> <span data-ttu-id="b0528-129">In caso contrario, il controller restituisce il codice di stato HTTP 415 (tipo di supporto non supportato).</span><span class="sxs-lookup"><span data-stu-id="b0528-129">If not, the controller returns HTTP status code 415 (Unsupported Media Type).</span></span>

<span data-ttu-id="b0528-130">Il **MultipartFormDataStreamProvider** classe è un oggetto helper che consente di allocare i flussi di file per i file caricati.</span><span class="sxs-lookup"><span data-stu-id="b0528-130">The **MultipartFormDataStreamProvider** class is a helper object that allocates file streams for uploaded files.</span></span> <span data-ttu-id="b0528-131">Per leggere il messaggio MIME multiparte, chiamare il **ReadAsMultipartAsync** (metodo).</span><span class="sxs-lookup"><span data-stu-id="b0528-131">To read the multipart MIME message, call the **ReadAsMultipartAsync** method.</span></span> <span data-ttu-id="b0528-132">Questo metodo estrae tutte le parti del messaggio e li scrive in dei flussi forniti dal **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="b0528-132">This method extracts all of the message parts and writes them into the streams provided by the **MultipartFormDataStreamProvider**.</span></span>

<span data-ttu-id="b0528-133">Quando il metodo viene completato, è possibile ottenere informazioni sui file dal **FileData** proprietà, ovvero una raccolta di **MultipartFileData** oggetti.</span><span class="sxs-lookup"><span data-stu-id="b0528-133">When the method completes, you can get information about the files from the **FileData** property, which is a collection of **MultipartFileData** objects.</span></span>

- <span data-ttu-id="b0528-134">**MultipartFileData.FileName** è il nome del file locale nel server di cui è stato salvato il file.</span><span class="sxs-lookup"><span data-stu-id="b0528-134">**MultipartFileData.FileName** is the local file name on the server, where the file was saved.</span></span>
- <span data-ttu-id="b0528-135">**MultipartFileData.Headers** contiene l'intestazione di parte (*non* l'intestazione della richiesta).</span><span class="sxs-lookup"><span data-stu-id="b0528-135">**MultipartFileData.Headers** contains the part header (*not* the request header).</span></span> <span data-ttu-id="b0528-136">Ciò consente di accedere al contenuto\_intestazioni Disposition e Content-Type.</span><span class="sxs-lookup"><span data-stu-id="b0528-136">You can use this to access the Content\_Disposition and Content-Type headers.</span></span>

<span data-ttu-id="b0528-137">Come suggerisce il nome, **ReadAsMultipartAsync** è un metodo asincrono.</span><span class="sxs-lookup"><span data-stu-id="b0528-137">As the name suggests, **ReadAsMultipartAsync** is an asynchronous method.</span></span> <span data-ttu-id="b0528-138">Per eseguire operazioni dopo il completamento del metodo, usare una [attività di continuazione](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) o il **await** (parola chiave) (.NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="b0528-138">To perform work after the method completes, use a [continuation task](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) or the **await** keyword (.NET 4.5).</span></span>

<span data-ttu-id="b0528-139">Ecco la versione di .NET Framework 4.0 del codice precedente:</span><span class="sxs-lookup"><span data-stu-id="b0528-139">Here is the .NET Framework 4.0 version of the previous code:</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a><span data-ttu-id="b0528-140">Lettura di dati di controllo Form</span><span class="sxs-lookup"><span data-stu-id="b0528-140">Reading Form Control Data</span></span>

<span data-ttu-id="b0528-141">Il form HTML che ho illustrato in precedenza aveva un controllo input di testo.</span><span class="sxs-lookup"><span data-stu-id="b0528-141">The HTML form that I showed earlier had a text input control.</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

<span data-ttu-id="b0528-142">È possibile ottenere il valore del controllo dal **FormData** proprietà delle **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="b0528-142">You can get the value of the control from the **FormData** property of the **MultipartFormDataStreamProvider**.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

<span data-ttu-id="b0528-143">**FormData** è un **NameValueCollection** che contiene coppie nome/valore per i controlli del form.</span><span class="sxs-lookup"><span data-stu-id="b0528-143">**FormData** is a **NameValueCollection** that contains name/value pairs for the form controls.</span></span> <span data-ttu-id="b0528-144">La raccolta può contenere chiavi duplicate.</span><span class="sxs-lookup"><span data-stu-id="b0528-144">The collection can contain duplicate keys.</span></span> <span data-ttu-id="b0528-145">Si consideri questo modulo:</span><span class="sxs-lookup"><span data-stu-id="b0528-145">Consider this form:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

<span data-ttu-id="b0528-146">Il corpo della richiesta potrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="b0528-146">The request body might look like this:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

<span data-ttu-id="b0528-147">In tal caso, il **FormData** insieme contiene le coppie chiave/valore seguente:</span><span class="sxs-lookup"><span data-stu-id="b0528-147">In that case, the **FormData** collection would contain the following key/value pairs:</span></span>

- <span data-ttu-id="b0528-148">ottimizzazione dei viaggi: eseguire il round trip</span><span class="sxs-lookup"><span data-stu-id="b0528-148">trip: round-trip</span></span>
- <span data-ttu-id="b0528-149">opzioni: fino</span><span class="sxs-lookup"><span data-stu-id="b0528-149">options: nonstop</span></span>
- <span data-ttu-id="b0528-150">opzioni: date</span><span class="sxs-lookup"><span data-stu-id="b0528-150">options: dates</span></span>
- <span data-ttu-id="b0528-151">postazioni: finestra</span><span class="sxs-lookup"><span data-stu-id="b0528-151">seat: window</span></span>
