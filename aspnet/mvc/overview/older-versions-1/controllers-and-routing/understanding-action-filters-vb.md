---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
title: Informazioni sui filtri dell'azione (VB) | Documenti Microsoft
author: microsoft
description: L'obiettivo di questa esercitazione è illustrare i filtri dell'azione. Un filtro azione è un attributo che è possibile applicare a un controller intero o un'azione del controller -...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: e83812f2-c53e-4a43-a7c1-d64c59ecf694
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
msc.type: authoredcontent
ms.openlocfilehash: 2796b67cba6a2ddaee7a006a170dfb7e5ff89888
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="understanding-action-filters-vb"></a>Informazioni sui filtri dell'azione (VB)
====================
by [Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_VB.pdf)

> L'obiettivo di questa esercitazione è illustrare i filtri dell'azione. Un filtro azione è un attributo che è possibile applicare a un'azione del controller - o a un intero controller - che modifica il modo in cui viene eseguita l'azione.


## <a name="understanding-action-filters"></a>Informazioni sui filtri azione

L'obiettivo di questa esercitazione è illustrare i filtri dell'azione. Un filtro azione è un attributo che è possibile applicare a un'azione del controller - o a un intero controller - che modifica il modo in cui viene eseguita l'azione. Il framework di MVC ASP.NET include alcuni filtri azione:

- OutputCache: il filtro azione memorizza nella cache l'output di un'azione del controller per un periodo di tempo specificato.
- Metodo HandleError – questo filtro azione gestisce gli errori generati quando viene eseguita un'azione del controller.
- Autorizzare: il filtro di azione consente di limitare l'accesso a un particolare utente o ruolo.

È inoltre possibile creare propri filtri azione personalizzati. Potrebbe ad esempio, si desidera creare un filtro azione personalizzato per implementare un sistema di autenticazione personalizzato. In alternativa, si potrebbe voler creare un filtro azione che modifica i dati della visualizzazione restituiti da un'azione del controller.

In questa esercitazione imparare a creare un filtro di azione da zero. Si crea un filtro di azione Log che registra diverse fasi dell'elaborazione di un'azione nella finestra di Output di Visual Studio.

### <a name="using-an-action-filter"></a>Utilizza un filtro azione

Un filtro azione è un attributo. È possibile applicare la maggior parte dei filtri azione un'azione di singoli controller o un controller di intero.

Ad esempio, il controller di dati nel listato 1 espone un'azione denominata `Index()` che restituisce l'ora corrente. Questa azione è decorata con il `OutputCache` filtro azione. Questo filtro, il valore restituito dall'azione da memorizzare nella cache per 10 secondi.

