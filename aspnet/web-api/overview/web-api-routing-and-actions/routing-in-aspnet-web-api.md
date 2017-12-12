---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routing di ASP.NET Web API | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2012
ms.topic: article
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: aa0ecc96029051fef6a81ac08f7fcf52a24de59c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="routing-in-aspnet-web-api"></a>Routing di ASP.NET Web API
====================
da [Mike Wasson](https://github.com/MikeWasson)

In questo articolo viene descritto come ASP.NET Web API instrada le richieste HTTP per i controller.

> [!NOTE]
> Se si ha familiarità con ASP.NET MVC, API Web routing è molto simile per il routing MVC. La differenza principale è che l'API Web utilizza il metodo HTTP, non il percorso dell'URI, per selezionare l'azione. È anche possibile utilizzare il routing MVC stile nell'API Web. In questo articolo non si presuppone la conoscenza di ASP.NET MVC.


## <a name="routing-tables"></a>Tabelle di routing

In ASP.NET Web API, un *controller* è una classe che gestisce le richieste HTTP. Vengono chiamati i metodi pubblici del controller *metodi di azione* o semplicemente *azioni*. Quando il framework di API Web riceve una richiesta, la richiesta viene indirizzata a un'azione.

Per determinare l'azione da richiamare, il framework utilizza un *tabella di routing*. Il modello di progetto di Visual Studio per l'API Web crea una route predefinita:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

La route è definita nel file WebApiConfig.cs, che viene inserito nell'App\_directory di avvio:

![](routing-in-aspnet-web-api/_static/image1.png)

Per ulteriori informazioni sul **WebApiConfig** classe, vedere [la configurazione di ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).

Se l'API Web indipendente, è necessario impostare la tabella di routing direttamente sul **HttpSelfHostConfiguration** oggetto. Per ulteriori informazioni, vedere [indipendente un'API Web](../older-versions/self-host-a-web-api.md).

Ogni voce nella tabella di routing contiene un *modello di route*. Il modello di route predefinita per l'API Web è &quot;api / {controller} / {id}&quot;. In questo modello, &quot;api&quot; è un segmento di percorso letterale e {controller} e {id} sono variabili di segnaposto.

Quando il framework di API Web riceve una richiesta HTTP, tenta di mettere in corrispondenza l'URI su uno dei modelli della route nella tabella di routing. Se nessuna route corrispondente, il client riceve un errore 404. Ad esempio, il seguente URI corrispondenza la route predefinita:

- / api/contatti
- /API/Contacts/1
- /API/Products/gizmo1

Tuttavia, l'URI seguente non corrisponde, perché manca il &quot;api&quot; segmento:

- contatti/1

> [!NOTE]
> Il motivo per l'uso di "api" nella route è per evitare conflitti con il routing di ASP.NET MVC. In questo modo, è possibile avere &quot;/contatta&quot; passare a un controller MVC, e &quot;/api/contatti&quot; passare a un controller API Web. Naturalmente, se non si desidera che questa convenzione, è possibile modificare la tabella di route predefinita.

Una volta trovata una route corrispondente, API Web consente di selezionare il controller e l'azione:

- Per trovare il controller, API Web aggiunge &quot;Controller&quot; il valore di *{controller}* variabile.
- Per trovare l'azione, API Web analizza il metodo HTTP e cercherà un'azione il cui nome inizia con lo stesso nome di metodo HTTP. Ad esempio, con una richiesta GET, API Web cerca un'azione che inizia con &quot;Get... &quot;, ad esempio &quot;GetContact&quot; o &quot;GetAllContacts&quot;. Questa convenzione si applica solo a GET, POST, PUT e DELETE di metodi. È possibile abilitare altri metodi HTTP tramite attributi nel controller. Vedremo un esempio di che in un secondo momento.
- Altre variabili di segnaposto nel modello di route, ad esempio *{id},* mappati ai parametri dell'azione.

Esaminiamo un esempio. Si supponga di definisce il seguente controller:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

Ecco alcune possibili richieste HTTP, insieme all'azione che viene richiamato per ogni:

| Metodo HTTP | Percorso URI | Azione | Parametro |
| --- | --- | --- | --- |
| GET | API/prodotti | GetAllProducts | *(none)* |
| GET | prodotti/API/4 | GetProductById | 4 |
| DELETE | prodotti/API/4 | DeleteProduct | 4 |
| INSERISCI | API/prodotti | *(nessuna corrispondenza)* |  |

Si noti che il *{id}* segmento dell'URI, se presente, viene eseguito il mapping per il *id* parametro dell'azione. In questo esempio, il controller definisce due metodi GET, uno con un *id* parametro e uno senza parametri.

Inoltre, si noti che la richiesta POST avrà esito negativo, perché il controller non definisce un &quot;Post... &quot; metodo.

## <a name="routing-variations"></a>Variazioni di routine

La sezione precedente è descritto il meccanismo di routing di base per l'API Web ASP.NET. Questa sezione vengono descritte alcune varianti.

### <a name="http-methods"></a>Metodi HTTP

Anziché utilizzare la convenzione di denominazione per i metodi HTTP, è possibile specificare in modo esplicito il metodo HTTP per un'azione tramite il metodo di azione con la **HttpGet**, **HttpPut**, **HttpPost** , o **HttpDelete** attributo.

Nell'esempio seguente, il metodo FindProduct viene eseguito il mapping alle richieste GET:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Per consentire più metodi HTTP per un'azione, o per consentire a metodi HTTP diversi da GET, PUT, POST e DELETE, utilizzare il **AcceptVerbs** attributo, che accetta un elenco di metodi HTTP.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Routing in base al nome di azione

Con il modello di routing predefinito, API Web Usa il metodo HTTP per selezionare l'azione. Tuttavia, è anche possibile creare una route in cui il nome dell'azione è incluso nell'URI:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

In questo modello di route, il *{action}* nomi di parametro del metodo di azione nel controller. Con questo stile di routing, utilizzare gli attributi per specificare i metodi HTTP consentiti. Ad esempio, si supponga che il controller sia il metodo seguente:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

In questo caso, sarebbe eseguire il mapping di una richiesta GET per "api/prodotti/dettagli/1" al metodo di dettagli. Questo stile di routing è simile a ASP.NET MVC e potrebbe essere appropriato per un'API di stile RPC.

È possibile ignorare il nome dell'azione utilizzando il **ActionName** attributo. Nell'esempio seguente, sono disponibili due azioni che eseguono il mapping a &quot;prodotti/api/anteprima/*id*. Uno supporta GET e l'altro supporta POST:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>Non azioni

Per impedire che un metodo richiamato come un'azione, utilizzare il **NonAction** attributo. Segnala al framework che il metodo non è un'azione, anche se le regole di routing in caso contrario sarebbe corrisponde.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>Ulteriori informazioni

In questo argomento viene fornita una panoramica generale del routing. Per ulteriori dettagli, vedere [Routing e la selezione di azione](routing-and-action-selection.md), che descrive esattamente come il framework corrisponde a un URI a una route, seleziona un controller e quindi seleziona l'azione da richiamare.
