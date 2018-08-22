---
uid: webhooks/receiving/receivers
title: Webhook ASP.NET ricevitori | Microsoft Docs
author: rick-anderson
description: Ricevitori di Webhook ASP.NET
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.openlocfilehash: 376cb3e3fdc0bc7bd248da1f57e1064fb27b3cef
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823721"
---
# <a name="aspnet-webhooks-receivers"></a>Ricevitori di Webhook ASP.NET

Ricezione di Webhook varia a seconda che il mittente è. In alcuni casi sono necessari passaggi aggiuntivi la registrazione di un WebHook verifica che il sottoscrittore sia effettivamente in ascolto. Alcuni Webhook offrono un modello di push di pull in cui la richiesta HTTP POST contiene solo un riferimento a informazioni relative all'evento che è quindi possibile recuperare in modo indipendente. Spesso il modello di sicurezza varia in modo sostanziale.

Lo scopo di Microsoft ASP.NET WebHooks è necessario sia più semplice e coerente per collegare l'API senza un notevole dispendio di tempo a comprendere come gestire qualsiasi variante particolare del Webhook.

Un ricevitore WebHook è responsabile dell'accettazione e verifica i Webhook da un determinato mittente. Un ricevitore WebHook può supportare un numero qualsiasi di Webhook, ognuno con la propria configurazione. Ad esempio, il ricevitore GitHub WebHook può accettare i Webhook da un numero qualsiasi di repository GitHub.

## <a name="webhook-receiver-uris"></a>URI ricevitore WebHook

Tramite l'installazione di Microsoft ASP.NET WebHooks otterrai un controller di WebHook generale che accetta le richieste dei WebHook da un numero aperto di servizi. Quando arriva una richiesta, sceglie il ricevitore appropriato installati per la gestione di un determinato mittente WebHook.

L'URI di questo controller è l'URI del WebHook che si registra con il servizio e ha il formato:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Per motivi di sicurezza, molti ricevitori WebHook richiedono che l'URI è un *https* URI e in alcuni casi deve contenere anche un parametro di query aggiuntive che viene utilizzato per garantire che solo l'entità desiderata può inviare i Webhook per l'URI precedente .

Il <em> <receiver> </em> componente è il nome del destinatario, ad esempio <em>github</em> oppure <em>slack</em>.

Il *{id}* è un identificatore facoltativo che può essere usato per identificare una particolare configurazione ricevitore WebHook. Utilizzabile per registrare i N i Webhook con un destinatario specifico. Ad esempio, gli URI seguenti tre utilizzabile per registrarsi per tre Webhook indipendenti:

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Installazione di un ricevitore di WebHook

Per ricevere i Webhook con Microsoft ASP.NET WebHooks, è prima di tutto installare il pacchetto Nuget per il provider di WebHook o i provider di che webhook da ricevere. I pacchetti Nuget sono denominati [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) in cui l'ultima parte indica il servizio è supportato. Esempio:

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) fornisce il supporto per la ricezione di Webhook di GitHub e [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) fornisce il supporto per la ricezione di Webhook generato da ASP. Webhook di NET.

Impostazione predefinita è possibile trovare il supporto per Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello e WordPress, ma è possibile supportare un numero qualsiasi di altri provider.

## <a name="configuring-a-webhook-receiver"></a>Configurazione di un ricevitore di WebHook

I ricevitori WebHook vengono configurati tramite il [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) all'interfaccia e determinate implementazioni dell'interfaccia possono essere registrate con qualsiasi modello di inserimento delle dipendenze. L'implementazione predefinita Usa le impostazioni dell'applicazione che può essere impostata nel file Web. config o, se si usa App Web di Azure può essere impostata tramite il [portale di Azure](https://portal.azure.com/).

![Impostazioni App di Azure](_static/AzureAppSettings.png)

Il formato per le chiavi di impostazione dell'applicazione è come segue:

```
MS_WebHookReceiverSecret_<receiver>
```

Il valore è un elenco delimitato da virgole di valori corrispondenti di *{id}* i valori per il quale i Webhook sono stati registrati, ad esempio:

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Inizializzazione di un ricevitore di WebHook

I ricevitori WebHook vengono inizializzati registrandoli, in genere nel *WebApiConfig* classe statica, ad esempio:

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
