---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Usa $select, $expand e $value in ASP.NET Web API 2 OData | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 10/11/2013
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: c5c0d374a918706e65254121ba5fa1516afdaf75
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814459"
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a>Usa $select, $expand e $value in ASP.NET Web API 2 OData
====================
da [Mike Wasson](https://github.com/MikeWasson)

API Web 2 aggiunge il supporto per il $expand, $select $value opzioni e in OData. Queste opzioni consentono a un client controllare la rappresentazione che ottiene risposta dal server.

- **$expand** fa sì che le entità correlate possono essere incluse inline nella risposta.
- **$select** seleziona un subset di proprietà da includere nella risposta.
- **$value** Ottiene il valore di una proprietà non elaborato.

## <a name="example-schema"></a>Schema di esempio

In questo articolo userà un servizio OData che definisce tre entità: prodotto, fornitore e categoria. Ogni prodotto dispone di una categoria e un unico fornitore.

![](using-select-expand-and-value/_static/image1.png)

Di seguito sono le classi di c# che definiscono i modelli di entità:

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

Si noti che il `Product` classe definisce le proprietà di navigazione per il `Supplier` e `Category`. Il `Category` classe definisce una proprietà di navigazione per i prodotti in ogni categoria.

Per creare un endpoint OData per questo schema, usare lo scaffolding di Visual Studio 2013, come descritto in [creazione di un OData Endpoint nell'API Web ASP.NET](odata-v3/creating-an-odata-endpoint.md). Aggiungere controller separato per prodotto, categoria e fornitore.

## <a name="enabling-expand-and-select"></a>Abilitazione di $espandere e $select

In Visual Studio 2013, lo scaffolding API Web OData crea un controller che supporta automaticamente $expand e $select. Per riferimento, ecco i requisiti per supportare $espandere e $select in un controller.

Per delle raccolte, il controller `Get` metodo deve restituire un **IQueryable**.

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

Per le entità singole, restituiscono un **SingleResult&lt;T&gt;**, dove T è un **IQueryable** che contiene zero o un'entità.

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

Inoltre, decorare il `Get` metodi con il **[Queryable]** dell'attributo, come illustrato nei frammenti di codice precedente. In alternativa, chiamare **EnableQuerySupport** nel **HttpConfiguration** oggetto all'avvio. (Per altre informazioni, vedere [attivazione delle opzioni di Query OData](supporting-odata-query-options.md#enable).)

## <a name="using-expand"></a>Usa $espandere

Quando si eseguono query un'entità OData o una raccolta, la risposta predefinita non include le entità correlate. Ad esempio, ecco la risposta predefinita per il set di entità categorie:

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

Come può notare, la risposta non include tutti i prodotti, anche se l'entità Category è un collegamento di navigazione di prodotti. Tuttavia, il client può usare $espandere per visualizzare l'elenco dei prodotti per ogni categoria. Opzione $expand va inserito nella stringa di query della richiesta:

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

Ora il server includerà i prodotti per ogni categoria, in linea con le categorie. Ecco il payload di risposta:

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

Si noti che ogni voce nella matrice "value" contiene un elenco di prodotti.

Il $expand opzione elenco accetta un delimitato da virgole delle proprietà di navigazione da espandere. La richiesta seguente espande sia la categoria e il fornitore per un prodotto.

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

Di seguito è riportato il corpo della risposta:

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

È possibile espandere più di un livello di proprietà di navigazione. L'esempio seguente include tutti i prodotti per una categoria e anche il fornitore per ogni prodotto.

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

Di seguito è riportato il corpo della risposta:

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

Per impostazione predefinita, API Web consente di limitare la profondità di espansione massima a 2. Che impedisce il client di inviare le richieste complesse, ad esempio `$expand=Orders/OrderDetails/Product/Supplier/Region`, che potrebbe risultare inefficace per eseguire query e creare le risposte di grandi dimensioni. Per sostituire il valore predefinito, impostare il **MaxExpansionDepth** proprietà di **[Queryable]** attributo.

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

Per altre informazioni sulla $expand opzione, vedere [Query opzione di sistema Expand ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) nella documentazione ufficiale relativa a OData.

## <a name="using-select"></a>Usando $select

L'opzione $select specifica un subset delle proprietà da includere nel corpo della risposta. Ad esempio, per ottenere solo il nome e il prezzo di ogni prodotto, usare la query seguente:

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

Di seguito è riportato il corpo della risposta:

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

È possibile combinare $select e $expand nella stessa query. Assicurarsi di includere la proprietà estesa nell'opzione $select. Ad esempio, la richiesta seguente ottiene il nome del prodotto e il fornitore.

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

Di seguito è riportato il corpo della risposta:

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

È anche possibile selezionare le proprietà all'interno di una proprietà espansa. La richiesta seguente espande i prodotti e consente di selezionare il nome di categoria e il nome del prodotto.

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

Di seguito è riportato il corpo della risposta:

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

Per altre informazioni sull'opzione $select, vedere [selezionare l'opzione di Query di sistema ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) nella documentazione ufficiale relativa a OData.

## <a name="getting-individual-properties-of-an-entity-value"></a>Recupero di singole proprietà di un'entità ($value)

Esistono due modi per ottenere una singola proprietà da un'entità di un client OData. Il client può ottenere il valore in formato OData oppure ottenere il valore non elaborato della proprietà.

La richiesta seguente ottiene una proprietà nel formato OData.

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

Ecco un esempio di risposta in formato JSON:

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

Per ottenere il valore non elaborato della proprietà, aggiungere $value all'URI:

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

Ecco la risposta. Si noti che il tipo di contenuto "text/plain", non è JSON.

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

Per supportare queste query in controller di OData, aggiungere un metodo denominato `GetProperty`, dove `Property` è il nome della proprietà. Ad esempio, il metodo da ottenere la proprietà Name deve essere denominato `GetName`. Il metodo deve restituire il valore di tale proprietà:

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
