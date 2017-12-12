---
uid: web-api/overview/advanced/http-message-handlers
title: Gestori di messaggi HTTP in ASP.NET Web API | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 931ad3330d5e4dc2424ab37b8a6e2a3d123a8684
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="http-message-handlers-in-aspnet-web-api"></a>Gestori di messaggi HTTP in ASP.NET Web API
====================
da [Mike Wasson](https://github.com/MikeWasson)

Oggetto *gestore di messaggi* è una classe che riceve una richiesta HTTP e restituisce una risposta HTTP. Gestori messaggi derivano dalla classe astratta **HttpMessageHandler** classe.

In genere, una serie di gestori di messaggi sono concatenati. Il primo gestore riceve una richiesta HTTP, esegue alcune operazioni di elaborazione e inviare la richiesta al gestore successivo. A un certo punto, la risposta viene creata e viene reimpostato fino alla catena. Questo modello viene denominato un *delega* gestore.

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a>Gestori di messaggi sul lato server

Sul lato server, la pipeline di Web API utilizza alcuni gestori di messaggi predefiniti:

- **Server HTTP** Ottiene la richiesta dall'host.
- **HttpRoutingDispatcher** invia la richiesta in base alla route.
- **HttpControllerDispatcher** invia la richiesta a un controller API Web.

È possibile aggiungere gestori personalizzati per la pipeline. Gestori di messaggi sono ideali per problemi di montaggio incrociato che operano a livello di HTTP messaggi (anziché le azioni del controller). Ad esempio, potrebbe essere un gestore di messaggi:

- Leggere o modificare le intestazioni di richiesta.
- Aggiungere un'intestazione di risposta per le risposte.
- Convalidare le richieste prima di raggiungere il controller.

Questo diagramma mostra due gestori personalizzati, inseriti nella pipeline di:

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> Sul lato client, HttpClient Usa anche i gestori di messaggi. Per ulteriori informazioni, vedere [gestori di messaggi HttpClient](httpclient-message-handlers.md).


## <a name="custom-message-handlers"></a>Gestori di messaggi personalizzato

Per scrivere un gestore di messaggi personalizzato, derivare da **System.Net.Http.DelegatingHandler** ed eseguire l'override di **SendAsync** metodo. Questo metodo ha la firma seguente:

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

Il metodo accetta un **HttpRequestMessage** come input e, in modo asincrono, restituisce un **HttpResponseMessage**. Un'implementazione tipica esegue le operazioni seguenti:

1. Elaborare il messaggio di richiesta.
2. Chiamare `base.SendAsync` per inviare la richiesta al gestore interno.
3. Il gestore interno restituisce un messaggio di risposta. (Questo passaggio è asincrono).
4. Elaborare la risposta e restituirlo al chiamante.

Di seguito è riportato un esempio semplice:

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> La chiamata a `base.SendAsync` è asincrono. Se il gestore esegue qualsiasi lavoro dopo questa chiamata, utilizzare il **await** (parola chiave), come illustrato.


Un gestore delega può anche ignorare il gestore interno e creare direttamente la risposta:

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

Se una delega gestore crea la risposta senza chiamare `base.SendAsync`, la richiesta ignora il resto della pipeline. Questo può essere utile per un gestore che convalida la richiesta (creazione di una risposta di errore).

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a>Aggiunta di un gestore per la Pipeline

Per aggiungere un gestore di messaggi sul lato server, aggiungere il gestore di **HttpConfiguration.MessageHandlers** insieme. Se si usa il modello "Applicazione Web di ASP.NET MVC 4" per creare il progetto, non è presente all'interno di **WebApiConfig** classe:

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

Vengono chiamati i gestori di messaggi nello stesso ordine in cui vengono visualizzati in **MessageHandlers** insieme. Perché sono nidificati, il messaggio di risposta viene spostato in altra direzione. L'ultimo gestore è il primo ad avere il messaggio di risposta.

Si noti che non è necessario impostare gestori interni; framework Web API si connette automaticamente i gestori dei messaggi.

Se si è [self-hosting](../older-versions/self-host-a-web-api.md), creare un'istanza del **HttpSelfHostConfiguration** classe e aggiungere i gestori per il **MessageHandlers** insieme.

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

Ora esaminiamo alcuni esempi di gestori di messaggi personalizzata.

## <a name="example-x-http-method-override"></a>Esempio: X-HTTP-Method-Override

X-HTTP-Method-Override è un'intestazione HTTP non standard. È progettato per i client che non è possibile inviare determinati tipi di richiesta HTTP, ad esempio PUT o DELETE. Al contrario, il client invia una richiesta POST e imposta l'intestazione X-HTTP-Method-Override per il metodo desiderato. Ad esempio:

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

Di seguito è riportato un gestore di messaggi che aggiunge il supporto per X-HTTP-Method-Override:

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

Nel **SendAsync** metodo, il gestore verifica se il messaggio di richiesta è una richiesta POST, e se contiene l'intestazione X-HTTP-Method-Override. In caso affermativo, convalida il valore dell'intestazione e viene quindi modificato il metodo di richiesta. Infine, il gestore chiama `base.SendAsync` per passare il messaggio al gestore successivo.

Quando la richiesta raggiunge il **HttpControllerDispatcher** (classe), **HttpControllerDispatcher** indirizza la richiesta in base al metodo di richiesta di aggiornamento.

## <a name="example-adding-a-custom-response-header"></a>Esempio: Aggiunta di un'intestazione di risposta personalizzata

Di seguito è riportato un gestore di messaggi che aggiunge un'intestazione personalizzata per ogni messaggio di risposta:

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

In primo luogo, il gestore chiama `base.SendAsync` per passare la richiesta al gestore di messaggi interno. Il gestore interno restituisce un messaggio di risposta, ma non così in modo asincrono utilizzando un **attività&lt;T&gt;**  oggetto. Il messaggio di risposta non è disponibile fino al `base.SendAsync` viene completata in modo asincrono.

Questo esempio viene utilizzato il **await** (parola chiave) per lavorare in modo asincrono dopo `SendAsync` viene completata. Se la destinazione è .NET Framework 4.0, utilizzare il **attività**&lt;T&gt;**. Metodo ContinueWith** metodo:

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a>Esempio: Verifica di una chiave API

Alcuni servizi web richiedono i client includere una chiave API nella richiesta. Nell'esempio seguente viene illustrato come un gestore di messaggi può controllare le richieste per una chiave API valida:

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

Questo gestore esegue la ricerca per la chiave API nella stringa di query dell'URI. (In questo esempio si presuppone che la chiave è una stringa statica. Un'implementazione reale utilizzerebbe una convalida più complessa.) Se la stringa di query contiene la chiave, il gestore passa la richiesta al gestore interno.

