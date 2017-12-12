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
ms.openlocfilehash: 3aaef756ee00d7e44aa757062e1ef297312ecf22
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="7b25b-103">Gestori ASP.NET Webhook</span><span class="sxs-lookup"><span data-stu-id="7b25b-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="7b25b-104">Una volta richieste Webhook è stata convalidata da un ricevitore WebHook, è pronto per essere eseguito dal codice utente.</span><span class="sxs-lookup"><span data-stu-id="7b25b-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="7b25b-105">Questa opzione è quando *gestori* sono disponibili in.</span><span class="sxs-lookup"><span data-stu-id="7b25b-105">This is where *handlers* come in.</span></span> <span data-ttu-id="7b25b-106">Gestori derivano il [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interfaccia, ma in genere viene utilizzato il [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) classe anziché derivare direttamente dall'interfaccia.</span><span class="sxs-lookup"><span data-stu-id="7b25b-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="7b25b-107">Una richiesta di WebHook può essere elaborata da uno o più gestori.</span><span class="sxs-lookup"><span data-stu-id="7b25b-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="7b25b-108">Gestori eventi vengono chiamati nell'ordine in base alle rispettive *ordine* proprietà dal più basso al più alto in ordine è un semplice valore integer (suggerito per essere compreso tra 1 e 100):</span><span class="sxs-lookup"><span data-stu-id="7b25b-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Diagramma di proprietà WebHook gestore ordine](_static/Handlers.png)

<span data-ttu-id="7b25b-110">È possibile impostare un gestore di *risposta* proprietà WebHookHandlerContext che comporti l'elaborazione di arresto e la risposta da inviare nuovamente come risposta HTTP per il WebHook.</span><span class="sxs-lookup"><span data-stu-id="7b25b-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="7b25b-111">Nel caso precedente, verrà richiamato gestore C perché contiene un ordine più elevato rispetto a, B e B imposta la risposta.</span><span class="sxs-lookup"><span data-stu-id="7b25b-111">In the case above, Handler C won’t get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="7b25b-112">Impostazione della risposta è in genere rilevante solo per Webhook in cui la risposta può contenere informazioni all'API di origine.</span><span class="sxs-lookup"><span data-stu-id="7b25b-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="7b25b-113">È ad esempio il caso Slack Webhook in cui la risposta viene restituita al canale di provenienza il WebHook.</span><span class="sxs-lookup"><span data-stu-id="7b25b-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="7b25b-114">Gestori possono impostare la proprietà di ricevitore se si desidera ricevere Webhook da tale destinatario specifico.</span><span class="sxs-lookup"><span data-stu-id="7b25b-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="7b25b-115">Se il destinatario non impostati vengono chiamati per ciascuno di essi.</span><span class="sxs-lookup"><span data-stu-id="7b25b-115">If they don’t set the receiver they are called for all of them.</span></span>

<span data-ttu-id="7b25b-116">Un altro utilizzo comune di una risposta consiste nell'usare un *Gone 410* risposta per indicare che il WebHook non è attivo e non ulteriori richieste devono essere inviate.</span><span class="sxs-lookup"><span data-stu-id="7b25b-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="7b25b-117">Per impostazione predefinita, un gestore verrà chiamato da tutti i ricevitori WebHook.</span><span class="sxs-lookup"><span data-stu-id="7b25b-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="7b25b-118">Tuttavia, se il *ricevitore* proprietà è impostata sul nome di un gestore quindi tale gestore verrà visualizzato solo richieste WebHook da tale destinatario.</span><span class="sxs-lookup"><span data-stu-id="7b25b-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="7b25b-119">L'elaborazione di un WebHook</span><span class="sxs-lookup"><span data-stu-id="7b25b-119">Processing a WebHook</span></span>

<span data-ttu-id="7b25b-120">Quando viene chiamato un gestore, ottiene un [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) contenente informazioni sulla richiesta WebHook.</span><span class="sxs-lookup"><span data-stu-id="7b25b-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="7b25b-121">I dati, in genere il corpo della richiesta HTTP, sono disponibili il *dati* proprietà.</span><span class="sxs-lookup"><span data-stu-id="7b25b-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="7b25b-122">Il tipo di dati è in genere dati di form HTML o JSON, ma è possibile eseguire il cast di un tipo più specifico, se lo si desidera.</span><span class="sxs-lookup"><span data-stu-id="7b25b-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="7b25b-123">Ad esempio, il Webhook personalizzato generato da ASP.NET Webhook può eseguire il cast al tipo di [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="7b25b-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

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

  ## <a name="queued-processing"></a><span data-ttu-id="7b25b-124">Elaborazione in coda</span><span class="sxs-lookup"><span data-stu-id="7b25b-124">Queued Processing</span></span>

<span data-ttu-id="7b25b-125">La maggior parte dei mittenti WebHook invia di nuovo un WebHook se non viene generata una risposta entro pochi secondi.</span><span class="sxs-lookup"><span data-stu-id="7b25b-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="7b25b-126">Ciò significa che il gestore deve completare l'elaborazione all'interno di tale periodo di tempo in modo che non può essere chiamata nuovamente.</span><span class="sxs-lookup"><span data-stu-id="7b25b-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="7b25b-127">Se l'elaborazione richiede più tempo, oppure è preferibile gestire separatamente il [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) può essere utilizzato per inviare la richiesta di WebHook da una coda, ad esempio [coda di archiviazione di Azure](https://msdn.microsoft.com/en-us/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="7b25b-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/en-us/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="7b25b-128">Una struttura di un [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementazione è fornita di seguito:</span><span class="sxs-lookup"><span data-stu-id="7b25b-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

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
