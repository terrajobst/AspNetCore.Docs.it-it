---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
title: Informazioni sui filtri azione (c#) | Microsoft Docs
author: microsoft
description: L'obiettivo di questa esercitazione è di spiegare i filtri azione. Un filtro azione è un attributo che è possibile applicare un'azione del controller, o un intero controller...
ms.author: aspnetcontent
ms.date: 10/16/2008
ms.assetid: a94e4e81-40c1-47b7-8613-126a1a6cc93d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
msc.type: authoredcontent
ms.openlocfilehash: aaec90a14e53f0173ea09fbdbaa591ec0eb680b1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810163"
---
<a name="understanding-action-filters-c"></a>Informazioni sui filtri azione (c#)
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_CS.pdf)

> L'obiettivo di questa esercitazione è di spiegare i filtri azione. Un filtro azione è un attributo che è possibile applicare un'azione del controller, o un intero controller, che modifica il modo in cui viene eseguita l'azione.


## <a name="understanding-action-filters"></a>Informazioni sui filtri azione

L'obiettivo di questa esercitazione è di spiegare i filtri azione. Un filtro azione è un attributo che è possibile applicare un'azione del controller, o un intero controller, che modifica il modo in cui viene eseguita l'azione. Il framework ASP.NET MVC include vari filtri di azione:

- OutputCache – questo filtro delle azioni memorizza nella cache l'output di un'azione del controller per un periodo di tempo specificato.
- HandleError – questo filtro delle azioni di gestione degli errori generati quando viene eseguita un'azione del controller.
- Autorizzare: questo filtro delle azioni consente di limitare l'accesso a un particolare utente o ruolo.

È anche possibile creare i propri filtri azione personalizzati. Ad esempio, è possibile creare un filtro azione personalizzato per implementare un sistema di autenticazione personalizzato. In alternativa, si potrebbe voler creare un filtro azione che modifica i dati della visualizzazione restituiti da un'azione del controller.

In questa esercitazione descrive come creare un filtro azione fin dall'inizio. Creiamo un filtro di azione di Log che registra diverse fasi dell'elaborazione di un'azione di finestra di Output di Visual Studio.

### <a name="using-an-action-filter"></a>Usando un filtro azioni

Un filtro azione è un attributo. È possibile applicare la maggior parte dei filtri azione un'azione di singoli controller o un intero controller.

Ad esempio, il trattamento dei dati nel listato 1 espone un'azione denominata `Index()` che restituisce l'ora corrente. Questa azione è decorata con il `OutputCache` filtro delle azioni. Questo filtro, il valore restituito dall'azione da memorizzare nella cache per 10 secondi.

**Listato 1: `Controllers\DataController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample1.cs)]

Se viene richiamato più volte il `Index()` azione immettendo l'URL/Data/indice nella barra degli indirizzi del browser e l'aggiornamento di raggiungere il numero di volte, viene visualizzata la stessa ora per 10 secondi. L'output del `Index()` azione viene memorizzato nella cache per 10 secondi (vedere la figura 1).


[![Ora memorizzati nella cache](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)

**Figura 01**: memorizzate nella cache ([fare clic per visualizzare l'immagine con dimensioni normali](understanding-action-filters-cs/_static/image3.png))


Nel listato 1, un filtro di singola azione: il `OutputCache` viene applicato il filtro di azione: il `Index()` (metodo). Se è necessario, è possibile applicare più filtri di azione per la stessa azione. Ad esempio, è possibile applicare sia la `OutputCache` e `HandleError` filtri dell'azione alla stessa azione.

Nel listato 1, il `OutputCache` viene applicato il filtro azione il `Index()` azione. È anche possibile applicare questo attributo per il `DataController` classe stessa. In tal caso, sarebbe essere memorizzati nella cache il risultato restituito da qualsiasi azione esposta dal controller di per 10 secondi.

### <a name="the-different-types-of-filters"></a>I diversi tipi di filtri

Il framework ASP.NET MVC supporta quattro tipi diversi di filtri:

1. Filtri di autorizzazione: implementa la `IAuthorizationFilter` attributo.
2. Filtri azione: implementa la `IActionFilter` attributo.
3. Generare filtri: implementa la `IResultFilter` attributo.
4. I filtri eccezioni – implementa il `IExceptionFilter` attributo.

I filtri vengono eseguiti nell'ordine indicato in precedenza. Ad esempio, i filtri di autorizzazione vengono sempre eseguiti prima dei filtri azione e i filtri eccezioni vengono sempre eseguiti dopo ogni altro tipo di filtro.

Filtri di autorizzazione vengono utilizzati per implementare l'autenticazione e autorizzazione per le azioni del controller. Ad esempio, il filtro Authorize è un esempio di un filtro di autorizzazione.

Filtri azione contengono la logica che viene eseguita prima e dopo l'esecuzione di un'azione del controller. È possibile usare un filtro azione, ad esempio, per modificare i dati di visualizzazione che restituisce un'azione del controller.

I filtri risultato contengono la logica che viene eseguita prima e dopo l'esecuzione di un risultato della visualizzazione. Ad esempio, si potrebbe voler modificare un risultato della visualizzazione pulsante destro del mouse prima del rendering della visualizzazione nel browser.

I filtri eccezioni sono l'ultimo tipo di filtro per l'esecuzione. È possibile usare un filtro eccezioni per gestire gli errori generati dalle azioni del controller o i risultati dell'azione controller. È anche possibile usare i filtri eccezioni per registrare gli errori.

Ogni tipo di filtro viene eseguita in un ordine particolare. Se si desidera controllare l'ordine in cui vengono eseguiti i filtri dello stesso tipo è possibile impostare proprietà di ordine del filtro.

La classe base per tutti i filtri di azione è il `System.Web.Mvc.FilterAttribute` classe. Se si desidera implementare un particolare tipo di filtro, quindi è necessario creare una classe che eredita dalla classe di filtro di base e implementa una o più delle `IAuthorizationFilter`, `IActionFilter`, `IResultFilter`, o `IExceptionFilter` interfacce.

### <a name="the-base-actionfilterattribute-class"></a>La classe ActionFilterAttribute Base

Per rendere più semplice per implementare un filtro azioni personalizzato, il framework ASP.NET MVC include una base `ActionFilterAttribute` classe. Questa classe implementa sia il `IActionFilter` e `IResultFilter` interfacce ed eredita dal `Filter` classe.

La terminologia utilizzata in questo caso non è completamente coerenza. Tecnicamente, una classe che eredita dalla classe ActionFilterAttribute è sia un filtro azioni che un filtro risultato. Tuttavia, nel senso separato, il filtro azioni di word consente di fare riferimento a qualsiasi tipo di filtro nel framework di MVC ASP.NET.

La base `ActionFilterAttribute` classe dispone dei metodi seguenti che è possibile eseguire l'override:

- OnActionExecuting: questo metodo viene chiamato prima che venga eseguita un'azione del controller.
- OnActionExecuted: questo metodo viene chiamato dopo l'esecuzione di un'azione del controller.
- OnResultExecuting: questo metodo viene chiamato prima dell'esecuzione di un risultato di azione del controller.
- OnResultExecuted: questo metodo viene chiamato dopo l'esecuzione di un risultato di azione del controller.

Nella sezione successiva, si vedrà come è possibile implementare ognuno di questi metodi diversi.

### <a name="creating-a-log-action-filter"></a>Creazione di un filtro di azione di Log

Per illustrare come creare un filtro azioni personalizzato, si creerà un filtro azioni personalizzato che registra le fasi dell'elaborazione di un'azione del controller per la finestra di Output di Visual Studio. Nostro `LogActionFilter` è contenuta nel listato 2.

**Listato 2: `ActionFilters\LogActionFilter.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample2.cs)]

