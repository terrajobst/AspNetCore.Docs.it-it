---
uid: webhooks/receiving/handlers
title: Gestori eventi Webhook ASP.NET | Microsoft Docs
author: rick-anderson
description: Come gestire le richieste in ASP.NET i Webhook.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: bc8f4ef3f4ade775b395d73dfa8d73fec92fba3f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841422"
---
# <a name="aspnet-webhooks-handlers"></a>Gestori eventi Webhook ASP.NET

Una volta le richieste di Webhook è stato convalidato da ricevitore WebHook, è pronto per essere eseguito dal codice utente. Si tratta di where *gestori* sono disponibili in. I gestori di derivano dal [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interfaccia, ma in genere utilizza la [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) classe anziché derivare direttamente dall'interfaccia.

Una richiesta di WebHook può essere elaborata da uno o più gestori. Che vengono chiamati in ordine di base nelle rispettive *ordine* proprietà sarà dal più basso al più alto in ordine è un semplice numero intero (consigliato sia compreso tra 1 e 100):

![Gestore WebHook ordine proprietà diagramma](_static/Handlers.png)

Un gestore di è possibile impostare facoltativamente il *risposta* proprietà WebHookHandlerContext che offrirà l'elaborazione stop e la risposta da inviare nuovamente come risposta HTTP al WebHook. Nel caso precedente, non verrà chiamato gestore C perché contiene un ordine più elevato rispetto a B e B imposta la risposta.

Impostazione della risposta è in genere rilevante solo per i Webhook in cui la risposta può contenere informazioni all'API di origine. Ciò avviene, ad esempio con i Webhook Slack in cui la risposta viene eseguita il postback del canale dove proviene il WebHook. Gestori possono impostare la proprietà ricevitore se lo desiderano solo ricevere i Webhook da tale particolare ricevitore. Se il ricevitore che vengono chiamati per ciascuno di essi non impostati.

Un altro uso comune di una risposta consiste nell'usare un *410-non* risposta per indicare che il WebHook non è più attivo e non ci sono richieste altre devono essere inviate.

Per impostazione predefinita un gestore verrà chiamato da tutti i destinatari di WebHook. Tuttavia, se il *ricevitore* viene impostata sul nome di un gestore quindi tale gestore riceverà solo le richieste dei WebHook da tale destinatario.

## <a name="processing-a-webhook"></a>L'elaborazione di un WebHook

Quando viene chiamato un gestore, ottiene un [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) contenente informazioni relative alla richiesta di WebHook. I dati, in genere il corpo della richiesta HTTP, sono disponibili i *dati* proprietà.

Il tipo di dati è in genere i dati del modulo HTML o JSON, ma è possibile eseguire il cast a un tipo più specifico, se lo si desidera. Ad esempio, il Webhook personalizzati generati da ASP.NET Webhook può essere convertito in tipo [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) come indicato di seguito:

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

La maggior parte dei mittenti WebHook verranno inviato di nuovo un WebHook se non viene generata una risposta entro pochi secondi. Ciò significa che il gestore di è necessario completare l'elaborazione all'interno di tale periodo di tempo affinché non possa essere chiamato nuovamente.

Se l'elaborazione richiede più tempo, o è preferibile gestire separatamente il [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) può essere utilizzato per inviare la richiesta di WebHook a una coda, ad esempio [archiviazione code di Azure](https://msdn.microsoft.com/library/azure/dd179353.aspx).

Una struttura di un [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementazione è disponibile qui:

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
