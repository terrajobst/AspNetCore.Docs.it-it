---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Azioni e funzioni in OData v4 tramite ASP.NET Web API 2.2 | Documenti Microsoft
author: MikeWasson
description: In OData, funzioni e le azioni sono un modo per aggiungere comportamenti sul lato server che non possono essere definiti facilmente come operazioni CRUD su entità. In questa esercitazione viene illustrato come...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 532362f0c0faaaf0cb0c04726856f0497e5261b5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2018
ms.locfileid: "26508230"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>Azioni e funzioni in OData v4 tramite ASP.NET Web API 2.2
====================
da [Mike Wasson](https://github.com/MikeWasson)

> In OData, funzioni e le azioni sono un modo per aggiungere comportamenti sul lato server che non possono essere definiti facilmente come operazioni CRUD su entità. In questa esercitazione viene illustrato come aggiungere un endpoint OData v4 con Web API 2.2 funzioni e le azioni. L'esercitazione si basa sull'esercitazione [creare un OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - 2.2 API Web
> - OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Versioni dell'esercitazione
> 
> Per OData versione 3, vedere [le azioni in ASP.NET Web API 2 OData](../odata-v3/odata-actions.md).


La differenza tra *azioni* e *funzioni* è che le azioni possono avere effetti collaterali e non funzioni. Le azioni e funzioni possono restituire dati. Alcuni tipi di utilizzo per le azioni includono:

- Transazioni complesse.
- Modifica di alcune entità in una sola volta.
- Consentire aggiornamenti solo a determinate proprietà di un'entità.
- L'invio dei dati che non è un'entità.

Le funzioni sono utili per la restituzione di informazioni che non corrispondono direttamente a un'entità o una raccolta.

Un'azione (o una funzione) può far riferimento una singola entità o una raccolta. Nella terminologia di OData, si tratta di *associazione*. È anche possibile &quot;non associato&quot; azioni/funzioni, che vengono chiamate come statiche operazioni nel servizio.

## <a name="example-adding-an-action"></a>Esempio: Aggiunta di un'azione

È pertanto possibile definire un'azione per valutare un prodotto.

> [!NOTE]
> Questa esercitazione si basa sull'esercitazione [creare un OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)


Aggiungere innanzitutto un `ProductRating` modello per rappresentare le classificazioni.

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

Aggiungere anche un **DbSet** per la `ProductsContext` classe, in modo che EF verrà creata una tabella di classificazioni nel database.

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>Aggiungere l'azione di EDM

In WebApiConfig.cs, aggiungere il codice seguente:

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

Il **EntityTypeConfiguration.Action** metodo aggiunge un'azione a entity data model (EDM). Il **parametro** metodo specifica un parametro tipizzato per l'azione.

Questo codice imposta anche lo spazio dei nomi per il modello EDM. Lo spazio dei nomi è importante perché l'URI per l'azione include il nome dell'azione completo:

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> In una configurazione tipica di IIS, il punto in questo URL causerà IIS restituire l'errore 404. È possibile risolvere il problema aggiungendo la seguente sezione al file Web. config:

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>Aggiungere un metodo del Controller per l'azione

Per abilitare il &quot;frequenza&quot; azione, aggiungere il seguente metodo alla `ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

Si noti che il nome del metodo corrisponde al nome di azione. Il **[HttpPost]** attributo specifica il metodo è un metodo HTTP POST.

Per richiamare l'azione, il client invia una richiesta HTTP POST simile al seguente:

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

Il &quot;frequenza&quot; azione è associata a istanze del prodotto, pertanto l'URI per l'azione è il nome dell'azione completo aggiunto all'URI dell'entità. (Tenere presente che lo spazio dei nomi EDM è impostato su &quot;ProductService&quot;, pertanto è il nome dell'azione completo &quot;ProductService.Rate&quot;.)

Il corpo della richiesta contiene i parametri dell'azione come un payload JSON. API Web converte automaticamente il payload JSON per un **ODataActionParameters** oggetto, che è semplicemente un dizionario di valori di parametro. Utilizzare questo dizionario per accedere ai parametri del metodo del controller.

Se il client invia i parametri dell'azione in errato formattare, il valore di **ModelState.IsValid** è false. Controllare questo flag nel metodo di controller e viene restituito un errore se **IsValid** è false.

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>Esempio: Aggiunta di una funzione

Ora aggiungere una funzione OData che restituisce il prodotto più costoso. Come prima, il primo passaggio consiste nell'aggiungere la funzione a EDM. In WebApiConfig.cs, aggiungere il codice seguente.

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

In questo caso, la funzione è associata la raccolta di prodotti, anziché singole istanze di prodotto. I client di richiamare la funzione inviando una richiesta GET:

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

Ecco il metodo del controller per questa funzione:

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

Si noti che il nome del metodo corrisponde al nome di funzione. Il **[HttpGet]** attributo specifica il metodo è un metodo HTTP GET.

Di seguito è riportata la risposta HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>Esempio: Aggiunta di una funzione non associata

Nell'esempio precedente è una funzione associata a una raccolta. Nell'esempio seguente, si creerà un *non associato* (funzione). Funzioni non associate vengono chiamate come statiche operazioni nel servizio. In questo esempio la funzione restituirà l'IVA per un determinato codice postale.

Nel file WebApiConfig, aggiungere la funzione EDM:

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

Si noti che stiamo chiamando il **funzione** direttamente sul **ODataModelBuilder**, anziché il tipo di entità o la raccolta. Ciò indica che la funzione è associata il generatore di modelli.

Ecco il metodo del controller che implementa la funzione:

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

Non è importante inserire questo metodo in quale controller di Web API. È stato possibile inserirla `ProductsController`, o definire controller separato. Il **[ODataRoute]** attributo definisce il modello URI per la funzione.

Di seguito è riportato un esempio di richiesta client:

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

La risposta HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
