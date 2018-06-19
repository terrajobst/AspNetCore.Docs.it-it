---
uid: webhooks/index
title: Panoramica di ASP.NET Webhook | Documenti Microsoft
author: rick-anderson
description: Introduzione a ASP.NET Webhook.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: 52399c23cdf393a2f7f94661fd48098ced65948c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530050"
---
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="3dd97-103">Panoramica di ASP.NET Webhook</span><span class="sxs-lookup"><span data-stu-id="3dd97-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="3dd97-104">Webhook sono un semplice modello HTTP fornisce un modello di pubblicazione/sottoscrizione semplice per collegare tra loro servizi API Web e SaaS.</span><span class="sxs-lookup"><span data-stu-id="3dd97-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="3dd97-105">Quando si verifica un evento in un servizio, viene inviata una notifica sotto forma di una richiesta HTTP POST per i sottoscrittori registrati.</span><span class="sxs-lookup"><span data-stu-id="3dd97-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="3dd97-106">La richiesta POST contiene informazioni sull'evento che rende possibile per il ricevitore di agire di conseguenza.</span><span class="sxs-lookup"><span data-stu-id="3dd97-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="3dd97-107">A causa delle loro semplicità, Webhook già esposte da un numero elevato di servizi tra cui [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)e molto altro ancora.</span><span class="sxs-lookup"><span data-stu-id="3dd97-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="3dd97-108">Ad esempio, un WebHook può indicare che un file è stato modificato [Dropbox](http://dropbox.com/), è stato eseguito il commit di una modifica del codice in GitHub o un pagamento è stato avviato [PayPal](http://www.paypal.com/), o una scheda è stata creata [ Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="3dd97-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="3dd97-109">Le possibilità sono infinite!</span><span class="sxs-lookup"><span data-stu-id="3dd97-109">The possibilities are endless!</span></span>

<span data-ttu-id="3dd97-110">Microsoft ASP.NET WebHooks rende più semplice inviare e ricevere Webhook come parte dell'applicazione ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="3dd97-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="3dd97-111">Sul lato ricevente, fornisce un modello comune per la ricezione e l'elaborazione di Webhook da qualsiasi numero di provider WebHook.</span><span class="sxs-lookup"><span data-stu-id="3dd97-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="3dd97-112">Si tratta con supporto per [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) e [Zendesk](https://www.zendesk.com/) ma è aggiungere supporto per più facilmente.</span><span class="sxs-lookup"><span data-stu-id="3dd97-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="3dd97-113">Sul lato di invio fornisce supporto per la gestione e memorizzare anche le sottoscrizioni per l'invio di notifiche degli eventi per il set corretto di sottoscrittori.</span><span class="sxs-lookup"><span data-stu-id="3dd97-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="3dd97-114">Ciò consente di definire il proprio set di eventi che i sottoscrittori possono sottoscrivere e inviare una notifica in caso di operazioni.</span><span class="sxs-lookup"><span data-stu-id="3dd97-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="3dd97-115">Le due parti possono essere utilizzate insieme o divide in due a seconda dello scenario.</span><span class="sxs-lookup"><span data-stu-id="3dd97-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="3dd97-116">Se si desidera solo ricevere Webhook da altri servizi è possibile utilizzare solo la parte destinatario; Se si desidera esporre Webhook per altri utenti di utilizzare, quindi è possibile farlo.</span><span class="sxs-lookup"><span data-stu-id="3dd97-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="3dd97-117">Il codice destinato a ASP.NET Web API 2 e MVC ASP.NET 5 ed è disponibile come [OSS su GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="3dd97-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="3dd97-118">Panoramica del Webhook</span><span class="sxs-lookup"><span data-stu-id="3dd97-118">WebHooks Overview</span></span>

