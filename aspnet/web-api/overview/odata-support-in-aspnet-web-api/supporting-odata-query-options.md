---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Che supportano le opzioni di Query OData nell'API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/04/2013
ms.topic: article
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 3ce2b38a13e8684a88bb0ce6183671fae98795c7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380843"
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>Supportare opzioni di Query OData nell'API Web ASP.NET 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

OData definisce parametri che possono essere usati per modificare una query OData. Il client invia questi parametri nella stringa di query dell'URI della richiesta. Per ordinare i risultati, ad esempio, un client usa il parametro $orderby:

`http://localhost/Products?$orderby=Name`

La specifica OData chiama questi parametri *opzioni query*. È possibile abilitare le opzioni di query OData per i controller API Web nel progetto &#8212; il controller non è necessario essere un endpoint OData. Ciò offre un modo pratico per aggiungere funzionalità come filtro e ordinamento a qualsiasi applicazione API Web.

Prima di abilitare le opzioni di query, leggere l'argomento [OData Security Guidance](odata-security-guidance.md).

- [Abilitazione delle opzioni di Query OData](#enable)
- [Query di esempio](#examples)
- [Paging guidato da server](#server-paging)
- [Limitando le opzioni di Query](#limiting_query_options)
- [Richiamare direttamente le opzioni di Query](#ODataQueryOptions)
- [Convalida query](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>Abilitazione delle opzioni di Query OData

API Web supporta le opzioni di query OData seguenti:

| Opzione | Descrizione |
| --- | --- |
| $expand | Espande le entità correlate inline. |
| $filter | Filtra i risultati, in base a una condizione booleana. |
| $inlinecount | Indica al server di includere il conteggio totale di entità corrispondenti nella risposta. (Utile per il paging del lato server). |
| $orderby | Ordina i risultati. |
| $select | Seleziona le proprietà da includere nella risposta. |
| $skip | Ignora i primi n risultati. |
| $top | Restituisce solo i primi n risultati. |

Per usare le opzioni di query OData, è necessario consentire in modo esplicito. È possibile abilitarli a livello globale per l'intera applicazione o abilitarli per controller o azioni specifiche specifiche.

Per abilitare le opzioni di query OData a livello globale, chiamare **EnableQuerySupport** nel **HttpConfiguration** classe all'avvio:

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

Il **EnableQuerySupport** metodo abilita le opzioni di query a livello globale per qualsiasi azione del controller che restituisce un' **IQueryable** tipo. Se non si vuole abilitate per l'intera applicazione opzioni di query, è possibile abilitarli per le azioni del controller specifico mediante l'aggiunta del **[Queryable]** attributo al metodo di azione.

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>Query di esempio

Questa sezione illustra i tipi di query che sono possibili utilizzando le opzioni di query OData. Per informazioni dettagliate sulle opzioni di query, fare riferimento alla documentazione di OData [www.odata.org](http://www.odata.org/).

Per informazioni su $espandere e $select, vedere [Usa $select, $expand, $value in ASP.NET Web API OData e](using-select-expand-and-value.md).

**Paging basato su client**

Per i set di entità di grandi dimensioni, il client potrebbe voler limitare il numero di risultati. Ad esempio, un client potrebbe mostrare 10 voci alla volta, con collegamenti "Avanti" per accedere alla pagina successiva di risultati. A tale scopo, il client usa le opzioni $top e $skip.

`http://localhost/Products?$top=10&$skip=20`

L'opzione $top fornisce il numero massimo di voci da restituire, mentre l'opzione $skip fornisce il numero di voci da ignorare. Nell'esempio precedente recupera le voci 21 e 30.

**Applicazione di filtri**

L'opzione $filter consente a un client di filtrare i risultati applicando un'espressione booleana. Le espressioni di filtro sono molto potenti; includono gli operatori logici e aritmetici, funzioni stringa e funzioni di Data.

| Restituire tutti i prodotti con la categoria è uguale a "Toys". | `http://localhost/Products?$filter=Category` EQ 'Toys' |
| --- | --- |
| Restituire tutti i prodotti con prezzo minore di 10. | `http://localhost/Products?$filter=Price` lt 10 |
| Gli operatori logici: restituire tutti i prodotti in cui price > = 5 e il prezzo < = 15. | `http://localhost/Products?$filter=Price` Ge 5 e prezzo le 15 |
| Funzioni stringa: restituisca tutti i prodotti con "zz" nel nome. | `http://localhost/Products?$filter=substringof('zz',Name)` |
| Le funzioni di data: restituisce tutti i prodotti con ReleaseDate dopo 2005. | `http://localhost/Products?$filter=year(ReleaseDate)` gt 2005 |

**L'ordinamento**

Per ordinare i risultati, utilizzare i filtro $orderby.

| Ordinamento in base al prezzo. | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| Ordinamento in base al prezzo in senso decrescente (più alto al più basso). | `http://localhost/Products?$orderby=Price desc` |
| Ordinare per categoria, quindi ordinare in base al prezzo in senso decrescente all'interno delle categorie. | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>Paging guidato da server

Se il database contiene milioni di record, non si desidera inviare loro in un payload. Per evitare questo problema, il server può limitare il numero di voci che invia in un'unica risposta. Per abilitare il paging del server, impostare il **PageSize** proprietà di **Queryable** attributo. Il valore è il numero massimo di voci da restituire.

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

Se il controller restituisce formato OData, il corpo della risposta conterrà un collegamento alla pagina successiva di dati:

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

Il client può utilizzare questo collegamento per recuperare la pagina successiva. Per individuare il numero totale di voci del set di risultati, il client può impostare l'opzione di query $inlinecount con il valore "allpages".

`http://localhost/Products?$inlinecount=allpages`

Il valore "allpages" indica al server di includere il conteggio totale nella risposta:

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> I collegamenti alla pagina successiva e conteggio inline richiedono entrambi il formato OData. Il motivo è che OData definisce i campi speciali nel corpo della risposta per contenere il collegamento e il conteggio.


Per i formati non OData, è comunque possibile supportare conteggio inline e i collegamenti di pagina successiva, includendo i risultati della query in una **PageResult&lt;T&gt;**  oggetto. Tuttavia, è necessario un po' più codice. Ecco un esempio:

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

Di seguito è riportato un esempio di risposta JSON:

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>Limitando le opzioni di Query

Le opzioni di query restituito al client un ampio controllo sulla query che viene eseguita nel server. In alcuni casi, si potrebbe voler limitare le opzioni disponibili per motivi di sicurezza o di prestazioni. Il **[Queryable]** attributo ha alcuni incorporati nelle proprietà per questo oggetto. Ecco alcuni esempi.

Consenti solo $skip e $top, per supportare il paging e nient'altro:

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

Consenti ordinamento solo per determinate proprietà evitare che l'ordinamento in base a proprietà che non vengono indicizzate nel database:

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

Consenti la funzione logica "eq" ma non altre funzioni logiche:

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

Non consentire tutti gli operatori aritmetici:

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

È possibile limitare le opzioni a livello globale costruendo un **QueryableAttribute** istanza e passarlo al **EnableQuerySupport** funzione:

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>Richiamare direttamente le opzioni di Query

Invece di usare la **[Queryable]** attributo, è possibile richiamare le opzioni di query direttamente nel controller di. A tale scopo, aggiungere un' **ODataQueryOptions** parametro al metodo del controller. In questo caso, non è necessario il **[Queryable]** attributo.

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

API Web popola la **ODataQueryOptions** dall'URI stringa di query. Per applicare la query, passare un **IQueryable** per il **ApplyTo** (metodo). Il metodo restituisce un'altra **IQueryable**.

Per scenari avanzati, se non è un **IQueryable** provider di query, è possibile esaminare le **ODataQueryOptions** e convertire le opzioni di query in un altro formato. (Ad esempio, vedere di RaghuRam Nadiminti post di blog [le query OData traduzione HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), che include anche una [esempio](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)

<a id="query-validation"></a>
## <a name="query-validation"></a>Convalida query

Il **[Queryable]** attributo convalida la query prima dell'esecuzione. Il passaggio di convalida viene eseguito nel **QueryableAttribute.ValidateQuery** (metodo). È anche possibile personalizzare il processo di convalida.

Vedere anche [OData Security Guidance](odata-security-guidance.md).

In primo luogo, eseguire l'override di uno dei validator le classi che è definito nel **Web.Http.OData.Query.Validators** dello spazio dei nomi. Ad esempio, la classe di convalida seguente disabilita l'opzione 'desc' per l'opzione $orderby.

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

Sottoclasse di **[Queryable]** attributo per eseguire l'override di **ValidateQuery** (metodo).

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

Quindi impostare l'attributo personalizzato sia a livello globale o per ogni controller:

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

Se si usa **ODataQueryOptions** direttamente, impostare il validator per le opzioni:

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
