---
uid: webhooks/index
title: Cenni preliminari sui Webhook ASP.NET | Microsoft Docs
author: rick-anderson
description: Introduzione ai Webhook ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.openlocfilehash: 702cc0bf0d0bb887c64bec19e1faf249bd96617a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "48254959"
---
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="3af33-103">Panoramica di Webhook ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3af33-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="3af33-104">I Webhook sono un pattern HTTP leggero che fornisce un modello pub/sub semplice per collegare tra loro servizi SaaS e API Web.</span><span class="sxs-lookup"><span data-stu-id="3af33-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="3af33-105">Quando si verifica un evento in un servizio, sotto forma di una richiesta HTTP POST viene inviata una notifica ai sottoscrittori registrati.</span><span class="sxs-lookup"><span data-stu-id="3af33-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="3af33-106">La richiesta POST contiene informazioni sull'evento che rende possibile per il ricevitore di agire di conseguenza.</span><span class="sxs-lookup"><span data-stu-id="3af33-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="3af33-107">A causa delle loro semplicità, i Webhook sono già esposte da un numero elevato di servizi, incluse [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)e molto altro ancora.</span><span class="sxs-lookup"><span data-stu-id="3af33-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="3af33-108">Ad esempio, un WebHook può indicare che un file è stato modificato [Dropbox](http://dropbox.com/), è stato eseguito il commit di una modifica del codice in GitHub o un pagamento è stato avviato nella [PayPal](http://www.paypal.com/), o una scheda è stata creata [ Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="3af33-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="3af33-109">Le possibilità sono infinite!</span><span class="sxs-lookup"><span data-stu-id="3af33-109">The possibilities are endless!</span></span>

