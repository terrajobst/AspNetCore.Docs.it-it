---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Azioni e funzioni in OData v4 tramite ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: In OData, funzioni e le azioni sono un modo per aggiungere comportamenti lato server che non sono definiti facilmente come operazioni CRUD su entità. Questa esercitazione viene illustrato come...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 4e61c6b4bf59792b6570e32e6d24635d4f5e5ac3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838017"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>Azioni e funzioni in OData v4 tramite ASP.NET Web API 2.2
====================
da [Mike Wasson](https://github.com/MikeWasson)

> In OData, funzioni e le azioni sono un modo per aggiungere comportamenti lato server che non sono definiti facilmente come operazioni CRUD su entità. Questa esercitazione illustra come aggiungere azioni e funzioni a un endpoint OData v4, usando l'API Web 2.2. L'esercitazione si basa sull'esercitazione [creare un OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - API Web 2.2
> - OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Versioni dell'esercitazione
> 
> Per OData versione 3, vedere [azioni OData nell'API Web ASP.NET 2](../odata-v3/odata-actions.md).


La differenza tra *azioni* e *funzioni* è che le azioni possono avere effetti collaterali e non funzioni. Le azioni e funzioni possono restituire dati. Alcuni usi della azioni includono:

- Transazioni complesse.
- La modifica più entità in una sola volta.
- Consentire gli aggiornamenti solo a determinate proprietà di un'entità.
- L'invio di dati che non è un'entità.

Le funzioni sono utili per la restituzione di informazioni che non corrispondono direttamente a un'entità o una raccolta.

Un'azione (o (funzione)) è destinata a una singola entità o una raccolta. Nella terminologia di OData, questo è il *associazione*. È anche possibile avere &quot;non associato&quot; azioni/funzioni, che vengono chiamate come operazioni statiche nel servizio.

## <a name="example-adding-an-action"></a>Esempio: Aggiunta di un'azione

È importante definire un'azione per valutare un prodotto.

> [!NOTE]
> Questa esercitazione si basa sull'esercitazione [creare un OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)


In primo luogo, aggiungere un `ProductRating` modello per rappresentare le classificazioni.

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

Aggiungere anche un **DbSet** per il `ProductsContext` classe, in modo che Entity Framework creerà una tabella di classificazione nel database.

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>Aggiungere l'azione per il modello EDM

In WebApiConfig.cs, aggiungere il codice seguente:

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

Il **EntityTypeConfiguration.Action** metodo aggiunge un'azione a entity data model (EDM). Il **parametro** metodo specifica un parametro tipizzato per l'azione.

Questo codice imposta inoltre lo spazio dei nomi per il modello EDM. Lo spazio dei nomi è importante perché l'URI per l'azione include il nome dell'azione completo:

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> In una tipica configurazione di IIS, il punto in questo URL causerà IIS restituire l'errore 404. È possibile risolvere il problema aggiungendo la sezione seguente al file Web. config:

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>Aggiungere un metodo del Controller per l'azione

Per abilitare la &quot;tasso&quot; azione, aggiungere il metodo seguente alla `ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

Si noti che il nome del metodo corrisponda al nome dell'azione. Il **[HttpPost]** attributo specifica il metodo è un metodo HTTP POST.

Per richiamare l'azione, il client invia una richiesta POST HTTP simile al seguente:

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

Il &quot;frequenza&quot; azione è associata a istanze del prodotto, in modo che l'URI per l'azione è il nome dell'azione completo aggiunto all'URI dell'entità. (Tenere presente che lo spazio dei nomi EDM è impostato su &quot;ProductService&quot;, quindi è il nome dell'azione completo &quot;ProductService.Rate&quot;.)

Il corpo della richiesta contiene i parametri dell'azione come payload JSON. API Web converte automaticamente il payload JSON per un **ODataActionParameters** oggetto, che è semplicemente un dizionario di valori di parametro. Usare questo dizionario per accedere ai parametri nel metodo controller.

Se il client invia i parametri dell'azione in errate formattare, il valore di **ModelState** è false. Selezionare questo flag nel metodo di controller e restituire un errore se **IsValid** è false.

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>Esempio: Aggiunta di una funzione

A questo punto è possibile aggiungere una funzione OData che restituisce il prodotto più costoso. Come prima, il primo passaggio viene aggiunta la funzione al modello EDM. In WebApiConfig.cs, aggiungere il codice seguente.

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

In questo caso, la funzione è associata la raccolta di prodotti, anziché singole istanze di Product. I client di richiamano la funzione inviando una richiesta GET:

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

Ecco il metodo di controller per questa funzione:

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

Si noti che il nome del metodo corrisponda al nome della funzione. Il **[HttpGet]** attributo specifica il metodo è un metodo HTTP GET.

Ecco la risposta HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>Esempio: Aggiunta di una funzione non associata

Nell'esempio precedente è stato una funzione associata a una raccolta. In questo esempio, si creerà un' *non associato* (funzione). Le funzioni non associate vengono chiamate come operazioni statiche nel servizio. In questo esempio la funzione restituirà l'imposta sulle vendite per un determinato codice postale.

Nel file WebApiConfig, aggiungere la funzione al modello EDM:

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

Si noti che qui viene chiamato **funzione** direttamente sulle **ODataModelBuilder**, anziché il tipo di entità o la raccolta. In questo modo il generatore di modelli che la funzione è non associata.

Ecco il metodo del controller che implementa la funzione:

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

Non importa quale controller API Web è inserire in questo metodo. È possibile inserirlo `ProductsController`, o definire un controller distinto. Il **[ODataRoute]** attributo definisce il modello URI per la funzione.

Di seguito è riportato un esempio di richiesta client:

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

La risposta HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
