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
ms.openlocfilehash: 331d0e520a1fd8ec84aecd09a9c9e6d286c5893b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>Invia i dati di Form HTML in ASP.NET Web API: Multipart MIME e il caricamento di File
====================
da [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>Parte 2: Caricamento del File e in più parti MIME

In questa esercitazione viene illustrato come caricare i file in un'API web. Viene inoltre descritto come elaborare i dati MIME multipart.

> [!NOTE]
> [Scaricare il progetto completato](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).


Di seguito è riportato un esempio di un form HTML per il caricamento di un file:

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

Questo modulo contiene un controllo input di testo e un controllo input di file. Quando un modulo contiene un controllo input di file, il **enctype** attributo deve essere sempre &quot;multipart/form-data&quot;, che specifica che il modulo verrà inviato come messaggio MIME multiparte.

Il formato di un messaggio MIME multiparte è più facile da comprendere esaminando un esempio di richiesta:

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

Questo messaggio viene diviso in due *parti*, uno per ogni controllo del form. Per le righe che iniziano con trattini sono indicati i contorni della parte.

> [!NOTE]
> Il limite di parte include un componente casuale (&quot;41184676334&quot;) per garantire che la stringa di delimitazione viene accidentalmente visualizzato all'interno di una parte del messaggio.


Ogni parte del messaggio contiene una o più intestazioni, seguite dal contenuto di parte.

- L'intestazione Content-Disposition include il nome del controllo. Per i file, nonché il nome del file.
- L'intestazione Content-Type descrive i dati nella parte. Se questa intestazione viene omesso, il valore predefinito è text/plain.

Nell'esempio precedente, l'utente è caricato un file denominato GrandCanyon.jpg, con tipo di contenuto image/jpeg; e il valore del testo di input è &quot;ferie estate&quot;.

## <a name="file-upload"></a>Caricamento del file

Ora esaminiamo controller API Web che legge i file da un messaggio MIME multiparte. Il controller verrà letti i file in modo asincrono. API Web supporta azioni asincrone utilizzando il [il modello di programmazione basato su attività](https://msdn.microsoft.com/library/dd460693.aspx). In primo luogo, ecco il codice se la destinazione è .NET Framework 4.5, che supporta il **async** e **await** parole chiave.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

Si noti che l'azione del controller non accetta parametri. Ciò avviene perché si elabora il corpo di richiesta all'interno dell'azione, senza richiamare un formattatore di media type.

Il **IsMultipartContent** metodo controlla se la richiesta contiene un messaggio MIME multiparte. In caso contrario, il controller restituisce il codice di stato HTTP 415 (tipo di supporto non supportato).

Il **MultipartFormDataStreamProvider** classe è un oggetto helper che consente di allocare i flussi di file per i file caricati. Per leggere il messaggio MIME multiparte, chiamare il **ReadAsMultipartAsync** metodo. Questo metodo estrae tutte le parti del messaggio e li scrive in dei flussi forniti dal **MultipartFormDataStreamProvider**.

Quando il metodo viene completato, è possibile ottenere informazioni sui file dal **FileData** proprietà, che è una raccolta di **MultipartFileData** oggetti.

- **MultipartFileData.FileName** è il nome del file locale nel server, in cui è stato salvato il file.
- **MultipartFileData.Headers** contiene l'intestazione di parte (*non* l'intestazione della richiesta). Ciò consente di accedere al contenuto\_intestazioni Disposition e Content-Type.

Come suggerisce il nome, **ReadAsMultipartAsync** è un metodo asincrono. Per eseguire operazioni dopo il completamento del metodo, utilizzare un [attività di continuazione](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) o **await** (parola chiave) (.NET 4.5).

Di seguito è la versione di .NET Framework 4.0 del codice precedente:

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>Lettura di dati di controllo Form

Il form HTML che illustrato in precedenza era un controllo input di testo.

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

È possibile ottenere il valore del controllo dal **FormData** proprietà del **MultipartFormDataStreamProvider**.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**FormData** è un **NameValueCollection** che contiene coppie nome/valore per i controlli del form. La raccolta può contenere chiavi duplicate. Si consideri questo formato:

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

Il corpo della richiesta potrebbe essere simile al seguente:

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

In tal caso, il **FormData** insieme contiene le coppie chiave/valore seguenti:

- viaggi: round trip
- opzioni: fino
- opzioni: date
- postazione: finestra
