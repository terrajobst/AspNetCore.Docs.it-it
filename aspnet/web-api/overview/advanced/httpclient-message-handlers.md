---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Gestori di messaggi HttpClient nell'API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/01/2012
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 764244d1299d8cfcb59c3f15d63b42ebff4f6ac0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823921"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a>Gestori di messaggi HttpClient nell'API Web ASP.NET
====================
da [Mike Wasson](https://github.com/MikeWasson)

Oggetto *gestore di messaggi* è una classe che riceve una richiesta HTTP e restituisce una risposta HTTP.

In genere, una serie di gestori di messaggi vengono concatenati. Il primo gestore riceve una richiesta HTTP, esegue alcune operazioni di elaborazione e offre la richiesta al gestore successivo. A un certo punto, la risposta viene creata e viene reimpostato fino alla catena. Questo modello viene chiamato un *delega* gestore.

![](httpclient-message-handlers/_static/image1.png)

Sul lato client, il **HttpClient** classe Usa un gestore di messaggi per elaborare le richieste. Il gestore predefinito è **HttpClientHandler**, che invia la richiesta attraverso la rete e ottiene la risposta dal server. È possibile inserire i gestori di messaggi personalizzato nella pipeline del client:

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> API Web ASP.NET usa anche i gestori di messaggi sul lato server. Per altre informazioni, vedere [gestori di messaggi HTTP](http-message-handlers.md).


## <a name="custom-message-handlers"></a>Gestori di messaggi personalizzato

Per scrivere un gestore di messaggi personalizzato, derivare da **System.Net.Http.DelegatingHandler** ed eseguire l'override di **SendAsync** (metodo). Ecco la firma del metodo:

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

Il metodo accetta un **HttpRequestMessage** come input e in modo asincrono restituisce un **HttpResponseMessage**. Una tipica implementazione esegue le operazioni seguenti:

1. Elaborare il messaggio di richiesta.
2. Chiamare `base.SendAsync` per inviare la richiesta per il gestore interno.
3. Il gestore interno restituisce un messaggio di risposta. (Questo passaggio è asincrono).
4. Elaborare la risposta e restituirlo al chiamante.

Nell'esempio seguente viene illustrato un gestore di messaggi che aggiunge un'intestazione personalizzata per la richiesta in uscita:

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

La chiamata a `base.SendAsync` è asincrona. Se il gestore esegue qualsiasi attività dopo questa chiamata, usare il **await** parola chiave per riprendere l'esecuzione dopo il completamento del metodo. Nell'esempio seguente illustra un gestore che registra i codici di errore. La registrazione se stessa non è molto interessante, ma nell'esempio viene illustrato come ottenere la risposta all'interno del gestore.

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>Aggiunta di gestori di messaggi alla Pipeline di Client

Per aggiungere gestori personalizzati per **HttpClient**, utilizzare il **HttpClientFactory.Create** metodo:

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

Gestori di messaggi vengono chiamati nell'ordine in cui vengono trasmessi nel **Create** (metodo). Perché vengono annidati i gestori eventi, il messaggio di risposta viene trasferita in altra direzione. L'ultimo gestore, ovvero è il primo per ottenere il messaggio di risposta.
