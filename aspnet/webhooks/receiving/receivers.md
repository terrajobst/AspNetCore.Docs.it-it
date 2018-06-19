---
uid: webhooks/receiving/receivers
title: Ricevitori di ASP.NET Webhook | Documenti Microsoft
author: rick-anderson
description: Ricevitori di ASP.NET Webhook
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: a8e42521f201f88b0ed433550e8786411b4487b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896296"
---
# <a name="aspnet-webhooks-receivers"></a>Ricevitori di ASP.NET Webhook

Ricezione Webhook varia a seconda che il mittente. In alcuni casi sono necessari passaggi aggiuntivi registrazione un WebHook verifica che il sottoscrittore è effettivamente in ascolto. Alcuni Webhook forniscono un modello di push-a-pull in cui la richiesta HTTP POST contiene solo un riferimento alle informazioni di evento che è quindi possibile recuperare in modo indipendente. Spesso il modello di sicurezza varia abbastanza.

Lo scopo di Microsoft ASP.NET WebHooks consiste nel verificare sia più semplice e coerente per associare l'API senza dedicare molto tempo per comprendere in che modo gestire qualsiasi particolare variante del Webhook.

Un destinatario WebHook è responsabile per l'accettazione e verifica Webhook da un determinato mittente. Un destinatario WebHook può supportare un numero qualsiasi di Webhook, ognuno con la propria configurazione. Ad esempio, il ricevitore GitHub WebHook può accettare Webhook da un numero qualsiasi di repository GitHub.

## <a name="webhook-receiver-uris"></a>URI destinatario WebHook

Per l'installazione di Microsoft ASP.NET WebHooks si otterrà un controller WebHook generale che accetta le richieste di WebHook da un numero di servizi aperto. Quando arriva una richiesta, viene selezionato il ricevitore appropriato che sono stati installati per la gestione di un determinato mittente WebHook.

L'URI di questo controller è l'URI WebHook che si registra con il servizio e ha il formato:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Per motivi di sicurezza più destinatari WebHook richiedono che l'URI è un *https* URI e in alcuni casi deve contenere anche un parametro di query aggiuntive che viene utilizzato per garantire che solo l'entità desiderata può inviare Webhook all'URI precedente .

Il <em> <receiver> </em> componente è il nome del destinatario, ad esempio <em>github</em> o <em>slack</em>.

Il *{id}* è un identificatore facoltativo che può essere usato per identificare una particolare configurazione destinatario WebHook. Questo può essere utilizzato per registrare Webhook N con un destinatario specifico. Gli URI seguenti tre, ad esempio, possono essere utilizzato per registrare per tre Webhook indipendenti:

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>L'installazione di un ricevitore WebHook

Per ricevere l'uso di Microsoft ASP.NET WebHooks Webhook, innanzitutto necessario installare il pacchetto Nuget per il provider WebHook o si desidera ricevere Webhook dal provider. I pacchetti Nuget sono denominati [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) in cui l'ultima parte indica il servizio è supportato. Esempio:

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) fornisce il supporto per la ricezione di Webhook da GitHub e [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) fornisce il supporto per la ricezione di Webhook generato da ASP. NET Webhook.

All'esterno della casella è possibile trovare il supporto per Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, striping, Trello e WordPress, ma è possibile supportare un numero qualsiasi di altri provider.

## <a name="configuring-a-webhook-receiver"></a>Configurazione di un ricevitore WebHook

Ricevitori WebHook vengono configurati mediante il [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) interfaccia e implementazioni specifiche di tale interfaccia possono essere registrate mediante qualsiasi modello di aggiunta della dipendenza. L'implementazione predefinita utilizza le impostazioni dell'applicazione può essere impostata nel file Web. config o, se si usa l'App Web di Azure, può essere impostata tramite il [portale Azure](https://portal.azure.com/).

![Impostazioni dell'App di Azure](_static/AzureAppSettings.png)

Il formato per le chiavi di impostazione dell'applicazione è come segue:

```
MS_WebHookReceiverSecret_<receiver>
```

Il valore è un elenco delimitato da virgole dei valori corrispondenti di *{id}* valori per il quale Webhook sono stati registrati, ad esempio:

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>L'inizializzazione di un ricevitore WebHook

Ricevitori WebHook vengono inizializzati tramite la registrazione, in genere nel *WebApiConfig* classe statica, ad esempio:

```csharp
namespace WebHookReceivers
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            // Load receivers
            config.InitializeReceiveGitHubWebHooks();
        }
    }
}
```
