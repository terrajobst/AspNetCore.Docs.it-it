---
uid: web-api/overview/error-handling/exception-handling
title: Gestione delle eccezioni in ASP.NET Web API | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2012
ms.topic: article
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: c65ddcca012840d70ab5a33af92edb30041be971
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506960"
---
<a name="exception-handling-in-aspnet-web-api"></a>Gestione delle eccezioni in ASP.NET Web API
====================
da [Mike Wasson](https://github.com/MikeWasson)

Questo articolo descrive l'errore e gestione delle eccezioni nell'API Web ASP.NET.

- [HttpResponseException](#httpresponserexception)
- [Filtri eccezioni](#exception_filters)
- [Registrazione di filtri eccezioni](#registering_exception_filters)
- [Errore HTTP](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

Cosa accade se un controller API Web genera un'eccezione non rilevata. Per impostazione predefinita, la maggior parte delle eccezioni vengono convertite in una risposta HTTP con codice di stato 500 Errore interno del Server.

Il **HttpResponseException** tipo è un caso speciale. Questa eccezione restituisce qualsiasi codice di stato HTTP specificato nel costruttore eccezione. Ad esempio, il metodo seguente restituisce 404, non viene trovato, se il *id* parametro non è valido.

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

Per un maggiore controllo sulla risposta, è inoltre possibile costruire l'intero messaggio di risposta e includerla con il **HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>Filtri eccezioni

È possibile personalizzare come API Web gestisce le eccezioni scrivendo un *filtro eccezioni*. Un filtro eccezioni viene eseguito quando un metodo del controller genera qualsiasi eccezione non gestita che è *non* un **HttpResponseException** eccezione. Il **HttpResponseException** tipo è un caso speciale, poiché è progettato specificamente per la restituzione di una risposta HTTP.

Implementano i filtri eccezioni di **System.Web.Http.Filters.IExceptionFilter** interfaccia. È il modo più semplice per scrivere un filtro eccezioni da cui derivare il **System.Web.Http.Filters.ExceptionFilterAttribute** classe ed eseguire l'override di **OnException** metodo.

> [!NOTE]
> I filtri eccezioni nell'API Web ASP.NET sono simili a quelle in ASP.NET MVC. Tuttavia, vengono dichiarati in un spazio dei nomi separato e una funzione separatamente. In particolare, il **HandleErrorAttribute** classe utilizzata in MVC non gestisce le eccezioni generate dai controller API Web.


Ecco un filtro che converte **NotImplementedException** eccezioni nel codice di stato HTTP 501-, non di codice:

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

Il **risposta** proprietà del **HttpActionExecutedContext** oggetto contiene il messaggio di risposta HTTP che verrà inviato al client.

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>Registrazione di filtri eccezioni

Esistono diversi modi per registrare un filtro eccezioni di API Web:

- Da un'azione
- Dal controller
- A livello globale

Per applicare il filtro a un'azione specifica, aggiungere il filtro come attributo per l'azione:

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

Per applicare il filtro per tutte le azioni in un controller, aggiungere il filtro come attributo per la classe controller:

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

Per applicare il filtro a livello globale in tutti i controller API Web, aggiungere un'istanza del filtro per il **GlobalConfiguration.Configuration.Filters** insieme. Filtri di eccezione in questa raccolta si applicano a qualsiasi azione del controller API Web.

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

Se si utilizza il modello di progetto "Applicazione Web di ASP.NET MVC 4" per creare il progetto, inserire il codice di configurazione Web API all'interno di `WebApiConfig` (classe), che si trova nell'App\_cartella di avvio:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>Errore HTTP

Il **HttpError** oggetto fornisce un sistema coerente per restituire informazioni sugli errori nel corpo della risposta. Nell'esempio seguente viene illustrato come restituire il codice di stato HTTP 404 (non trovato) con un **HttpError** nel corpo della risposta.

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse** è un metodo di estensione definito nel **System.Net.Http.HttpRequestMessageExtensions** classe. Internamente, **CreateErrorResponse** crea un **HttpError** istanza e quindi crea un **HttpResponseMessage** che contiene il **HttpError**.

In questo esempio, se il metodo ha esito positivo, restituisce il prodotto nella risposta HTTP. Ma se il prodotto richiesto non viene trovato, la risposta HTTP contiene un **HttpError** nel corpo della richiesta. La risposta potrebbe essere simile al seguente:

[!code-console[Main](exception-handling/samples/sample9.cmd)]

Si noti che il **HttpError** è stato serializzato in JSON in questo esempio. Un vantaggio dell'utilizzo **HttpError** è che passano attraverso lo stesso [negoziazione di contenuto](../formats-and-model-binding/content-negotiation.md) e processo di serializzazione come qualsiasi altro modello fortemente tipizzato.

### <a name="httperror-and-model-validation"></a>Errore HTTP e la convalida del modello

Per la convalida del modello, è possibile passare lo stato del modello per **CreateErrorResponse**, per includere gli errori di convalida nella risposta:

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

In questo esempio potrebbe restituire la risposta seguente:

[!code-console[Main](exception-handling/samples/sample11.cmd)]

Per ulteriori informazioni sulla convalida del modello, vedere [convalida del modello in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

### <a name="using-httperror-with-httpresponseexception"></a>Con errore HTTP HttpResponseException

Negli esempi precedenti vengono restituiti un **HttpResponseMessage** messaggio dall'azione del controller, ma è anche possibile usare **HttpResponseException** per restituire un **HttpError**. Ciò consente di restituire un modello fortemente tipizzato nel caso di esito positivo normale, pur restituendo **HttpError** se si verifica un errore:

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