<span data-ttu-id="3af33-110">Microsoft ASP.NET WebHooks rende più semplice inviare e ricevere i Webhook come parte dell'applicazione ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="3af33-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="3af33-111">Sul lato ricevente, fornisce un modello comune per la ricezione e l'elaborazione di Webhook da un numero qualsiasi di provider di WebHook.</span><span class="sxs-lookup"><span data-stu-id="3af33-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="3af33-112">Si tratta di cui viene fornita con il supporto per [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) e [Zendesk](https://www.zendesk.com/) ma è facile aggiungere il supporto per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="3af33-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="3af33-113">Sul lato di invio fornisce supporto per la gestione e l'archiviazione delle sottoscrizioni per l'invio di notifiche degli eventi per il set corretto di sottoscrittori.</span><span class="sxs-lookup"><span data-stu-id="3af33-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="3af33-114">In questo modo è possibile definire il proprio set di eventi che i sottoscrittori possono sottoscrivere e inviare una notifica in caso di operazioni.</span><span class="sxs-lookup"><span data-stu-id="3af33-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="3af33-115">Le due parti sono utilizzabile tra loro o distanti tra loro a seconda dello scenario.</span><span class="sxs-lookup"><span data-stu-id="3af33-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="3af33-116">Se è necessario solo ricevere i Webhook da altri servizi è possibile usare solo la parte receiver; Se si desidera soltanto esporre i Webhook per gli altri utenti di utilizzare, quindi è possibile farlo.</span><span class="sxs-lookup"><span data-stu-id="3af33-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="3af33-117">Il codice è destinato a ASP.NET MVC 5 e API Web ASP.NET 2 ed è disponibile come [software Open Source su GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="3af33-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="3af33-118">Panoramica di Webhook</span><span class="sxs-lookup"><span data-stu-id="3af33-118">WebHooks Overview</span></span>

<span data-ttu-id="3af33-119">I Webhook sono un modello che significa che varia come viene usato dal servizio per servizio, ma l'idea di base è lo stesso.</span><span class="sxs-lookup"><span data-stu-id="3af33-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="3af33-120">È possibile considerare i Webhook come un modello pub/sub semplice in cui un utente può sottoscrivere eventi che si verificano in un' posizione.</span><span class="sxs-lookup"><span data-stu-id="3af33-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="3af33-121">Le notifiche degli eventi vengono propagati come richieste HTTP POST contenente le informazioni sull'evento stesso.</span><span class="sxs-lookup"><span data-stu-id="3af33-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="3af33-122">In genere la richiesta POST HTTP contiene un oggetto JSON o determinati dal mittente WebHook incluse le informazioni sull'evento che causa il WebHook per attivare i dati di form HTML.</span><span class="sxs-lookup"><span data-stu-id="3af33-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="3af33-123">Ad esempio, un esempio di un corpo della richiesta POST WebHook dal [GitHub](http://www.github.com/) ha un aspetto simile a seguito di un nuovo problema in corso l'apertura in un repository specifico:</span><span class="sxs-lookup"><span data-stu-id="3af33-123">For example, an example of a WebHook POST request body from [GitHub](http://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

```json
{
  "action": "opened",
  "issue": {
      "url": "https://api.github.com/repos/octocat/Hello-World/issues/1347",
      "number": 1347,
      ...
  },
  "repository": {
      "id": 1296269,
      "full_name": "octocat/Hello-World",
      "owner": {
          "login": "octocat",
          "id": 1
          ...
      },
      ...
  },
  "sender": {
      "login": "octocat",
      "id": 1,
      ...
  }
}
```

<span data-ttu-id="3af33-124">Per assicurarsi che il WebHook è effettivamente dal mittente corretto, la richiesta POST è protetta in qualche modo e quindi, viene verificata dal ricevitore.</span><span class="sxs-lookup"><span data-stu-id="3af33-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="3af33-125">Ad esempio, [GitHub WebHooks](https://developer.github.com/webhooks/) include un *X-Hub-Signature* intestazione HTTP con un hash del corpo della richiesta che viene controllato dall'implementazione del ricevitore, in modo non è necessario preoccuparsene.</span><span class="sxs-lookup"><span data-stu-id="3af33-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="3af33-126">Il flusso di WebHook in genere è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="3af33-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="3af33-127">Il mittente di WebHook espone gli eventi che può sottoscrivere un client.</span><span class="sxs-lookup"><span data-stu-id="3af33-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="3af33-128">Gli eventi di descrivono osservabile le modifiche al sistema, ad esempio che un nuovo elemento di dati è stata inserita, completamento di un processo o qualche altro.</span><span class="sxs-lookup"><span data-stu-id="3af33-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="3af33-129">Il ricevitore WebHook sottoscrive registrando un WebHook costituita da quattro elementi:</span><span class="sxs-lookup"><span data-stu-id="3af33-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="3af33-130">Un URI in cui la notifica degli eventi deve essere registrata in forma di una richiesta HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="3af33-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="3af33-131">Un set di filtri che descrive gli eventi specifici per il quale il WebHook deve essere generato;</span><span class="sxs-lookup"><span data-stu-id="3af33-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="3af33-132">Una chiave privata viene usata per firmare la richiesta HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="3af33-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="3af33-133">Dati aggiuntivi che deve essere incluso nella richiesta HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="3af33-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="3af33-134">Ciò può essere ad esempio campi di intestazione HTTP aggiuntivi o proprietà incluse nel corpo della richiesta HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="3af33-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="3af33-135">Una volta che si verifica un evento, le registrazioni di WebHook corrispondente si trovano e vengono inviate le richieste HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="3af33-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="3af33-136">In genere, la generazione delle richieste HTTP POST vengano ripetute più volte se per qualche motivo che il destinatario non risponde o i risultati della richiesta HTTP POST in una risposta di errore.</span><span class="sxs-lookup"><span data-stu-id="3af33-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="3af33-137">Pipeline di elaborazione di Webhook</span><span class="sxs-lookup"><span data-stu-id="3af33-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="3af33-138">La pipeline di elaborazione di Microsoft ASP.NET WebHooks per i Webhook in ingresso è simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="3af33-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![Webhook ASP.NET che elabora la Pipeline](_static/WebHookReceivers.png)

<span data-ttu-id="3af33-140">I due concetti chiave sono *ricevitori* e *gestori*:</span><span class="sxs-lookup"><span data-stu-id="3af33-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="3af33-141">*I ricevitori* sono responsabili per la gestione di versione particolare del WebHook da un mittente specificato e per l'applicazione controlli di sicurezza per assicurarsi che la richiesta del WebHook è effettivamente dal mittente desiderato.</span><span class="sxs-lookup"><span data-stu-id="3af33-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="3af33-142">*I gestori* sono in genere in cui il codice utente viene eseguita l'elaborazione di WebHook specifico.</span><span class="sxs-lookup"><span data-stu-id="3af33-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="3af33-143">Nei nodi seguenti sono descritti questi concetti in modo più dettagliato.</span><span class="sxs-lookup"><span data-stu-id="3af33-143">In the following nodes these concepts are described in more details.</span></span>
