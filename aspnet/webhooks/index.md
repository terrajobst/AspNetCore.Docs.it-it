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
# <a name="aspnet-webhooks-overview"></a>Panoramica di Webhook ASP.NET

I Webhook sono un pattern HTTP leggero che fornisce un modello pub/sub semplice per collegare tra loro servizi SaaS e API Web. Quando si verifica un evento in un servizio, sotto forma di una richiesta HTTP POST viene inviata una notifica ai sottoscrittori registrati. La richiesta POST contiene informazioni sull'evento che rende possibile per il ricevitore di agire di conseguenza.

A causa delle loro semplicità, i Webhook sono già esposte da un numero elevato di servizi, incluse [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)e molto altro ancora. Ad esempio, un WebHook può indicare che un file è stato modificato [Dropbox](http://dropbox.com/), è stato eseguito il commit di una modifica del codice in GitHub o un pagamento è stato avviato nella [PayPal](http://www.paypal.com/), o una scheda è stata creata [ Trello](http://www.trello.com/). Le possibilità sono infinite!

Microsoft ASP.NET WebHooks rende più semplice inviare e ricevere i Webhook come parte dell'applicazione ASP.NET:

* Sul lato ricevente, fornisce un modello comune per la ricezione e l'elaborazione di Webhook da un numero qualsiasi di provider di WebHook. Si tratta di cui viene fornita con il supporto per [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) e [Zendesk](https://www.zendesk.com/) ma è facile aggiungere il supporto per altre informazioni.

* Sul lato di invio fornisce supporto per la gestione e l'archiviazione delle sottoscrizioni per l'invio di notifiche degli eventi per il set corretto di sottoscrittori. In questo modo è possibile definire il proprio set di eventi che i sottoscrittori possono sottoscrivere e inviare una notifica in caso di operazioni.

Le due parti sono utilizzabile tra loro o distanti tra loro a seconda dello scenario. Se è necessario solo ricevere i Webhook da altri servizi è possibile usare solo la parte receiver; Se si desidera soltanto esporre i Webhook per gli altri utenti di utilizzare, quindi è possibile farlo.

Il codice è destinato a ASP.NET MVC 5 e API Web ASP.NET 2 ed è disponibile come [software Open Source su GitHub](https://github.com/aspnet/WebHooks).

## <a name="webhooks-overview"></a>Panoramica di Webhook

I Webhook sono un modello che significa che varia come viene usato dal servizio per servizio, ma l'idea di base è lo stesso. È possibile considerare i Webhook come un modello pub/sub semplice in cui un utente può sottoscrivere eventi che si verificano in un' posizione. Le notifiche degli eventi vengono propagati come richieste HTTP POST contenente le informazioni sull'evento stesso.

In genere la richiesta POST HTTP contiene un oggetto JSON o determinati dal mittente WebHook incluse le informazioni sull'evento che causa il WebHook per attivare i dati di form HTML. Ad esempio, un esempio di un corpo della richiesta POST WebHook dal [GitHub](http://www.github.com/) ha un aspetto simile a seguito di un nuovo problema in corso l'apertura in un repository specifico:

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

Per assicurarsi che il WebHook è effettivamente dal mittente corretto, la richiesta POST è protetta in qualche modo e quindi, viene verificata dal ricevitore. Ad esempio, [GitHub WebHooks](https://developer.github.com/webhooks/) include un *X-Hub-Signature* intestazione HTTP con un hash del corpo della richiesta che viene controllato dall'implementazione del ricevitore, in modo non è necessario preoccuparsene.

Il flusso di WebHook in genere è simile al seguente:

* Il mittente di WebHook espone gli eventi che può sottoscrivere un client. Gli eventi di descrivono osservabile le modifiche al sistema, ad esempio che un nuovo elemento di dati è stata inserita, completamento di un processo o qualche altro.

* Il ricevitore WebHook sottoscrive registrando un WebHook costituita da quattro elementi:

     1. Un URI in cui la notifica degli eventi deve essere registrata in forma di una richiesta HTTP POST.

     2. Un set di filtri che descrive gli eventi specifici per il quale il WebHook deve essere generato;

     3. Una chiave privata viene usata per firmare la richiesta HTTP POST.

     4. Dati aggiuntivi che deve essere incluso nella richiesta HTTP POST. Ciò può essere ad esempio campi di intestazione HTTP aggiuntivi o proprietà incluse nel corpo della richiesta HTTP POST.

* Una volta che si verifica un evento, le registrazioni di WebHook corrispondente si trovano e vengono inviate le richieste HTTP POST. In genere, la generazione delle richieste HTTP POST vengano ripetute più volte se per qualche motivo che il destinatario non risponde o i risultati della richiesta HTTP POST in una risposta di errore.

## <a name="webhooks-processing-pipeline"></a>Pipeline di elaborazione di Webhook

La pipeline di elaborazione di Microsoft ASP.NET WebHooks per i Webhook in ingresso è simile alla seguente:

![Webhook ASP.NET che elabora la Pipeline](_static/WebHookReceivers.png)

I due concetti chiave sono *ricevitori* e *gestori*:

* *I ricevitori* sono responsabili per la gestione di versione particolare del WebHook da un mittente specificato e per l'applicazione controlli di sicurezza per assicurarsi che la richiesta del WebHook è effettivamente dal mittente desiderato.

* *I gestori* sono in genere in cui il codice utente viene eseguita l'elaborazione di WebHook specifico.

Nei nodi seguenti sono descritti questi concetti in modo più dettagliato.
