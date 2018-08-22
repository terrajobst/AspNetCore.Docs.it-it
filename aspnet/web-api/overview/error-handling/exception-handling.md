---
uid: web-api/overview/error-handling/exception-handling
title: Gestione delle eccezioni in API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 03/12/2012
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: 62e6187cd82252e7d30f21e03cc4d08418fa39ee
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826284"
---
<a name="exception-handling-in-aspnet-web-api"></a>Gestione delle eccezioni in API Web ASP.NET
====================
da [Mike Wasson](https://github.com/MikeWasson)

Questo articolo descrive l'errore e gestione delle eccezioni in API Web ASP.NET.

- [HttpResponseException](#httpresponserexception)
- [Filtri eccezioni](#exception_filters)
- [La registrazione di filtri eccezioni](#registering_exception_filters)
- [Errore HTTP](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

Cosa accade se un controller Web API genera un'eccezione non rilevata. Per impostazione predefinita, la maggior parte delle eccezioni vengono convertite in una risposta HTTP con codice di stato 500, errore interno del Server.

Il **HttpResponseException** tipo è un caso speciale. Questa eccezione restituisce qualsiasi codice di stato HTTP specificato nel costruttore di eccezione. Ad esempio, il metodo seguente restituisce 404, non viene trovato, se il *id* parametro non è valido.

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

Per un maggiore controllo sulla risposta, è possibile costruire l'intero messaggio di risposta e includerla con il **HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>Filtri eccezioni

È possibile personalizzare la modalità di gestione eccezioni API Web mediante la scrittura di un *filtro eccezioni*. Un filtro eccezioni viene eseguito quando un metodo del controller genera un'eccezione non gestita viene *non* un' **HttpResponseException** eccezione. Il **HttpResponseException** tipo è un caso speciale, poiché è progettato specificamente per restituire una risposta HTTP.

I filtri eccezioni implementano il **System.Web.Http.Filters.IExceptionFilter** interfaccia. Il modo più semplice per scrivere un filtro eccezioni è derivare dal **System.Web.Http.Filters.ExceptionFilterAttribute** classe ed eseguire l'override di **OnException** (metodo).

> [!NOTE]
> I filtri eccezioni in API Web ASP.NET sono simili a quelle di ASP.NET MVC. Tuttavia, vengono dichiarati in un spazio dei nomi separato e funzione separatamente. In particolare, il **HandleErrorAttribute** classe usata nel MVC non gestisce le eccezioni generate da controller API Web.


Ecco un filtro che converte **NotImplementedException** eccezioni nel codice di stato HTTP 501, non è implementato del codice:

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

Il **risposta** proprietà delle **HttpActionExecutedContext** oggetto contiene il messaggio di risposta HTTP che verrà inviato al client.

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>La registrazione di filtri eccezioni

Esistono diversi modi per registrare un filtro eccezioni API Web:

- Mediante un'azione
- Dal controller
- A livello globale

Per applicare il filtro a un'azione specifica, aggiungere il filtro come attributo per l'azione:

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

Per applicare il filtro a tutte le azioni in un controller, aggiungere il filtro come attributo alla classe controller:

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

Per applicare il filtro a livello globale in tutti i controller API Web, aggiungere un'istanza del filtro per il **GlobalConfiguration.Configuration.Filters** raccolta. Eccezione filtri in questa raccolta si applicano a qualsiasi azione del controller API Web.

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

Se si usa il modello di progetto "Applicazione Web di ASP.NET MVC 4" per creare il progetto, inserire il codice di configurazione Web API all'interno di `WebApiConfig` (classe), che si trova nell'App\_cartella di avvio:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>Errore HTTP

Il **HttpError** oggetto fornisce un modo coerente per restituire informazioni sull'errore nel corpo della risposta. Nell'esempio seguente viene illustrato come restituire il codice di stato HTTP 404 (non trovato) con un **HttpError** nel corpo della risposta.

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse** è un metodo di estensione definito nel **System.Net.Http.HttpRequestMessageExtensions** classe. Internamente, **CreateErrorResponse** consente di creare un **HttpError** dell'istanza e quindi crea un' **HttpResponseMessage** che contiene il **HttpError**.

In questo esempio, se il metodo ha esito positivo, restituisce il prodotto nella risposta HTTP. Ma se il prodotto richiesto non viene trovato, la risposta HTTP contiene un **HttpError** nel corpo della richiesta. La risposta potrebbe essere simile al seguente:

[!code-console[Main](exception-handling/samples/sample9.cmd)]

Si noti che il **HttpError** è stato serializzato in JSON in questo esempio. Un vantaggio dell'utilizzo **HttpError** è che passano attraverso lo stesso [la negoziazione del contenuto](../formats-and-model-binding/content-negotiation.md) e processo di serializzazione come qualsiasi altro modello fortemente tipizzato.

### <a name="httperror-and-model-validation"></a>Errore HTTP e la convalida del modello

Per la convalida del modello, è possibile passare lo stato del modello per **CreateErrorResponse**per includere gli errori di convalida nella risposta:

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

In questo esempio potrebbe restituire la risposta seguente:

[!code-console[Main](exception-handling/samples/sample11.cmd)]

Per altre informazioni sulla convalida del modello, vedere [convalida del modello in API Web ASP.NET](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

### <a name="using-httperror-with-httpresponseexception"></a>Uso di errore HTTP con HttpResponseException

Gli esempi precedenti restituiscono un **HttpResponseMessage** messaggio dall'azione del controller, ma è anche possibile usare **HttpResponseException** per restituire un' **HttpError**. Ciò consente di restituire un modello fortemente tipizzato in caso di esito positivo normale, pur restituendo **HttpError** se si è verificato un errore:

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