Se la richiesta non ha una chiave valida, il gestore crea un messaggio di risposta con stato, 403 accesso negato. In questo caso, il gestore non chiama `base.SendAsync`, pertanto, il gestore interno mai riceve la richiesta e neppure il controller. Pertanto, il controller può presupporre che tutte le richieste in ingresso presenta una chiave di API valida.

> [!NOTE]
> Se la chiave API si applica solo a determinate azioni del controller, considerare l'utilizzo di un filtro azione anziché un gestore di messaggi. I filtri di azione eseguiti dopo l'esecuzione di routing di URI.


## <a name="per-route-message-handlers"></a>Gestori messaggi Route

Gestori di **HttpConfiguration.MessageHandlers** raccolta si applicano globalmente.

In alternativa, è possibile aggiungere un gestore di messaggi a un ciclo specifico quando si definisce la route:

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

In questo esempio, se l'URI della richiesta corrisponde a "Route2", la richiesta viene inviata al `MessageHandler2`. Il diagramma seguente mostra la pipeline per queste due route:

![](http-message-handlers/_static/image4.png)

Si noti che `MessageHandler2` sostituisce il valore predefinito **HttpControllerDispatcher**. In questo esempio, `MessageHandler2` crea la risposta, mentre le richieste che corrispondono a "Route2" mai accedere a un controller. Ciò consente di sostituire l'intero meccanismo controller API Web con il proprio endpoint personalizzati.

In alternativa, un gestore di messaggi per ogni route può delegare a **HttpControllerDispatcher**, che quindi la invia a un controller.

![](http-message-handlers/_static/image5.png)

Il codice seguente viene illustrato come configurare questa route:

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
