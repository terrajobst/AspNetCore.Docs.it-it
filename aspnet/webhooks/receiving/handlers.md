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
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="7c0d0-103">Gestori eventi Webhook ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7c0d0-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="7c0d0-104">Una volta le richieste di Webhook è stato convalidato da ricevitore WebHook, è pronto per essere eseguito dal codice utente.</span><span class="sxs-lookup"><span data-stu-id="7c0d0-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="7c0d0-105">Si tratta di where *gestori* sono disponibili in.</span><span class="sxs-lookup"><span data-stu-id="7c0d0-105">This is where *handlers* come in.</span></span> <span data-ttu-id="7c0d0-106">I gestori di derivano dal [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interfaccia, ma in genere utilizza la [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) classe anziché derivare direttamente dall'interfaccia.</span><span class="sxs-lookup"><span data-stu-id="7c0d0-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="7c0d0-107">Una richiesta di WebHook può essere elaborata da uno o più gestori.</span><span class="sxs-lookup"><span data-stu-id="7c0d0-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="7c0d0-108">Che vengono chiamati in ordine di base nelle rispettive *ordine* proprietà sarà dal più basso al più alto in ordine è un semplice numero intero (consigliato sia compreso tra 1 e 100):</span><span class="sxs-lookup"><span data-stu-id="7c0d0-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Gestore WebHook ordine proprietà diagramma](_static/Handlers.png)

<span data-ttu-id="7c0d0-110">Un gestore di è possibile impostare facoltativamente il *risposta* proprietà WebHookHandlerContext che offrirà l'elaborazione stop e la risposta da inviare nuovamente come risposta HTTP al WebHook.</span><span class="sxs-lookup"><span data-stu-id="7c0d0-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="7c0d0-111">Nel caso precedente, non verrà chiamato gestore C perché contiene un ordine più elevato rispetto a B e B imposta la risposta.</span><span class="sxs-lookup"><span data-stu-id="7c0d0-111">In the case above, Handler C won't get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="7c0d0-112">Impostazione della risposta è in genere rilevante solo per i Webhook in cui la risposta può contenere informazioni all'API di origine.</span><span class="sxs-lookup"><span data-stu-id="7c0d0-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="7c0d0-113">Ciò avviene, ad esempio con i Webhook Slack in cui la risposta viene eseguita il postback del canale dove proviene il WebHook.</span><span class="sxs-lookup"><span data-stu-id="7c0d0-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="7c0d0-114">Gestori possono impostare la proprietà ricevitore se lo desiderano solo ricevere i Webhook da tale particolare ricevitore.</span><span class="sxs-lookup"><span data-stu-id="7c0d0-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="7c0d0-115">Se il ricevitore che vengono chiamati per ciascuno di essi non impostati.</span><span class="sxs-lookup"><span data-stu-id="7c0d0-115">If they don't set the receiver they are called for all of them.</span></span>

<span data-ttu-id="7c0d0-116">Un altro uso comune di una risposta consiste nell'usare un *410-non* risposta per indicare che il WebHook non è più attivo e non ci sono richieste altre devono essere inviate.</span><span class="sxs-lookup"><span data-stu-id="7c0d0-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="7c0d0-117">Per impostazione predefinita un gestore verrà chiamato da tutti i destinatari di WebHook.</span><span class="sxs-lookup"><span data-stu-id="7c0d0-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="7c0d0-118">Tuttavia, se il *ricevitore* viene impostata sul nome di un gestore quindi tale gestore riceverà solo le richieste dei WebHook da tale destinatario.</span><span class="sxs-lookup"><span data-stu-id="7c0d0-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="7c0d0-119">L'elaborazione di un WebHook</span><span class="sxs-lookup"><span data-stu-id="7c0d0-119">Processing a WebHook</span></span>

<span data-ttu-id="7c0d0-120">Quando viene chiamato un gestore, ottiene un [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) contenente informazioni relative alla richiesta di WebHook.</span><span class="sxs-lookup"><span data-stu-id="7c0d0-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="7c0d0-121">I dati, in genere il corpo della richiesta HTTP, sono disponibili i *dati* proprietà.</span><span class="sxs-lookup"><span data-stu-id="7c0d0-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="7c0d0-122">Il tipo di dati è in genere i dati del modulo HTML o JSON, ma è possibile eseguire il cast a un tipo più specifico, se lo si desidera.</span><span class="sxs-lookup"><span data-stu-id="7c0d0-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="7c0d0-123">Ad esempio, il Webhook personalizzati generati da ASP.NET Webhook può essere convertito in tipo [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="7c0d0-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

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

  ## <a name="queued-processing"></a><span data-ttu-id="7c0d0-124">Elaborazione in coda</span><span class="sxs-lookup"><span data-stu-id="7c0d0-124">Queued Processing</span></span>

<span data-ttu-id="7c0d0-125">La maggior parte dei mittenti WebHook verranno inviato di nuovo un WebHook se non viene generata una risposta entro pochi secondi.</span><span class="sxs-lookup"><span data-stu-id="7c0d0-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="7c0d0-126">Ciò significa che il gestore di è necessario completare l'elaborazione all'interno di tale periodo di tempo affinché non possa essere chiamato nuovamente.</span><span class="sxs-lookup"><span data-stu-id="7c0d0-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="7c0d0-127">Se l'elaborazione richiede più tempo, o è preferibile gestire separatamente il [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) può essere utilizzato per inviare la richiesta di WebHook a una coda, ad esempio [archiviazione code di Azure](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="7c0d0-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="7c0d0-128">Una struttura di un [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementazione è disponibile qui:</span><span class="sxs-lookup"><span data-stu-id="7c0d0-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

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
