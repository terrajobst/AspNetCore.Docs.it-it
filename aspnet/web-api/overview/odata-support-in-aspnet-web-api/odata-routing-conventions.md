---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: Convenzioni di routing in ASP.NET Web API 2 Odata | Microsoft Docs
author: MikeWasson
description: Questo articolo descrive le convenzioni di routing che usa API Web con gli endpoint OData.
ms.author: riande
ms.date: 07/31/2013
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 0199ccde100c8c3ab70db912b8ab984e0617f9dd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827102"
---
<a name="routing-conventions-in-aspnet-web-api-2-odata"></a>Convenzioni di routing in ASP.NET Web API 2 Odata
====================
da [Mike Wasson](https://github.com/MikeWasson)

> Questo articolo descrive le convenzioni di routing che usa API Web con gli endpoint OData.


Quando l'API Web riceve una richiesta di OData, esegue il mapping della richiesta a un nome di controller e un nome di azione. Il mapping si basa sul metodo HTTP e l'URI. Ad esempio, `GET /odata/Products(1)` esegue il mapping a `ProductsController.GetProduct`.

Nella parte 1 di questo articolo descrivono le convenzioni di routing OData predefinite. Queste convenzioni sono appositamente progettati per gli endpoint OData, e sostituire il sistema di routing di API Web predefinito. (La sostituzione avviene quando si chiama **MapODataRoute**.)

Nella parte 2, illustrato come aggiungere convenzioni di routing personalizzate. Attualmente le convenzioni predefinite non coprono l'intero intervallo di OData URIs, ma è possibile estendere in modo da gestire altri casi.

- [Convenzioni di Routing predefinite](#conventions)
- [Convenzioni di Routing personalizzate](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>Convenzioni di Routing predefinite

Prima descrivono le convenzioni di routing OData nell'API Web, è utile comprendere OData URIs. Un' [URI OData](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/) è costituito da:

- La radice del servizio
- Il percorso della risorsa
- Opzioni di query

![](odata-routing-conventions/_static/image1.png)

Per il routing, la parte importante è il percorso della risorsa. Il percorso della risorsa è suddivisa in segmenti. Ad esempio, `/Products(1)/Supplier` dispone di tre segmenti:

- `Products` fa riferimento a un set di entità denominato "Prodotti".
- `1` è una chiave di entità, la selezione di una singola entità dal set.
- `Supplier` è una proprietà di navigazione che consente di selezionare un'entità correlata.

Pertanto, questo percorso preleva il fornitore del prodotto 1.

> [!NOTE]
> I segmenti di percorso OData non sempre corrispondono a segmenti dell'URI. Ad esempio, "1" è considerato un segmento di percorso.


**Nomi dei controller.** Il nome del controller è sempre derivato dal set alla radice del percorso della risorsa di entità. Ad esempio, se è il percorso della risorsa `/Products(1)/Supplier`, API Web è simile per un controller denominato `ProductsController`.

**Nomi delle azioni.** I nomi delle azioni derivano dalla somma di segmenti del percorso ed entity data model (EDM), come indicato nelle tabelle seguenti. In alcuni casi, sono disponibili due opzioni per il nome dell'azione. Ad esempio, "Get" o &quot;GetProducts&quot;.

**Query su entità**

| Richiesta | URI di esempio | Nome dell'azione | Azione di esempio |
| --- | --- | --- | --- |
| OTTENERE /entityset | / I prodotti | GetEntitySet o Get | GetProducts |
| OTTENERE /entityset(key) | /Products(1) | GetEntityType o Get | GetProduct |
| OTTENERE /entityset (key) o eseguire il cast | / /Models.Book prodotti (1) | GetEntityType o Get | GetBook |

Per altre informazioni, vedere [creare un OData Endpoint di sola lettura](odata-v3/creating-an-odata-endpoint.md).

**Creazione, aggiornamento ed eliminazione di entità**

| Richiesta | URI di esempio | Nome dell'azione | Azione di esempio |
| --- | --- | --- | --- |
| POST /entityset | / I prodotti | PostEntityType o Post | PostProduct |
| Inserire /entityset(key) | /Products(1) | PutEntityType o Put | PutProduct |
| PUT /entityset (chiave) / eseguire il cast | / /Models.Book prodotti (1) | PutEntityType o Put | PutBook |
| PATCH /entityset(key) | /Products(1) | PatchEntityType o Patch | PatchProduct |
| Applicare PATCH /entityset (key) o eseguire il cast | / /Models.Book prodotti (1) | PatchEntityType o Patch | PatchBook |
| Elimina /entityset(key) | /Products(1) | DeleteEntityType o Delete | DeleteProduct |
| ELIMINARE /entityset (chiave) / eseguire il cast | / /Models.Book prodotti (1) | DeleteEntityType o Delete | DeleteBook |

**Esecuzione di query su una proprietà di navigazione**

| Richiesta | URI di esempio | Nome dell'azione | Azione di esempio |
| --- | --- | --- | --- |
| /Entityset GET (tasto) / spostamento | / I prodotti (1) / fornitore | GetNavigationFromEntityType o GetNavigation | GetSupplierFromProduct |
| OTTENERE/cast/spostamento /entityset (chiave) | / /Models.Book/Author prodotti (1) | GetNavigationFromEntityType o GetNavigation | GetAuthorFromBook |

Per altre informazioni, vedere [Working with Entity Relations](odata-v3/working-with-entity-relations.md).

**Creazione ed eliminazione di collegamenti**

| Richiesta | URI di esempio | Nome dell'azione |
| --- | --- | --- |
| POST /entityset (chiave) / $links/spostamento | / I collegamenti (1) / $ prodotti/fornitore | CreateLink |
| PUT /entityset (chiave) / $links/spostamento | / I collegamenti (1) / $ prodotti/fornitore | CreateLink |
| ELIMINAZIONE /entityset (chiave) / $links/spostamento | / I collegamenti (1) / $ prodotti/fornitore | DeleteLink |
| Elimina /entityset(key)/$links/navigation(relatedKey) | /Products/(1)/$Links/Suppliers(1) | DeleteLink |

Per altre informazioni, vedere [Working with Entity Relations](odata-v3/working-with-entity-relations.md).

**Proprietà**

*Richiede l'API Web 2*

| Richiesta | URI di esempio | Nome dell'azione | Azione di esempio |
| --- | --- | --- | --- |
| /Entityset GET (tasto) / proprietà | / I prodotti (1) / nome | GetPropertyFromEntityType o GetProperty | GetNameFromProduct |
| OTTENERE/cast o la proprietà /entityset (chiave) | / /Models.Book/Author prodotti (1) | GetPropertyFromEntityType o GetProperty | GetTitleFromBook |

**Azioni**

| Richiesta | URI di esempio | Nome dell'azione | Azione di esempio |
| --- | --- | --- | --- |
| POST /entityset (chiave) / azione | / I prodotti (1) / velocità | ActionNameOnEntityType o ActionName | RateOnProduct |
| Registra azione/cast//entityset (chiave) | / /Models.Book/CheckOut prodotti (1) | ActionNameOnEntityType o ActionName | CheckOutOnBook |

Per altre informazioni, vedere [le azioni OData](odata-v3/odata-actions.md).

**Firme del metodo**

Ecco alcune regole per le firme del metodo:

- Se il percorso contiene una chiave, l'azione deve avere un parametro denominato *chiave*.
- Se il percorso contiene una chiave in una proprietà di navigazione, l'azione deve avere un parametro denominato *relatedKey*.
- Decorare *key* e *relatedKey* parametri con il **[FromODataUri]** parametro.
- Le richieste PUT e POST accettano un parametro del tipo di entità.
- Le richieste PATCH accettano un parametro di tipo **Delta&lt;T&gt;**, dove *T* è il tipo di entità.

Per riferimento, ecco un esempio che illustra le firme di metodo per ogni convenzione di routing OData predefinite.

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>Convenzioni di Routing personalizzate

Attualmente le convenzioni predefinite non coprono tutti i possibili OData URIs. È possibile aggiungere nuove convenzioni implementando il **IODataRoutingConvention** interfaccia. Questa interfaccia offre due metodi:

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController** restituisce il nome del controller.
- **SelectAction** restituisce il nome dell'azione.

Per entrambi i metodi, se la convenzione non viene applicato a tale richiesta, è possibile che il metodo deve restituire null.

Il **ODataPath** parametro rappresenta il percorso della risorsa OData analizzato. Contiene un elenco delle **[ODataPathSegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** istanze, una per ogni segmento del percorso della risorsa. **Oggetto ODataPathSegment** è una classe astratta; ogni tipo di segmento è rappresentata da una classe che deriva da **ODataPathSegment**.

Il **ODataPath.TemplatePath** proprietà è una stringa che rappresenta la concatenazione di tutti i segmenti di percorso. Ad esempio, se è l'URI `/Products(1)/Supplier`, il modello di percorso viene &quot;~/entityset/key/navigation&quot;. Si noti che i segmenti non corrispondono direttamente ai segmenti dell'URI. Ad esempio, la chiave di entità (1) è rappresentata come propria **ODataPathSegment**.

In genere, un'implementazione di **IODataRoutingConvention** esegue le operazioni seguenti:

1. Confrontare il modello di percorso per verificare se questa convenzione viene applicata la richiesta corrente. Se non è applicabile, restituisce null.
2. Se si applica la convenzione, usare le proprietà del **ODataPathSegment** istanze per derivare i nomi di controller e azione.
3. Per le azioni, aggiungere tutti i valori al dizionario della route che deve essere associato a parametri di azione (in genere chiavi di entità).

Esaminiamo un esempio specifico. Le convenzioni di routing predefinite non supportano l'indicizzazione in una raccolta di navigazione. In altre parole, non sono previste convenzioni per gli URI simile al seguente:

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

Di seguito è una convenzione di routing personalizzata per gestire questo tipo di query.

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

Note:

1. La derivazione da **EntitySetRoutingConvention**, in quanto il **SelectController** è appropriato per questa convenzione di routing nuovo metodo nella classe. Ciò significa che non devo implementare nuovamente **SelectController**.
2. La convenzione viene applicata solo alle richieste GET e solo quando il modello di percorso viene &quot;~/entityset/key/navigation/key&quot;.
3. È il nome dell'azione &quot;ottenere {EntityType}&quot;, dove *{EntityType}* è il tipo della raccolta di navigazione. Ad esempio, &quot;GetSupplier&quot;. È possibile usare qualsiasi convenzione di denominazione che si desidera che &#8212; assicurarsi di azioni del controller corrispondono.
4. L'azione accetta due parametri denominati *key* e *relatedKey*. (Per un elenco di alcuni nomi di parametro predefiniti, vedere [ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx).)

Il passaggio successivo viene aggiunta la nuova convenzione all'elenco di convenzioni di routing. Ciò si verifica durante la configurazione, come illustrato nel codice seguente:

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

Ecco alcuni altri esempio convenzioni di routing che essere utili per analizzare:

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

E naturalmente stessa API Web è open source, pertanto è possibile vedere le [codice sorgente](http://aspnetwebstack.codeplex.com/) per le convenzioni di routing predefinite. Questi vengono definiti nel **System.Web.Http.OData.Routing.Conventions** dello spazio dei nomi.