<span data-ttu-id="3dd97-119">Webhook sono un modello che significa che varia in modo viene utilizzato dal servizio al servizio ma l'idea di base è la stessa.</span><span class="sxs-lookup"><span data-stu-id="3dd97-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="3dd97-120">È possibile considerare Webhook come un modello semplice di pubblicazione/sottoscrizione in cui un utente può sottoscrivere eventi che si verificano in un' posizione.</span><span class="sxs-lookup"><span data-stu-id="3dd97-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="3dd97-121">Le notifiche degli eventi vengono propagati come richieste HTTP POST che contiene informazioni sull'evento stesso.</span><span class="sxs-lookup"><span data-stu-id="3dd97-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="3dd97-122">In genere la richiesta POST HTTP contiene un oggetto JSON o un determinato dal mittente WebHook incluse le informazioni sull'evento che causa il WebHook per generare dati di form HTML.</span><span class="sxs-lookup"><span data-stu-id="3dd97-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="3dd97-123">Ad esempio, un esempio di un corpo della richiesta POST WebHook da [GitHub](http://www.github.com/) ha un aspetto simile a causa di un nuovo problema viene aperto in un repository specifico:</span><span class="sxs-lookup"><span data-stu-id="3dd97-123">For example, an example of a WebHook POST request body from [GitHub](http://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

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

<span data-ttu-id="3dd97-124">Per garantire che il WebHook è effettivamente dal mittente corretto, la richiesta POST è protetto in qualche modo e quindi verificata dal ricevitore.</span><span class="sxs-lookup"><span data-stu-id="3dd97-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="3dd97-125">Ad esempio, [GitHub WebHooks](https://developer.github.com/webhooks/) include un *X-Hub-Signature* intestazione HTTP con un hash del corpo della richiesta è selezionata per l'implementazione del ricevitore è non ti preoccupare.</span><span class="sxs-lookup"><span data-stu-id="3dd97-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="3dd97-126">Il flusso WebHook viene in genere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="3dd97-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="3dd97-127">Il mittente WebHook espone gli eventi che può sottoscrivere un client.</span><span class="sxs-lookup"><span data-stu-id="3dd97-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="3dd97-128">Gli eventi vengono descritte observable le modifiche al sistema, ad esempio che un nuovo elemento di dati è stata inserita, che un processo è stata completata o un altro.</span><span class="sxs-lookup"><span data-stu-id="3dd97-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="3dd97-129">Il destinatario WebHook sottoscrive registrando un WebHook costituito da quattro operazioni:</span><span class="sxs-lookup"><span data-stu-id="3dd97-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="3dd97-130">Un URI in cui la notifica degli eventi deve essere registrato in forma di una richiesta HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="3dd97-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="3dd97-131">Un set di filtri che descrive gli eventi per il quale deve essere generato il WebHook;</span><span class="sxs-lookup"><span data-stu-id="3dd97-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="3dd97-132">Una chiave privata utilizzato per firmare la richiesta HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="3dd97-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="3dd97-133">Dati aggiuntivi che deve essere incluso nella richiesta HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="3dd97-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="3dd97-134">Questo può essere ad esempio campi di intestazione HTTP aggiuntivi o proprietà incluse nel corpo della richiesta HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="3dd97-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="3dd97-135">Una volta si verifica un evento, le registrazioni WebHook corrispondenti si trovano e vengono inviate le richieste HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="3dd97-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="3dd97-136">In genere, la generazione delle richieste HTTP POST sono dopo diversi tentativi se per qualche motivo che il destinatario non risponde o i risultati della richiesta HTTP POST in una risposta di errore.</span><span class="sxs-lookup"><span data-stu-id="3dd97-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="3dd97-137">Pipeline di elaborazione Webhook</span><span class="sxs-lookup"><span data-stu-id="3dd97-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="3dd97-138">La pipeline di elaborazione di Microsoft ASP.NET WebHooks per Webhook in ingresso è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="3dd97-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![Pipeline di elaborazione ASP.NET Webhook](_static/WebHookReceivers.png)

<span data-ttu-id="3dd97-140">I due concetti chiavi sono *ricevitori* e *gestori*:</span><span class="sxs-lookup"><span data-stu-id="3dd97-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="3dd97-141">*Ricevitori* sono responsabili per la gestione di tipo particolare di WebHook da un mittente specificato e per applicare controlli di sicurezza per garantire che la richiesta di WebHook è effettivamente dal mittente corretto.</span><span class="sxs-lookup"><span data-stu-id="3dd97-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="3dd97-142">*Gestori* sono in genere in cui il codice utente viene eseguita l'elaborazione del WebHook particolare.</span><span class="sxs-lookup"><span data-stu-id="3dd97-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="3dd97-143">I nodi seguenti questi concetti sono descritte in dettaglio.</span><span class="sxs-lookup"><span data-stu-id="3dd97-143">In the following nodes these concepts are described in more details.</span></span>
