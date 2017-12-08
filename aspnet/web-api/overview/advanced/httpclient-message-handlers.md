---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Gestori messaggi HttpClient ASP.NET Web API | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2012
ms.topic: article
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 805741b0ac682b7479ce82127df48b1b9a49a427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a>Gestori messaggi HttpClient ASP.NET Web API
====================
da [Mike Wasson](https://github.com/MikeWasson)

Oggetto *gestore di messaggi* è una classe che riceve una richiesta HTTP e restituisce una risposta HTTP.

In genere, una serie di gestori di messaggi sono concatenati. Il primo gestore riceve una richiesta HTTP, esegue alcune operazioni di elaborazione e inviare la richiesta al gestore successivo. A un certo punto, la risposta viene creata e viene reimpostato fino alla catena. Questo modello viene denominato un *delega* gestore.

![](httpclient-message-handlers/_static/image1.png)

Sul lato client, il **HttpClient** classe utilizza un gestore di messaggi per elaborare le richieste. Il gestore predefinito **HttpClientHandler**, che invia la richiesta attraverso la rete e si ottiene la risposta dal server. È possibile inserire i gestori di messaggi personalizzati nella pipeline di client:

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> ASP.NET Web API utilizza anche i gestori di messaggi sul lato server. Per ulteriori informazioni, vedere [gestori di messaggi HTTP](http-message-handlers.md).


## <a name="custom-message-handlers"></a>Gestori di messaggi personalizzato

Per scrivere un gestore di messaggi personalizzato, derivare da **System.Net.Http.DelegatingHandler** ed eseguire l'override di **SendAsync** metodo. Di seguito è riportata la firma del metodo:

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

Il metodo accetta un **HttpRequestMessage** come input e, in modo asincrono, restituisce un **HttpResponseMessage**. Un'implementazione tipica esegue le operazioni seguenti:

1. Elaborare il messaggio di richiesta.
2. Chiamare `base.SendAsync` per inviare la richiesta al gestore interno.
3. Il gestore interno restituisce un messaggio di risposta. (Questo passaggio è asincrono).
4. Elaborare la risposta e restituirlo al chiamante.

Nell'esempio seguente viene illustrato un gestore di messaggi che aggiunge un'intestazione personalizzata alla richiesta in uscita:

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

La chiamata a `base.SendAsync` è asincrono. Se il gestore esegue qualsiasi lavoro dopo questa chiamata, utilizzare il **await** (parola chiave) per riprendere l'esecuzione dopo il completamento del metodo. Nell'esempio seguente viene illustrato un gestore che registra i codici di errore. La registrazione se stesso non è molto interessante, ma nell'esempio viene illustrato come ottenere la risposta all'interno del gestore.

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>Aggiunta di gestori di messaggi alla Pipeline di Client

Per aggiungere gestori personalizzati per **HttpClient**, utilizzare il **HttpClientFactory.Create** metodo:

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

Messaggio che vengono chiamati nell'ordine in cui vengono passate la **crea** metodo. Perché vengono annidati i gestori eventi, il messaggio di risposta viene spostato in altra direzione. L'ultimo gestore è il primo ad avere il messaggio di risposta.
