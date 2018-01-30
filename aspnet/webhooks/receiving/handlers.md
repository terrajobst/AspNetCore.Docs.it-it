---
uid: webhooks/receiving/handlers
title: Gestori ASP.NET Webhook | Documenti Microsoft
author: rick-anderson
description: Come gestire le richieste ASP.NET Webhook.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 4cf5770a731ef77842eb29b0a66ee0aac5d85d85
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-webhooks-handlers"></a>Gestori ASP.NET Webhook

Una volta richieste Webhook è stata convalidata da un ricevitore WebHook, è pronto per essere eseguito dal codice utente. Questa opzione è quando *gestori* sono disponibili in. Gestori derivano il [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interfaccia, ma in genere viene utilizzato il [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) classe anziché derivare direttamente dall'interfaccia.

Una richiesta di WebHook può essere elaborata da uno o più gestori. Gestori eventi vengono chiamati nell'ordine in base alle rispettive *ordine* proprietà dal più basso al più alto in ordine è un semplice valore integer (suggerito per essere compreso tra 1 e 100):

![Diagramma di proprietà WebHook gestore ordine](_static/Handlers.png)

È possibile impostare un gestore di *risposta* proprietà WebHookHandlerContext che comporti l'elaborazione di arresto e la risposta da inviare nuovamente come risposta HTTP per il WebHook. Nel caso precedente, verrà richiamato gestore C perché contiene un ordine più elevato rispetto a, B e B imposta la risposta.

Impostazione della risposta è in genere rilevante solo per Webhook in cui la risposta può contenere informazioni all'API di origine. È ad esempio il caso Slack Webhook in cui la risposta viene restituita al canale di provenienza il WebHook. Gestori possono impostare la proprietà di ricevitore se si desidera ricevere Webhook da tale destinatario specifico. Se il destinatario non impostati vengono chiamati per ciascuno di essi.

Un altro utilizzo comune di una risposta consiste nell'usare un *Gone 410* risposta per indicare che il WebHook non è attivo e non ulteriori richieste devono essere inviate.

Per impostazione predefinita, un gestore verrà chiamato da tutti i ricevitori WebHook. Tuttavia, se il *ricevitore* proprietà è impostata sul nome di un gestore quindi tale gestore verrà visualizzato solo richieste WebHook da tale destinatario.

## <a name="processing-a-webhook"></a>L'elaborazione di un WebHook

Quando viene chiamato un gestore, ottiene un [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) contenente informazioni sulla richiesta WebHook. I dati, in genere il corpo della richiesta HTTP, sono disponibili il *dati* proprietà.

Il tipo di dati è in genere dati di form HTML o JSON, ma è possibile eseguire il cast di un tipo più specifico, se lo si desidera. Ad esempio, il Webhook personalizzato generato da ASP.NET Webhook può eseguire il cast al tipo di [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) come indicato di seguito:

```csharp
public class MyWebHookHandler : WebHookHandler
{
    public MyWebHookHandler()
    {
        this.Receiver = "custom";
    }

    public override Task ExecuteAsync(string generator, WebHookHandlerContext context)
    {
        CustomNotifications notifications = context.GetDataOrDefault<CustomNotifications>();
        foreach (var notification in notifications.Notifications)
        {
            ...
        }
        return Task.FromResult(true);
    }
}
```

  ## <a name="queued-processing"></a>Elaborazione in coda

La maggior parte dei mittenti WebHook invia di nuovo un WebHook se non viene generata una risposta entro pochi secondi. Ciò significa che il gestore deve completare l'elaborazione all'interno di tale periodo di tempo in modo che non può essere chiamata nuovamente.

Se l'elaborazione richiede più tempo, oppure è preferibile gestire separatamente il [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) può essere utilizzato per inviare la richiesta di WebHook da una coda, ad esempio [coda di archiviazione di Azure](https://msdn.microsoft.com/library/azure/dd179353.aspx).

Una struttura di un [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementazione è fornita di seguito:

```csharp
public class QueueHandler : WebHookQueueHandler
{
    public override Task EnqueueAsync(WebHookQueueContext context)
    {

        // Enqueue WebHookQueueContext to your queuing system of choice

        return Task.FromResult(true);
    }
}
```
