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
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 52399c23cdf393a2f7f94661fd48098ced65948c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-overview"></a>Panoramica di ASP.NET Webhook

Webhook sono un semplice modello HTTP fornisce un modello di pubblicazione/sottoscrizione semplice per collegare tra loro servizi API Web e SaaS. Quando si verifica un evento in un servizio, viene inviata una notifica sotto forma di una richiesta HTTP POST per i sottoscrittori registrati. La richiesta POST contiene informazioni sull'evento che rende possibile per il ricevitore di agire di conseguenza.

A causa delle loro semplicità, Webhook già esposte da un numero elevato di servizi tra cui [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)e molto altro ancora. Ad esempio, un WebHook può indicare che un file è stato modificato [Dropbox](http://dropbox.com/), è stato eseguito il commit di una modifica del codice in GitHub o un pagamento è stato avviato [PayPal](http://www.paypal.com/), o una scheda è stata creata [ Trello](http://www.trello.com/). Le possibilità sono infinite!

Microsoft ASP.NET WebHooks rende più semplice inviare e ricevere Webhook come parte dell'applicazione ASP.NET:

* Sul lato ricevente, fornisce un modello comune per la ricezione e l'elaborazione di Webhook da qualsiasi numero di provider WebHook. Si tratta con supporto per [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) e [Zendesk](https://www.zendesk.com/) ma è aggiungere supporto per più facilmente.

* Sul lato di invio fornisce supporto per la gestione e memorizzare anche le sottoscrizioni per l'invio di notifiche degli eventi per il set corretto di sottoscrittori. Ciò consente di definire il proprio set di eventi che i sottoscrittori possono sottoscrivere e inviare una notifica in caso di operazioni.

Le due parti possono essere utilizzate insieme o divide in due a seconda dello scenario. Se si desidera solo ricevere Webhook da altri servizi è possibile utilizzare solo la parte destinatario; Se si desidera esporre Webhook per altri utenti di utilizzare, quindi è possibile farlo.

Il codice destinato a ASP.NET Web API 2 e MVC ASP.NET 5 ed è disponibile come [OSS su GitHub](https://github.com/aspnet/WebHooks).

## <a name="webhooks-overview"></a>Panoramica del Webhook

Webhook sono un modello che significa che varia in modo viene utilizzato dal servizio al servizio ma l'idea di base è la stessa. È possibile considerare Webhook come un modello semplice di pubblicazione/sottoscrizione in cui un utente può sottoscrivere eventi che si verificano in un' posizione. Le notifiche degli eventi vengono propagati come richieste HTTP POST che contiene informazioni sull'evento stesso.

In genere la richiesta POST HTTP contiene un oggetto JSON o un determinato dal mittente WebHook incluse le informazioni sull'evento che causa il WebHook per generare dati di form HTML. Ad esempio, un esempio di un corpo della richiesta POST WebHook da [GitHub](http://www.github.com/) ha un aspetto simile a causa di un nuovo problema viene aperto in un repository specifico:

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

Per garantire che il WebHook è effettivamente dal mittente corretto, la richiesta POST è protetto in qualche modo e quindi verificata dal ricevitore. Ad esempio, [GitHub WebHooks](https://developer.github.com/webhooks/) include un *X-Hub-Signature* intestazione HTTP con un hash del corpo della richiesta è selezionata per l'implementazione del ricevitore è non ti preoccupare.

Il flusso WebHook viene in genere simile al seguente:

* Il mittente WebHook espone gli eventi che può sottoscrivere un client. Gli eventi vengono descritte observable le modifiche al sistema, ad esempio che un nuovo elemento di dati è stata inserita, che un processo è stata completata o un altro.

* Il destinatario WebHook sottoscrive registrando un WebHook costituito da quattro operazioni:

     1. Un URI in cui la notifica degli eventi deve essere registrato in forma di una richiesta HTTP POST.

     2. Un set di filtri che descrive gli eventi per il quale deve essere generato il WebHook;

     3. Una chiave privata utilizzato per firmare la richiesta HTTP POST;

     4. Dati aggiuntivi che deve essere incluso nella richiesta HTTP POST. Questo può essere ad esempio campi di intestazione HTTP aggiuntivi o proprietà incluse nel corpo della richiesta HTTP POST.

* Una volta si verifica un evento, le registrazioni WebHook corrispondenti si trovano e vengono inviate le richieste HTTP POST. In genere, la generazione delle richieste HTTP POST sono dopo diversi tentativi se per qualche motivo che il destinatario non risponde o i risultati della richiesta HTTP POST in una risposta di errore.

## <a name="webhooks-processing-pipeline"></a>Pipeline di elaborazione Webhook

La pipeline di elaborazione di Microsoft ASP.NET WebHooks per Webhook in ingresso è simile al seguente:

![Pipeline di elaborazione ASP.NET Webhook](_static/WebHookReceivers.png)

I due concetti chiavi sono *ricevitori* e *gestori*:

* *Ricevitori* sono responsabili per la gestione di tipo particolare di WebHook da un mittente specificato e per applicare controlli di sicurezza per garantire che la richiesta di WebHook è effettivamente dal mittente corretto.

* *Gestori* sono in genere in cui il codice utente viene eseguita l'elaborazione del WebHook particolare.

I nodi seguenti questi concetti sono descritte in dettaglio.
