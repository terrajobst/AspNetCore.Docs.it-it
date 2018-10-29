---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routing in API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a7bc998fc23c0453fc9cd6ac1e7b9af7bd516225
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207303"
---
<a name="routing-in-aspnet-web-api"></a>Routing in API Web ASP.NET
====================
da [Mike Wasson](https://github.com/MikeWasson)

Questo articolo descrive come API Web ASP.NET instrada le richieste HTTP al controller.

> [!NOTE]
> Se si ha familiarità con ASP.NET MVC, API Web di routing è molto simile a il routing MVC. La differenza principale è che l'API Web Usa il verbo HTTP, non il percorso dell'URI, selezionare l'azione. È inoltre possibile utilizzare routing in stile MVC nell'API Web. Questo articolo non presuppone alcuna conoscenza di ASP.NET MVC.

## <a name="routing-tables"></a>Tabelle di routing

Nell'API Web ASP.NET, un *controller* è una classe che gestisce le richieste HTTP. I metodi del controller pubblici vengono chiamati *metodi di azione* o semplicemente *azioni*. Quando il framework API Web riceve una richiesta, indirizza la richiesta a un'azione.

Per determinare l'azione da richiamare, il framework Usa un *tabella di routing*. Il modello di progetto di Visual Studio per l'API Web crea una route predefinita:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

Questa route è definita nel *WebApiConfig.cs* file, che viene inserito nella *App\_avviare* directory:

![](routing-in-aspnet-web-api/_static/image1.png)

Per altre informazioni sul `WebApiConfig` classe, vedere [configurazione di ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).

Se self-hosting di API Web, è necessario impostare la tabella di routing direttamente sul `HttpSelfHostConfiguration` oggetto. Per altre informazioni, vedere [self-hosting un'API Web](../older-versions/self-host-a-web-api.md).

Ogni voce nella tabella di routing contiene un *modello di route*. Il modello di route predefinito per l'API Web &quot;api / {controller} / {id}&quot;. In questo modello &quot;api&quot; è un segmento di percorso letterale e {controller} e {id} sono variabili di segnaposto.

Quando il framework API Web riceve una richiesta HTTP, Cerca la corrispondenza con l'URI in base a uno dei modelli di route nella tabella di routing. Se nessuna route corrisponde, il client riceve un errore 404. Ad esempio, gli URI seguenti corrispondono alla route predefinita:

- / api/contacts
- /API/Contacts/1
- /API/Products/gizmo1

Tuttavia, l'URI seguente non corrisponde, perché manca il &quot;api&quot; segmento:

- / contacts/1

> [!NOTE]
> Il motivo per l'uso di "api" nella route è per evitare conflitti con il routing di ASP.NET MVC. In questo modo, è possibile avere &quot;/contatta&quot; passare a un controller MVC, e &quot;/api/contacts&quot; passare a un controller API Web. Naturalmente, se non si desidera che questa convenzione, è possibile modificare la tabella di route predefiniti.

Una volta trovata una route corrispondente, API Web consente di selezionare il controller e l'azione:

- Per trovare il controller, API Web aggiunge &quot;Controller&quot; al valore della *{controller}* variabile.
- Per trovare l'azione, API Web esamina il verbo HTTP e quindi cerca un'azione il cui nome inizia con lo stesso nome del verbo HTTP. Ad esempio, con una richiesta GET, API Web di Azure cerca un'azione con prefisso &quot;ottenere&quot;, ad esempio &quot;GetContact&quot; oppure &quot;GetAllContacts&quot;. Questa convenzione viene applicata solo per GET, POST, PUT, DELETE, HEAD, opzioni e applicare PATCH di verbi. È possibile abilitare altri verbi HTTP utilizzando attributi nel controller. Vedremo un esempio in un secondo momento.
- Altre variabili di segnaposto nel modello di route, ad esempio *{id},* vengono mappati ai parametri di azione.

Esaminiamo un esempio. Si supponga di definisce il controller seguente:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

Ecco alcune possibili richieste HTTP, insieme all'azione che viene richiamato per ogni:

| Verbo HTTP | Percorso dell'URI | Operazione | Parametro |
| --- | --- | --- | --- |
| GET | API/prodotti | GetAllProducts | *(nessuno)* |
| GET | API/prodotti/4 | GetProductById | 4 |
| DELETE | API/prodotti/4 | DeleteProduct | 4 |
| INSERISCI | API/prodotti | *(nessuna corrispondenza trovata)* |  |

Si noti che il *{id}* segmento dell'URI, se presente, viene eseguito il mapping per il *id* parametro dell'azione. In questo esempio, il controller definisce due metodi GET, uno con un *id* parametro e uno senza parametri.

Inoltre, tenere presente che la richiesta POST non riuscirà, perché il controller non definisce un &quot;Post... &quot; (metodo).

## <a name="routing-variations"></a>Variazioni di routine

La sezione precedente descrive il meccanismo di routing di base per ASP.NET Web API. Questa sezione descrive alcune variazioni.

### <a name="http-verbs"></a>Verbi HTTP

Invece di usare la convenzione di denominazione per i verbi HTTP, è possibile specificare il verbo HTTP per un'azione in modo esplicito tramite la decorazione del metodo di azione con uno dei seguenti attributi:

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

Nell'esempio seguente, il `FindProduct` metodo viene eseguito il mapping alle richieste GET:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Per consentire più verbi HTTP per un'azione o per consentire i verbi HTTP diversi da GET, PUT, POST, DELETE, HEAD, opzioni e delle PATCH, usare il `[AcceptVerbs]` attributo, che accetta un elenco di verbi HTTP.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Routing in base al nome di azione

Con il modello di routing predefinito, API Web Usa il verbo HTTP per selezionare l'azione. Tuttavia, è anche possibile creare una route in cui il nome dell'azione è incluso nell'URI:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

In questo modello di route, il *{action}* il metodo di azione nel controller di nomi dei parametri. Con questo stile di routing, usare gli attributi per specificare i verbi HTTP consentiti. Si supponga, ad esempio, che il controller è il metodo seguente:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

In questo caso, una richiesta GET per "api/prodotti/dettagli/1" esegue il mapping al `Details` (metodo). Questo stile di routing è simile a ASP.NET MVC e potrebbe essere appropriato per un'API di tipo RPC.

È possibile sostituire il nome dell'azione utilizzando il `[ActionName]` attributo. Nell'esempio seguente, esistono due azioni che eseguono il mapping a &quot;api/prodotti/miniature/*id*. Uno supporta GET e l'altro supporta POST:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>Non-azioni

Per evitare che un metodo richiamato come un'azione, usare il `[NonAction]` attributo. Segnala al framework che il metodo non è un'azione, anche se le regole di routing in caso contrario, in base alla distinzione.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>Ulteriori informazioni

In questo argomento è fornita una panoramica del routing. Per altre informazioni, vedere [Routing e selezione dell'azione](routing-and-action-selection.md), che descrive esattamente come il framework corrisponde a un URI a una route, seleziona un controller e quindi seleziona l'azione da richiamare.
