---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Supporto di opzioni Query OData in ASP.NET Web API 2 | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/04/2013
ms.topic: article
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 004c029db6f01627f7cadff26aaf5554ce2b93a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>Supporto di opzioni Query OData in ASP.NET Web API 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

OData definisce i parametri che possono essere usati per modificare una query OData. Il client invia questi parametri nella stringa di query dell'URI della richiesta. Per ordinare i risultati, ad esempio, un client utilizza il parametro $orderby:

`http://localhost/Products?$orderby=Name`

La specifica OData chiama questi parametri *opzioni query*. È possibile abilitare le opzioni di query OData per qualsiasi controller API Web nel progetto &#8212; il controller non è necessario essere un endpoint OData. Questo offre un modo pratico per aggiungere funzionalità di filtro e ordinamento a qualsiasi applicazione Web API.

Prima di abilitare le opzioni di query, leggere l'argomento [OData Security Guidance Center](odata-security-guidance.md).

- [L'attivazione di opzioni Query OData](#enable)
- [Query di esempio](#examples)
- [Paging basato su server](#server-paging)
- [Limitando le opzioni di Query](#limiting_query_options)
- [Richiamare direttamente le opzioni di Query](#ODataQueryOptions)
- [Convalida delle query](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>L'attivazione di opzioni Query OData

API Web supporta le opzioni di query OData seguenti:

| Opzione | Descrizione |
| --- | --- |
| $expand | Espande le entità correlate inline. |
| $filter | Filtra i risultati, in base a una condizione booleana. |
| $inlinecount | Indica al server di includere il numero totale di entità corrispondente nella risposta. (Utile per il paging sul lato server). |
| $orderby | Ordina i risultati. |
| $select | Seleziona le proprietà da includere nella risposta. |
| $skip | Ignora i primi n risultati. |
| $top | Restituisce solo il primo n i risultati. |

Per utilizzare opzioni di query OData, è necessario consentire in modo esplicito. È possibile abilitare a livello globale per l'intera applicazione o abilitare per i controller specifici o azioni specifiche.

Per abilitare le opzioni di query OData a livello globale, chiamare **EnableQuerySupport** sul **HttpConfiguration** classe all'avvio:

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

Il **EnableQuerySupport** metodo abilita le opzioni di query a livello globale per qualsiasi azione del controller che restituisce un **IQueryable** tipo. Se non si desidera opzioni query abilitate per l'intera applicazione, è possibile consentire per le azioni del controller specifico aggiungendo il **[Queryable]** attributo al metodo di azione.

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>Query di esempio

Questa sezione illustra i tipi di query che sono possibili utilizzando le opzioni di query OData. Per informazioni dettagliate sulle opzioni di query, vedere la documentazione di OData in [www.odata.org](http://www.odata.org/).

Per informazioni su $espandere e $select, vedere [utilizza $select, $expand, $value in ASP.NET Web API OData e](using-select-expand-and-value.md).

**Paging basato su client**

Per il set di entità di grandi dimensioni, il client potrebbe voler limitare il numero di risultati. Ad esempio, un client potrebbe essere 10 voci contemporaneamente, con collegamenti "Avanti" per ottenere la pagina successiva di risultati. A tale scopo, il client utilizza le opzioni $top e $skip.

`http://localhost/Products?$top=10&$skip=20`

L'opzione $top fornisce il numero massimo di voci da restituire, mentre l'opzione $skip fornisce il numero di voci da ignorare. Nell'esempio precedente recupera le voci compresi tra 21 e 30.

**Applicazione di filtri**

L'opzione $filter consente a un client di filtrare i risultati applicando un'espressione booleana. Le espressioni di filtro sono molto potente. includono operatori logici e aritmetici, funzioni stringa e funzioni di Data.

| Restituire tutti i prodotti con la categoria è uguale a "Toys". | `http://localhost/Products?$filter=Category`EQ 'Toys' |
| --- | --- |
| Restituire tutti i prodotti con prezzo inferiore a 10. | `http://localhost/Products?$filter=Price`lt 10 |
| Operatori logici: restituire tutti i prodotti in cui il prezzo > = 5 e price < = 15. | `http://localhost/Products?$filter=Price`Ge 5 e prezzo le 15 |
| Funzioni stringa: restituire tutti i prodotti con "zz" nel nome. | `http://localhost/Products?$filter=substringof('zz',Name)` |
| Funzioni di data: restituire tutti i prodotti con ReleaseDate dopo 2005. | `http://localhost/Products?$filter=year(ReleaseDate)`gt 2005 |

**Ordinamento**

Per ordinare i risultati, utilizzare il filtro $orderby.

| Ordinamento in base al prezzo. | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| Ordinamento in base al prezzo in senso decrescente (più alto al più basso). | `http://localhost/Products?$orderby=Price desc` |
| Ordinare per categoria, quindi ordinare in base al prezzo in ordine decrescente all'interno delle categorie. | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>Paging basato su server

Se il database contiene milioni di record, non si desidera inviare loro in un payload. Per evitare questo problema, il server può limitare il numero di voci che viene inviato in una singola risposta. Per abilitare il paging del server, impostare il **PageSize** proprietà il **Queryable** attributo. Il valore è il numero massimo di voci da restituire.

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

Se il controller viene restituito il formato OData, il corpo della risposta conterrà un collegamento alla pagina successiva di dati:

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

Il client può utilizzare questo collegamento per recuperare la pagina successiva. Per individuare il numero totale di voci nel set di risultati, il client può impostare l'opzione di query $inlinecount con il valore "allpages".

`http://localhost/Products?$inlinecount=allpages`

Il valore "allpages" indica al server di includere il conteggio totale nella risposta:

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> I collegamenti alla pagina successiva e conteggio inline richiedono entrambi il formato OData. Il motivo è che OData definisce campi speciali nel corpo della risposta per contenere il collegamento e il conteggio.


Per i formati non OData, è comunque possibile supporta il numero di collegamenti e inline alla pagina successiva, inserendo i risultati della query in un **PageResult&lt;T&gt;**  oggetto. Tuttavia, è necessario codice aggiuntivo. Ecco un esempio:

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

Di seguito è riportato un esempio di risposta JSON:

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>Limitando le opzioni di Query

Le opzioni di query fornire al client un grande controllo sulle query che viene eseguita nel server. In alcuni casi, si potrebbe voler limitare le opzioni disponibili per motivi di sicurezza o di prestazioni. Il **[Queryable]** attributo sia alcuni compilato nelle proprietà per questo oggetto. Ecco alcuni esempi.

Consenti solo $skip e $top, per supportare il paging e nient'altro:

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

Consente l'ordinamento solo per determinate proprietà impedire l'ordinamento sulle proprietà che non sono indicizzate nel database:

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

Consenti la funzione logica "eq" ma non altre funzioni logiche:

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

Non consentire tutti gli operatori aritmetici:

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

È possibile limitare le opzioni a livello globale costruendo un **QueryableAttribute** istanza e passarlo al **EnableQuerySupport** funzione:

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>Richiamare direttamente le opzioni di Query

Anziché utilizzare il **[Queryable]** attributo, è possibile richiamare le opzioni di query direttamente nel controller. A tale scopo, aggiungere un **ODataQueryOptions** parametro al metodo del controller. In questo caso, non è necessario il **[Queryable]** attributo.

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

API Web consente di popolare il **ODataQueryOptions** stringa di query dell'URI. Per applicare la query, passare un **IQueryable** per il **ApplyTo** metodo. Il metodo restituisce un altro **IQueryable**.

Per scenari avanzati, se non è un **IQueryable** provider di query, è possibile esaminare il **ODataQueryOptions** e convertire le opzioni di query in un altro formato. (Ad esempio, vedere di RaghuRam Nadiminti post di blog [query OData conversione HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), che include anche un [esempio](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)

<a id="query-validation"></a>
## <a name="query-validation"></a>Convalida delle query

Il **[Queryable]** attributo convalida la query prima dell'esecuzione. Il passaggio di convalida viene eseguito nel **QueryableAttribute.ValidateQuery** metodo. È anche possibile personalizzare il processo di convalida.

Vedere anche [OData Security Guidance Center](odata-security-guidance.md).

In primo luogo, eseguire l'override di uno dei validator classi che è definito nel **Web.Http.OData.Query.Validators** dello spazio dei nomi. Ad esempio, la classe di convalida seguente disabilita l'opzione 'desc' per l'opzione $orderby.

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

Sottoclasse di **[Queryable]** attributo per eseguire l'override di **ValidateQuery** metodo.

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

Quindi impostare l'attributo personalizzato sia a livello globale o controller:

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

Se si utilizza **ODataQueryOptions** direttamente, impostare la convalida per le opzioni:

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
