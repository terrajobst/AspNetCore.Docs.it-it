---
title: Richieste di gestione con i controller in ASP.NET MVC di base
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 07/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/actions
ms.openlocfilehash: 99dcf1bd4f0dc4fcb6169f48bd398c9e40c21a35
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="handling-requests-with-controllers-in-aspnet-core-mvc"></a>Richieste di gestione con i controller in ASP.NET MVC di base

Da [Steve Smith](https://ardalis.com/) e [Scott Addie](https://github.com/scottaddie)

I controller, azioni e risultati dell'azione sono una parte fondamentale della modalità con cui gli sviluppatori di compilare App scritte in ASP.NET MVC di base.

## <a name="what-is-a-controller"></a>Che cos'è un Controller?

Un controller viene utilizzato per definire e raggruppare un set di azioni. Un'azione (o *metodo di azione*) è un metodo in un controller che gestisce le richieste. I controller di raggruppano operazioni simili. Questa aggregazione di azioni consente a un set comune di regole, ad esempio il routing, la memorizzazione nella cache e l'autorizzazione, da applicare collettivamente. Le richieste vengono mappate alle azioni tramite [routing](xref:mvc/controllers/routing).

Per convenzione, le classi controller:
* Si trovano nel livello di radice del progetto *controller* cartella
* Ereditare da`Microsoft.AspNetCore.Mvc.Controller`

Un controller è una classe istanziabile in cui almeno una delle seguenti condizioni è true:
* Il nome della classe viene aggiunto il suffisso "Controller"
* La classe eredita da una classe il cui nome viene aggiunto il suffisso "Controller"
* La classe decorata con il `[Controller]` attributo

Una classe controller non deve avere un oggetto associato `[NonController]` attributo.

Controller devono seguire il [principio dipendenze esplicite](http://deviq.com/explicit-dependencies-principle/). Esistono due approcci per l'implementazione di questo principio. Se più azioni del controller richiedono lo stesso servizio, è consigliabile utilizzare [inserimento costruttore](xref:mvc/controllers/dependency-injection#constructor-injection) per richiedere tali dipendenze. Se il servizio è necessario da un metodo di azione singola, è consigliabile utilizzare [attacco intrusivo nel codice di azione](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) per richiedere la dipendenza.

All'interno di **M**enti -**V**ISTA -**C**ontroller, un controller è responsabile per l'elaborazione della richiesta iniziale e la creazione di istanze del modello. In genere, le decisioni aziendali devono essere eseguite all'interno del modello.

Il controller accetta i risultati dell'elaborazione del modello di (se presente) e restituisce la visualizzazione corretta e i dati di visualizzazione associata oppure il risultato della chiamata API. Altre informazioni, vedere [Panoramica di MVC ASP.NET Core](xref:mvc/overview) e [Guida introduttiva a Visual Studio e ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).

Il controller è un *a livello di interfaccia utente* astrazione. Le responsabilità sono per verificare che i dati di richiesta siano validi e di scegliere quale visualizzazione (o risultato di un'API) deve essere restituito. Nelle App ben fornita direttamente non include dati accesso o la regola business. Al contrario, il controller di delega per servizi per la gestione di tali responsabilità.

## <a name="defining-actions"></a>Definizione di azioni

Metodi pubblici in un controller, ad eccezione di quelle decorata con il `[NonAction]` attributo, sono azioni. Parametri sulle azioni sono associati a dati della richiesta e vengono convalidati utilizzando [associazione del modello](xref:mvc/models/model-binding). Convalida del modello si verifica per tutto il modello di associazione. Il `ModelState.IsValid` valore della proprietà indica se l'associazione di modelli e la convalida ha avuto esito positivo.

Metodi di azione devono contenere la logica per il mapping di una richiesta di un problema di business. Problemi aziendali devono essere rappresentate in genere come servizi che consente di accedere mediante il controller [inserimento di dipendenze](xref:mvc/controllers/dependency-injection). Azioni mappare il risultato dell'azione di business a uno stato dell'applicazione.

Azioni possono restituire alcun valore, ma spesso restituire un'istanza di `IActionResult` (o `Task<IActionResult>` per i metodi asincroni) che produce una risposta. Il metodo di azione è responsabile della scelta *il tipo di risposta*. Il risultato dell'azione *la risposta*.

### <a name="controller-helper-methods"></a>Metodi di supporto del controller

Controller è in genere ereditano da [Controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller), anche se questo non è necessario. Derivazione da `Controller` fornisce l'accesso a tre categorie di metodi di supporto:

#### <a name="1-methods-resulting-in-an-empty-response-body"></a>1. Metodi, creando un corpo della risposta vuota

Non `Content-Type` intestazione della risposta HTTP è incluso, poiché non dispone del contenuto per descrivere il corpo della risposta.

Esistono due tipi di risultati all'interno di questa categoria: reindirizzamento e codice di stato HTTP.

* **Codice di stato HTTP**

    Questo tipo viene restituito un codice di stato HTTP. Alcuni metodi di supporto di questo tipo sono `BadRequest`, `NotFound`, e `Ok`. Ad esempio, `return BadRequest();` produce un codice di 400 stato quando eseguita. Quando i metodi come `BadRequest`, `NotFound`, e `Ok` vengono sottoposti a overload, non è più considerata i risponditori codice di stato HTTP, poiché si sta eseguendo la negoziazione del contenuto.

* **Reindirizzamento**

    Questo tipo restituisce un reindirizzamento a un'azione o una destinazione (utilizzando `Redirect`, `LocalRedirect`, `RedirectToAction`, o `RedirectToRoute`). Ad esempio, `return RedirectToAction("Complete", new {id = 123});` reindirizza a `Complete`, passando un oggetto anonimo.

    Il tipo di risultato di reindirizzamento è diverso dal tipo di codice di stato HTTP principalmente per l'aggiunta di un `Location` intestazione risposta HTTP.

#### <a name="2-methods-resulting-in-a-non-empty-response-body-with-a-predefined-content-type"></a>2. Metodi, creando un corpo della risposta non vuoto con un tipo di contenuto predefinito.

La maggior parte dei metodi di supporto di questa categoria includono un `ContentType` proprietà, che consente di impostare il `Content-Type` intestazione della risposta per descrivere il corpo della risposta.

Esistono due tipi di risultati all'interno di questa categoria: [vista](xref:mvc/views/overview) e [risposta formattato](xref:mvc/models/formatting).

* **Visualizza**

    Questo tipo restituisce una vista che utilizza un modello per il rendering HTML. Ad esempio, `return View(customer);` passa un modello per la visualizzazione per l'associazione dati.

* **Risposta formattato**

    Questo tipo restituisce JSON o un formato di scambio di dati simili per rappresentare un oggetto in modo specifico. Ad esempio, `return Json(customer);` serializza l'oggetto specificato in formato JSON.
    
    Altri metodi più comuni di questo tipo sono `File`, `PhysicalFile`, e `VirtualFile`. Ad esempio, `return PhysicalFile(customerFilePath, "text/xml");` restituisce un file XML descritto da un `Content-Type` valore dell'intestazione di risposta "text/XML".

#### <a name="3-methods-resulting-in-a-non-empty-response-body-formatted-in-a-content-type-negotiated-with-the-client"></a>3. Metodi, creando un corpo della risposta non vuoto formattato in un tipo di contenuto negoziato con il client

Questa categoria è noto come **negoziazione del contenuto**. [Negoziazione di contenuto](xref:mvc/models/formatting#content-negotiation) applica ogni volta che un'azione restituisce un [ObjectResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.objectresult) tipo o un valore diverso da un [IActionResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iactionresult) implementazione. Un'azione che restituisce un non -`IActionResult` implementazione (ad esempio, `object`) restituisce inoltre una risposta formattato.

Alcuni metodi di supporto di questo tipo includono `BadRequest`, `CreatedAtRoute`, e `Ok`. Esempi di questi metodi includono `return BadRequest(modelState);`, `return CreatedAtRoute("routename", values, newobject);`, e `return Ok(value);`, rispettivamente. Si noti che `BadRequest` e `Ok` eseguono negoziazione del contenuto solo quando viene passato un valore; senza viene passato un valore, vengono invece utilizzati come tipi di risultati di codice di stato HTTP. Il `CreatedAtRoute` (metodo), d'altra parte, viene sempre eseguita negoziazione del contenuto dall'overload tutti richiedono di essere passato un valore.

### <a name="cross-cutting-concerns"></a>Problemi di montaggio incrociato

Le applicazioni condividono in genere parte del flusso di lavoro. Ad esempio un'app che richiede l'autenticazione per accedere il carrello acquisti o un'app che memorizza nella cache di dati in alcune pagine. Per eseguire la logica prima o dopo un metodo di azione, utilizzare un *filtro*. Utilizzando [filtri](xref:mvc/controllers/filters) sugli aspetti a montaggio incrociato può ridurre la duplicazione, consentendo loro di seguire il [principio non ripetere manualmente (sorgente)](http://deviq.com/don-t-repeat-yourself/).

Più di filtrare gli attributi, ad esempio `[Authorize]`, può essere applicato a livello di controller o un'azione in base al livello desiderato di granularità.

Gestione degli errori e la memorizzazione nella cache di risposta sono spesso problemi di montaggio incrociato:
   * [Gestione degli errori](xref:mvc/controllers/filters#exception-filters)
   * [Memorizzazione nella cache delle risposte](xref:performance/caching/response)

Numerosi problemi di montaggio incrociato possono essere gestiti utilizzando filtri o personalizzata [middleware](xref:fundamentals/middleware).