Nel listato 2, il `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, e `OnResultExecuted()` tutti i metodi chiamano il `Log()` (metodo). Il nome del metodo e i dati della route corrente viene passato per il `Log()` (metodo). Il `Log()` metodo scrive un messaggio nella finestra di Output di Visual Studio (vedere la figura 2).


[![La scrittura nella finestra di Output di Visual Studio](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)

**Figura 02**: la scrittura nella finestra di Output di Visual Studio ([fare clic per visualizzare l'immagine con dimensioni normali](understanding-action-filters-cs/_static/image6.png))


Il controller Home nel listato 3 viene illustrato come applicare il filtro di azione di Log a una classe intero controller. Ogni volta che le azioni esposte dal controller Home vengono richiamate, ovvero il `Index()` metodo o il `About()` metodo – le fasi di elaborazione azione vengono registrati nella finestra di Output di Visual Studio.

**Listato 3: `Controllers\HomeController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample3.cs)]

### <a name="summary"></a>Riepilogo

In questa esercitazione, sono stati presentati i filtri di azione MVC ASP.NET. Si è appreso come i quattro diversi tipi di filtri: filtri di autorizzazione, i filtri azione, i filtri risultato e i filtri eccezioni. Si è appreso anche sulla base `ActionFilterAttribute` classe.

Infine, si è appreso come implementare un filtro azione semplice. È stato creato un filtro di azione di Log che registra le fasi dell'elaborazione di un'azione del controller per la finestra di Output di Visual Studio.

> [!div class="step-by-step"]
> [Precedente](asp-net-mvc-routing-overview-cs.md)
> [Successivo](improving-performance-with-output-caching-cs.md)