**Elenco 1: `Controllers\DataController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample1.vb)]

Se si richiama più volte il `Index()` azione immettendo l'URL/dati/indice nella barra degli indirizzi del browser e l'aggiornamento di raggiungere pulsante più volte, si vedrà contemporaneamente per 10 secondi. L'output del `Index()` azione viene memorizzato nella cache per 10 secondi (vedere la figura 1).


[![Tempo memorizzati nella cache](understanding-action-filters-vb/_static/image2.png)](understanding-action-filters-vb/_static/image1.png)

**Figura 01**: momento memorizzata nella cache ([fare clic per visualizzare l'immagine ingrandita](understanding-action-filters-vb/_static/image3.png))


Nel listato 1, un filtro di azione singola – il `OutputCache` è applicato il filtro di azione: il `Index()` metodo. Se è necessario, è possibile applicare più filtri azione per la stessa azione. Ad esempio, è possibile applicare sia la `OutputCache` e `HandleError` filtri azione per la stessa azione.

Nel listato 1, il `OutputCache` è applicato il filtro di azione di `Index()` azione. È inoltre possibile applicare questo attributo per il `DataController` classe stessa. In tal caso, il risultato restituito da qualsiasi azione esposta dal controller di sarebbe memorizzare nella cache per 10 secondi.

### <a name="the-different-types-of-filters"></a>I diversi tipi di filtri

Il framework ASP.NET MVC supporta quattro tipi di filtri:

1. Filtri di autorizzazione: implementa la `IAuthorizationFilter` attributo.
2. Filtri azione: implementa la `IActionFilter` attributo.
3. Risultato filtri: implementa la `IResultFilter` attributo.
4. I filtri eccezioni – implementa il `IExceptionFilter` attributo.

I filtri vengono eseguiti nell'ordine elencato in precedenza. Ad esempio, i filtri di autorizzazione vengono eseguiti sempre prima dei filtri azione e i filtri eccezioni vengono sempre eseguiti dopo ogni altro tipo di filtro.

Filtri di autorizzazione vengono utilizzati per implementare l'autenticazione e autorizzazione per le azioni del controller. Ad esempio, il filtro di autorizzazione è un esempio di un filtro di autorizzazione.

Filtri azione contengono la logica che viene eseguita prima e dopo l'esecuzione di un'azione del controller. È possibile utilizzare un filtro di azione, ad esempio, per modificare i dati della visualizzazione che restituisce un'azione del controller.

Filtri dei risultati contengono la logica che viene eseguita prima e dopo l'esecuzione di un risultato della visualizzazione. Ad esempio, si potrebbe voler modificare di un risultato della visualizzazione destro del mouse prima che il rendering nel browser.

I filtri eccezioni sono l'ultimo tipo di filtro per l'esecuzione. È possibile utilizzare un filtro eccezioni per gestire gli errori generati dai risultati dell'azione controller o azioni del controller. È anche possibile utilizzare i filtri eccezioni per registrare gli errori.

Ogni tipo di filtro viene eseguita in un ordine particolare. Se si desidera controllare l'ordine in cui vengono eseguiti i filtri dello stesso tipo è possibile impostare proprietà Order di un filtro.

La classe base per tutti i filtri di azione è la `System.Web.Mvc.FilterAttribute` classe. Se si desidera implementare un particolare tipo di filtro, quindi è necessario creare una classe che eredita dalla classe base di filtro e implementa una o più degli IAuthorizationFilter, IActionFilter, IResultFilter o ExceptionFilter interfacce.

### <a name="the-base-actionfilterattribute-class"></a>La classe di Base ActionFilterAttribute

Per rendere più semplice per l'implementazione di un filtro azione personalizzato, il framework di MVC ASP.NET include una base `ActionFilterAttribute` classe. Questa classe implementa sia il `IActionFilter` e `IResultFilter` interfacce ed eredita la `Filter` classe.

La terminologia qui non è completamente coerenza. Tecnicamente, una classe che eredita dalla classe ActionFilterAttribute è sia un filtro azione che un filtro dei risultati. Tuttavia, nel senso separato, il filtro azioni di word viene utilizzato per fare riferimento a qualsiasi tipo di filtro nel framework di MVC ASP.NET.

La classe di base ActionFilterAttribute dispone dei metodi seguenti che è possibile eseguire l'override:

- OnActionExecuting: questo metodo viene chiamato prima che venga eseguita un'azione del controller.
- OnActionExecuted: questo metodo viene chiamato dopo l'esecuzione di un'azione del controller.
- OnResultExecuting: questo metodo viene chiamato prima dell'esecuzione di un risultato di azione del controller.
- OnResultExecuted: questo metodo viene chiamato dopo l'esecuzione di un risultato di azione del controller.

Nella sezione successiva, si vedrà come è possibile implementare ciascuno di questi metodi diversi.

### <a name="creating-a-log-action-filter"></a>Creazione di un filtro di azione di Log

Per illustrare come è possibile creare un filtro azione personalizzato, si creerà un filtro azione personalizzato che registra le fasi di elaborazione di un'azione del controller per la finestra di Output di Visual Studio. Il nostro `LogActionFilter` è contenuta nel listato 2.

**Elenco 2: `ActionFilters\LogActionFilter.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample2.vb)]

Nel listato 2, il `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, e `OnResultExecuted()` chiamano i metodi di `Log()` metodo. Il nome del metodo e i dati della route corrente viene passato per il `Log()` metodo. Il `Log()` metodo scrive un messaggio nella finestra di Output di Visual Studio (vedere la figura 2).


[![Scrittura nella finestra di Output di Visual Studio](understanding-action-filters-vb/_static/image5.png)](understanding-action-filters-vb/_static/image4.png)

**Figura 02**: la scrittura nella finestra di Output di Visual Studio ([fare clic per visualizzare l'immagine ingrandita](understanding-action-filters-vb/_static/image6.png))


Il controller Home nel listato 3 viene illustrato come è possibile applicare il filtro di azione di Log per una classe controller intero. Ogni volta che le azioni esposte dal controller Home vengono richiamate: sia il `Index()` metodo o `About()` metodo: le fasi di elaborazione azione vengono registrati nella finestra di Output di Visual Studio.

**Elenco di 3: `Controllers\HomeController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample3.vb)]

### <a name="summary"></a>Riepilogo

In questa esercitazione sono stati introdotti i filtri azione MVC ASP.NET. È stato sui quattro diversi tipi di filtri: filtri di autorizzazione, i filtri azione, filtri dei risultati e i filtri eccezioni. È stato inoltre relative alla base `ActionFilterAttribute` classe.

Infine, è stato descritto come implementare un filtro azione semplice. È stato creato un filtro di azione di Log che registra le fasi di elaborazione di un'azione del controller per la finestra di Output di Visual Studio.

> [!div class="step-by-step"]
> [Precedente](asp-net-mvc-routing-overview-vb.md)
> [Successivo](improving-performance-with-output-caching-vb.md)
