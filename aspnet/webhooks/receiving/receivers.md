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
---
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="7308f-103">Ricevitori di ASP.NET Webhook</span><span class="sxs-lookup"><span data-stu-id="7308f-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="7308f-104">Ricezione Webhook varia a seconda che il mittente.</span><span class="sxs-lookup"><span data-stu-id="7308f-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="7308f-105">In alcuni casi sono necessari passaggi aggiuntivi registrazione un WebHook verifica che il sottoscrittore è effettivamente in ascolto.</span><span class="sxs-lookup"><span data-stu-id="7308f-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="7308f-106">Alcuni Webhook forniscono un modello di push-a-pull in cui la richiesta HTTP POST contiene solo un riferimento alle informazioni di evento che è quindi possibile recuperare in modo indipendente.</span><span class="sxs-lookup"><span data-stu-id="7308f-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="7308f-107">Spesso il modello di sicurezza varia abbastanza.</span><span class="sxs-lookup"><span data-stu-id="7308f-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="7308f-108">Lo scopo di Microsoft ASP.NET WebHooks consiste nel verificare sia più semplice e coerente per associare l'API senza dedicare molto tempo per comprendere in che modo gestire qualsiasi particolare variante del Webhook.</span><span class="sxs-lookup"><span data-stu-id="7308f-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="7308f-109">Un destinatario WebHook è responsabile per l'accettazione e verifica Webhook da un determinato mittente.</span><span class="sxs-lookup"><span data-stu-id="7308f-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="7308f-110">Un destinatario WebHook può supportare un numero qualsiasi di Webhook, ognuno con la propria configurazione.</span><span class="sxs-lookup"><span data-stu-id="7308f-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="7308f-111">Ad esempio, il ricevitore GitHub WebHook può accettare Webhook da un numero qualsiasi di repository GitHub.</span><span class="sxs-lookup"><span data-stu-id="7308f-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="7308f-112">URI destinatario WebHook</span><span class="sxs-lookup"><span data-stu-id="7308f-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="7308f-113">Per l'installazione di Microsoft ASP.NET WebHooks si otterrà un controller WebHook generale che accetta le richieste di WebHook da un numero di servizi aperto.</span><span class="sxs-lookup"><span data-stu-id="7308f-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="7308f-114">Quando arriva una richiesta, viene selezionato il ricevitore appropriato che sono stati installati per la gestione di un determinato mittente WebHook.</span><span class="sxs-lookup"><span data-stu-id="7308f-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="7308f-115">L'URI di questo controller è l'URI WebHook che si registra con il servizio e ha il formato:</span><span class="sxs-lookup"><span data-stu-id="7308f-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="7308f-116">Per motivi di sicurezza più destinatari WebHook richiedono che l'URI è un *https* URI e in alcuni casi deve contenere anche un parametro di query aggiuntive che viene utilizzato per garantire che solo l'entità desiderata può inviare Webhook all'URI precedente .</span><span class="sxs-lookup"><span data-stu-id="7308f-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="7308f-117">Il <em> <receiver> </em> componente è il nome del destinatario, ad esempio <em>github</em> o <em>slack</em>.</span><span class="sxs-lookup"><span data-stu-id="7308f-117">The <em><receiver></em> component is the name of the receiver, for example <em>github</em> or <em>slack</em>.</span></span>

<span data-ttu-id="7308f-118">Il *{id}* è un identificatore facoltativo che può essere usato per identificare una particolare configurazione destinatario WebHook.</span><span class="sxs-lookup"><span data-stu-id="7308f-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="7308f-119">Questo può essere utilizzato per registrare Webhook N con un destinatario specifico.</span><span class="sxs-lookup"><span data-stu-id="7308f-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="7308f-120">Gli URI seguenti tre, ad esempio, possono essere utilizzato per registrare per tre Webhook indipendenti:</span><span class="sxs-lookup"><span data-stu-id="7308f-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="7308f-121">L'installazione di un ricevitore WebHook</span><span class="sxs-lookup"><span data-stu-id="7308f-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="7308f-122">Per ricevere l'uso di Microsoft ASP.NET WebHooks Webhook, innanzitutto necessario installare il pacchetto Nuget per il provider WebHook o si desidera ricevere Webhook dal provider.</span><span class="sxs-lookup"><span data-stu-id="7308f-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="7308f-123">I pacchetti Nuget sono denominati [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) in cui l'ultima parte indica il servizio è supportato.</span><span class="sxs-lookup"><span data-stu-id="7308f-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="7308f-124">Esempio:</span><span class="sxs-lookup"><span data-stu-id="7308f-124">For example</span></span>

<span data-ttu-id="7308f-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) fornisce il supporto per la ricezione di Webhook da GitHub e [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) fornisce il supporto per la ricezione di Webhook generato da ASP. NET Webhook.</span><span class="sxs-lookup"><span data-stu-id="7308f-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="7308f-126">All'esterno della casella è possibile trovare il supporto per Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, striping, Trello e WordPress, ma è possibile supportare un numero qualsiasi di altri provider.</span><span class="sxs-lookup"><span data-stu-id="7308f-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="7308f-127">Configurazione di un ricevitore WebHook</span><span class="sxs-lookup"><span data-stu-id="7308f-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="7308f-128">Ricevitori WebHook vengono configurati mediante il [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) interfaccia e implementazioni specifiche di tale interfaccia possono essere registrate mediante qualsiasi modello di aggiunta della dipendenza.</span><span class="sxs-lookup"><span data-stu-id="7308f-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="7308f-129">L'implementazione predefinita utilizza le impostazioni dell'applicazione può essere impostata nel file Web. config o, se si usa l'App Web di Azure, può essere impostata tramite il [portale Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="7308f-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Impostazioni dell'App di Azure](_static/AzureAppSettings.png)

<span data-ttu-id="7308f-131">Il formato per le chiavi di impostazione dell'applicazione è come segue:</span><span class="sxs-lookup"><span data-stu-id="7308f-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="7308f-132">Il valore è un elenco delimitato da virgole dei valori corrispondenti di *{id}* valori per il quale Webhook sono stati registrati, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="7308f-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="7308f-133">L'inizializzazione di un ricevitore WebHook</span><span class="sxs-lookup"><span data-stu-id="7308f-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="7308f-134">Ricevitori WebHook vengono inizializzati tramite la registrazione, in genere nel *WebApiConfig* classe statica, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="7308f-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

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
