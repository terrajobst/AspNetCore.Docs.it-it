---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Informazioni sul processo di esecuzione di ASP.NET MVC | Documenti Microsoft
author: microsoft
description: Informazioni su come framework di MVC ASP.NET elabora una richiesta del browser dettagliata.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 5837c6e49709d6b86ee52cd88ffd4759c1850544
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="understanding-the-aspnet-mvc-execution-process"></a>Informazioni sul processo di esecuzione di ASP.NET MVC
====================
da [Microsoft](https://github.com/microsoft)

> Informazioni su come framework di MVC ASP.NET elabora una richiesta del browser dettagliata.


Le richieste a un'applicazione Web basata su MVC ASP.NET passano innanzitutto attraverso il **UrlRoutingModule** oggetto, ovvero un modulo HTTP. Questo modulo analizza la richiesta ed esegue la selezione delle route. Il **UrlRoutingModule** oggetto seleziona il primo oggetto route che corrisponde alla richiesta corrente. (Un oggetto route è una classe che implementa **RouteBase**, e in genere è un'istanza di **Route** classe.) Se non viene indirizzato corrisponda, il **UrlRoutingModule** oggetto non ha alcun effetto e consente di eseguire il fallback alla richiesta di ASP.NET o IIS regolare l'elaborazione della richiesta.

Dall'oggetto selezionato **Route** oggetto, il **UrlRoutingModule** oggetto Ottiene il **IRouteHandler** oggetto a cui è associato il **Route**oggetto. In genere, in un'applicazione MVC, questo sarà un'istanza di **MvcRouteHandler**. Il **IRouteHandler** istanza crea un **IHttpHandler** dell'oggetto e lo passa il **IHttpContext** oggetto. Per impostazione predefinita, il **IHttpHandler** istanza per MVC è il **MvcHandler** oggetto. Il **MvcHandler** oggetto seleziona quindi il controller che gestirà infine la richiesta.

> [!NOTE]
> Quando un'applicazione Web MVC ASP.NET viene eseguito in IIS 7.0, estensione non è necessaria per i progetti MVC. Tuttavia, in IIS 6.0, il gestore richiede il mapping di estensione del nome di file MVC alla DLL ISAPI ASP.NET.


Il modulo e il gestore sono i punti di ingresso per il framework di MVC ASP.NET. Eseguire le operazioni seguenti:

- Selezionare il controller appropriato in un'applicazione Web MVC.
- Ottenere un'istanza di un controller specifico.
- Chiamare il controller **Execute** metodo.

Di seguito sono elencate le fasi di esecuzione per un progetto Web MVC:

- Ricezione della prima richiesta per l'applicazione 

    - Nel file Global. asax, **Route** gli oggetti vengono aggiunti per il **RouteTable** oggetto.
- Eseguire il routing 

    - Il **UrlRoutingModule** modulo Usa la prima corrispondenza **Route** oggetto di **RouteTable** insieme per creare il **RouteData** oggetto che viene quindi utilizzato per creare un **RequestContext** (**IHttpContext**) oggetto.
- Creare il gestore di richieste MVC 

    - Il **MvcRouteHandler** oggetto crea un'istanza di **MvcHandler** classe e la passa il **RequestContext** istanza.
- Creare controller 

    - Il **MvcHandler** viene utilizzata da object il **RequestContext** istanza per identificare il **IControllerFactory** oggetto (in genere un'istanza di  **DefaultControllerFactory** classe) per creare l'istanza del controller.
- Esegui controller - il **MvcHandler** istanza chiama controller s **Execute** metodo. |
- Richiama l'azione 

    - La maggior parte dei controller di ereditare il **Controller** classe di base. Per i controller che a tale scopo, il **ControllerActionInvoker** oggetto associato con il controller determina quale metodo di azione della classe controller da chiamare, quindi chiama tale metodo.
- Esecuzione del risultato 

    - Un metodo di azione tipica potrebbe ricevere l'input dell'utente, preparare i dati di risposta appropriato e quindi eseguire il risultato restituendo un tipo di risultato. Tipi di risultato incorporati che è possono eseguire includono: **ViewResult** (che esegue il rendering di una vista ed è il tipo di risultato utilizzato più frequentemente), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, e **EmptyResult**.
