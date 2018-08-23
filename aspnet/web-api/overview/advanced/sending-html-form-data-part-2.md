---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: "L'invio di dati di Form HTML nell'API Web ASP.NET: caricamento di File e MIME Multipart | Microsoft Docs"
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/21/2012
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 875f9ac62901dfbafc8224af2982c1daf3afc9c5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836788"
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>L'invio di dati di Form HTML nell'API Web ASP.NET: caricamento di File e MIME Multipart
====================
da [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>Parte 2: Caricamento di File e MIME Multipart

Questa esercitazione illustra come caricare i file a un'API web. Viene anche descritto come elaborare i dati MIME multipart.

> [!NOTE]
> [Scaricare il progetto completato](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).


Ecco un esempio di un form HTML per il caricamento di un file:

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

Questo modulo contiene un controllo input di testo e un controllo input di file. Quando un modulo contiene un controllo input di file, il **enctype** attributo deve essere sempre &quot;multipart/form-data&quot;, che specifica che verrà inviato il form come un messaggio MIME multiparte.

Il formato di un messaggio MIME multiparte è più facile da comprendere osservando un esempio di richiesta:

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

Questo messaggio viene diviso in due *parti*, uno per ogni controllo del modulo. Sono indicati i limiti di parte dalle righe che iniziano con i trattini.

> [!NOTE]
> Il limite di ambito include un componente casuale (&quot;41184676334&quot;) per garantire che la stringa di delimitazione non viene visualizzata per errore all'interno di una parte del messaggio.


Ogni parte del messaggio contiene una o più intestazioni, seguite dal contenuto di parte.

- L'intestazione Content-Disposition include il nome del controllo. Per i file, nonché il nome del file.
- L'intestazione Content-Type descrive i dati nella parte. Se questa intestazione viene omesso, il valore predefinito è text/plain.

Nell'esempio precedente, l'utente ha caricato un file denominato GrandCanyon.jpg, con tipo di contenuto image/jpeg; e il valore del testo di input è stata &quot;estive&quot;.

## <a name="file-upload"></a>Caricamento di file

A questo punto verrà ora esaminato un controller API Web che legge i file da un messaggio MIME multiparte. Il controller leggerà i file in modo asincrono. API Web supporta le azioni asincrone usando il [modello di programmazione basato su attività](https://msdn.microsoft.com/library/dd460693.aspx). In primo luogo, ecco il codice se la destinazione è .NET Framework 4.5, che supporta il **async** e **await** parole chiave.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

Si noti che l'azione del controller non accetta alcun parametro. Ciò avviene perché si elabora il corpo di richiesta nell'azione, senza richiamare un formattatore di media type.

Il **IsMultipartContent** metodo controlla se la richiesta contiene un messaggio MIME multiparte. In caso contrario, il controller restituisce il codice di stato HTTP 415 (tipo di supporto non supportato).

Il **MultipartFormDataStreamProvider** classe è un oggetto helper che consente di allocare i flussi di file per i file caricati. Per leggere il messaggio MIME multiparte, chiamare il **ReadAsMultipartAsync** (metodo). Questo metodo estrae tutte le parti del messaggio e li scrive in dei flussi forniti dal **MultipartFormDataStreamProvider**.

Quando il metodo viene completato, è possibile ottenere informazioni sui file dal **FileData** proprietà, ovvero una raccolta di **MultipartFileData** oggetti.

- **MultipartFileData.FileName** è il nome del file locale nel server di cui è stato salvato il file.
- **MultipartFileData.Headers** contiene l'intestazione di parte (*non* l'intestazione della richiesta). Ciò consente di accedere al contenuto\_intestazioni Disposition e Content-Type.

Come suggerisce il nome, **ReadAsMultipartAsync** è un metodo asincrono. Per eseguire operazioni dopo il completamento del metodo, usare una [attività di continuazione](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) o il **await** (parola chiave) (.NET 4.5).

Ecco la versione di .NET Framework 4.0 del codice precedente:

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>Lettura di dati di controllo Form

Il form HTML che ho illustrato in precedenza aveva un controllo input di testo.

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

È possibile ottenere il valore del controllo dal **FormData** proprietà delle **MultipartFormDataStreamProvider**.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**FormData** è un **NameValueCollection** che contiene coppie nome/valore per i controlli del form. La raccolta può contenere chiavi duplicate. Si consideri questo modulo:

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

Il corpo della richiesta potrebbe essere simile al seguente:

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

In tal caso, il **FormData** insieme contiene le coppie chiave/valore seguente:

- ottimizzazione dei viaggi: eseguire il round trip
- opzioni: fino
- opzioni: date
- postazioni: finestra
