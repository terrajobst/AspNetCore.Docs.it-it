---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Informazioni sul processo di esecuzione di ASP.NET MVC | Microsoft Docs
author: microsoft
description: Informazioni su come il framework ASP.NET MVC elabora una richiesta del browser dettagliata.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: d94fcedb6e0ec6134bdbd69729abf1f875fac420
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378271"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a>Informazioni sul processo di esecuzione di ASP.NET MVC
====================
da [Microsoft](https://github.com/microsoft)

> Informazioni su come il framework ASP.NET MVC elabora una richiesta del browser dettagliata.


Le richieste a un'applicazione Web basata su MVC ASP.NET passano innanzitutto attraverso il **UrlRoutingModule** oggetto, ovvero un modulo HTTP. Questo modulo analizza la richiesta ed esegue la selezione di route. Il **UrlRoutingModule** oggetto consente di selezionare il primo oggetto route che corrisponde alla richiesta corrente. (Un oggetto route è una classe che implementa **RouteBase**, ed è in genere un'istanza di **Route** classe.) Se nessuna route corrispondono, il **UrlRoutingModule** oggetto non esegue alcuna operazione e consente la richiesta di eseguire il fallback alla richiesta di ASP.NET o IIS normale elaborazione.

Dall'oggetto selezionato **Route** oggetto, il **UrlRoutingModule** oggetto Ottiene il **IRouteHandler** oggetto a cui è associato il **Route**oggetto. In genere, in un'applicazione MVC, sarà un'istanza di **MvcRouteHandler**. Il **IRouteHandler** istanza crea un **IHttpHandler** dell'oggetto e lo passa il **IHttpContext** oggetto. Per impostazione predefinita, il **IHttpHandler** dell'istanza per MVC è la **MvcHandler** oggetto. Il **MvcHandler** oggetto seleziona quindi il controller che gestirà infine la richiesta.

> [!NOTE]
> Quando un'applicazione Web MVC ASP.NET viene eseguita in IIS 7.0, non è necessario per i progetti MVC estensione. In IIS 6.0, il gestore richiede tuttavia che è eseguire il mapping di estensione del nome di file MVC alla DLL ISAPI ASP.NET.


Il modulo e il gestore sono i punti di ingresso per il framework ASP.NET MVC. Eseguire le operazioni seguenti:

- Selezionare il controller appropriato in un'applicazione Web MVC.
- Ottenere un'istanza del controller specifico.
- Chiamare il controller **Execute** (metodo).

Di seguito sono elencate le fasi di esecuzione per un progetto Web MVC:

- Ricezione della prima richiesta per l'applicazione 

    - Nel file Global. asax **Route** gli oggetti vengono aggiunti per il **RouteTable** oggetto.
- Eseguire il routing 

    - Il **UrlRoutingModule** modulo Usa la prima corrispondenza **Route** dell'oggetto nel **RouteTable** insieme per creare il **RouteData** oggetto, che viene quindi utilizzato per creare un **RequestContext** (**IHttpContext**) oggetti.
- Creare il gestore di richieste MVC 

    - Il **MvcRouteHandler** oggetto crea un'istanza del **MvcHandler** classe e la passa il **RequestContext** istanza.
- Creare controller 

    - Il **MvcHandler** oggetto utilizza il **RequestContext** istanza per identificare il **IControllerFactory** oggetto (in genere un'istanza del  **DefaultControllerFactory** classe) per creare l'istanza del controller.
- Execute controller - i **MvcHandler** istanza chiama il controller s **Execute** (metodo). |
- Azione di richiamo 

    - La maggior parte dei controller ereditano dal **Controller** classe di base. Per i controller che eseguire questa operazione, il **ControllerActionInvoker** oggetto, ovvero associato al controller determina quale metodo di azione della classe controller da chiamare e quindi chiama tale metodo.
- Risultato dell'esecuzione 

    - Un metodo di azione tipica potrebbe ricevere l'input utente, preparare i dati di risposta appropriato e quindi eseguire il risultato tramite la restituzione di un tipo di risultato. I tipi di risultato incorporati che possono essere eseguiti includono quanto segue: **ViewResult** (che esegue il rendering di una vista ed è il tipo di risultato utilizzato più frequentemente), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, e **EmptyResult**.
