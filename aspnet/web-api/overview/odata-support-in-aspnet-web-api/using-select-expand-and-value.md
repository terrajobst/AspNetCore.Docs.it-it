---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Utilizza $select, $expand e in ASP.NET Web API 2 OData $value | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/11/2013
ms.topic: article
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: f229cdbd8850a787dd3585e0640e8e66f6109331
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508060"
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a>Utilizza $select, $expand e $value in ASP.NET Web API 2 OData
====================
da [Mike Wasson](https://github.com/MikeWasson)

Web API 2 aggiunge il supporto per i $expand, $select e opzioni $value OData. Queste opzioni consentono di controllare la rappresentazione che ottiene dal server di un client.

- **$expand** fa sì che le entità correlate essere incluse inline nella risposta.
- **$select** seleziona un subset di proprietà da includere nella risposta.
- **$value** Ottiene il valore di una proprietà non elaborato.

## <a name="example-schema"></a>Nello Schema di esempio

Per questo articolo, si utilizzerà un servizio OData che definisce tre entità: prodotto, fornitore e la categoria. Ogni prodotto dispone di una categoria e un fornitore.

![](using-select-expand-and-value/_static/image1.png)

Di seguito sono le classi c# che definiscono i modelli di entità:

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

Si noti che il `Product` classe definisce le proprietà di navigazione per il `Supplier` e `Category`. La `Category` classe definisce una proprietà di navigazione per i prodotti in ogni categoria.

Per creare un endpoint OData per questo schema, usare lo scaffolding di Visual Studio 2013, come descritto in [creazione di un OData Endpoint in ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md). Aggiungere controller separato per prodotto, categoria e fornitore.

## <a name="enabling-expand-and-select"></a>Abilitazione di $espandere e $select

In Visual Studio 2013, lo scaffolding di Web API OData viene creato un controller che automaticamente supporta $expand e $select. Per riferimento, di seguito sono i requisiti per supportare $espandere e $select in un controller.

Per delle raccolte, il controller `Get` metodo deve restituire un **IQueryable**.

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

Per le entità singole, restituiscono un **SingleResult&lt;T&gt;**, dove T è un **IQueryable** che contiene zero o un'entità.

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

Anche decorare il `Get` metodi con il **[Queryable]** attributo, come illustrato in frammenti di codice precedente. In alternativa, chiamare **EnableQuerySupport** sul **HttpConfiguration** oggetto all'avvio. (Per ulteriori informazioni, vedere [attivazione delle opzioni di Query OData](supporting-odata-query-options.md#enable).)

## <a name="using-expand"></a>Usando $espandere

Quando si eseguono query un'entità di OData o una raccolta, la risposta predefinita non include le entità correlate. Ad esempio, ecco la risposta predefinita per il set di entità categorie:

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

Come si può notare, la risposta non include tutti i prodotti, anche se l'entità Category è un collegamento di navigazione di prodotti. Tuttavia, il client può usare $espandere per ottenere l'elenco dei prodotti per ogni categoria. Opzione $expand va inserito nella stringa di query della richiesta:

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

Ora il server includerà i prodotti per ogni categoria, in linea con le categorie. Di seguito è riportato il payload di risposta:

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

Si noti che ogni voce nella matrice di "value" contiene un elenco di prodotti.

I $expand opzione accetta separati da virgola elenco di proprietà di navigazione da espandere. La richiesta seguente si espande la categoria e il fornitore per un prodotto.

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

Di seguito è riportato il corpo della risposta:

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

È possibile espandere più di un livello di proprietà di navigazione. Nell'esempio seguente include tutti i prodotti per una categoria e il fornitore per ogni prodotto.

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

Di seguito è riportato il corpo della risposta:

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

Per impostazione predefinita, l'API Web limita la profondità di espansione massima a 2. Che impedisce al client di inviare richieste complesse, quali `$expand=Orders/OrderDetails/Product/Supplier/Region`, che potrebbe risultare inefficace per eseguire query e creare le risposte di grandi dimensioni. Per ignorare il valore predefinito, impostare il **MaxExpansionDepth** proprietà il **[Queryable]** attributo.

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

Per ulteriori informazioni sulla $expand opzione, vedere [sistema opzione Query Expand ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) nella documentazione ufficiale di OData.

## <a name="using-select"></a>Utilizzando $select

L'opzione $select specifica un subset di proprietà da includere nel corpo della risposta. Ad esempio, per ottenere solo il nome e il prezzo di ogni prodotto, usare la query seguente:

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

Di seguito è riportato il corpo della risposta:

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

È possibile combinare $select e $expand nella stessa query. Assicurarsi di includere la proprietà estesa nell'opzione $select. Ad esempio, la richiesta seguente ottiene il nome del prodotto e il fornitore.

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

Di seguito è riportato il corpo della risposta:

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

È anche possibile selezionare le proprietà all'interno di una proprietà espansa. La richiesta seguente espande i prodotti e seleziona il nome di categoria e il nome di prodotto.

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

Di seguito è riportato il corpo della risposta:

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

Per ulteriori informazioni sull'opzione $select, vedere [selezionare l'opzione di Query di sistema ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) nella documentazione ufficiale di OData.

## <a name="getting-individual-properties-of-an-entity-value"></a>Recupero di singole proprietà di un'entità ($value)

Esistono due modi per un client OData per ottenere una singola proprietà di un'entità. Il client può ottenere il valore in formato OData, oppure ottenere il valore della proprietà non elaborato.

La richiesta seguente ottiene una proprietà in formato OData.

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

Ecco un esempio di risposta in formato JSON:

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

Per ottenere il valore della proprietà non elaborato, aggiungere all'URI $value:

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

Di seguito è riportata la risposta. Si noti che il tipo di contenuto "text/plain", non è JSON.

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

Per supportare le query nel controller OData, aggiungere un metodo denominato `GetProperty`, dove `Property` è il nome della proprietà. Ad esempio, verrebbe denominato il metodo per ottenere la proprietà nome `GetName`. Il metodo deve restituire il valore della proprietà:

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
