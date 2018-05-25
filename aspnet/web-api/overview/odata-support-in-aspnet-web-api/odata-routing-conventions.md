---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: Convenzioni di routing in ASP.NET Web API 2 Odata | Documenti Microsoft
author: MikeWasson
description: Questo articolo descrive le convenzioni di routing che usa API Web per gli endpoint OData.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/31/2013
ms.topic: article
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 0ab99dd443040b90ffefd2f5b9261a63b91e9463
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="routing-conventions-in-aspnet-web-api-2-odata"></a>Convenzioni di routing in ASP.NET Web API 2 Odata
====================
da [Mike Wasson](https://github.com/MikeWasson)

> Questo articolo descrive le convenzioni di routing che usa API Web per gli endpoint OData.


Quando l'API Web riceve una richiesta di OData, viene eseguito il mapping della richiesta per un nome del controller e un nome di azione. Il mapping è basato sul metodo HTTP e l'URI. Ad esempio, `GET /odata/Products(1)` esegue il mapping a `ProductsController.GetProduct`.

Nella parte 1 di questo articolo, descrivo le convenzioni di routing OData predefinite. Queste convenzioni sono appositamente progettati per l'endpoint OData, e sostituire il sistema di routing di API Web predefinito. (La sostituzione avviene quando si chiama **MapODataRoute**.)

Nella parte 2 è illustrato come aggiungere le convenzioni di routine personalizzate. Attualmente le convenzioni predefinite non comprendono OData URIs intero intervallo, ma è possibile estendere in modo da gestire altri casi.

- [Convenzioni di Routing predefinite](#conventions)
- [Convenzioni di Routing personalizzate](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>Convenzioni di Routing predefinite

Prima descrivono le convenzioni di routing OData nell'API Web, è utile comprendere OData URIs. Un [URI OData](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/) è costituito da:

- La radice del servizio
- Il percorso della risorsa
- Opzioni di query

![](odata-routing-conventions/_static/image1.png)

Per il routing, la parte importante è il percorso della risorsa. Il percorso della risorsa è suddivisa in segmenti. Ad esempio, `/Products(1)/Supplier` ha tre segmenti:

- `Products`fa riferimento a un set di entità denominato "Prodotti".
- `1`è una chiave di entità, la selezione di una singola entità dal set.
- `Supplier`è una proprietà di navigazione che consente di selezionare un'entità correlata.

Pertanto, questo percorso preleva il fornitore del prodotto 1.

> [!NOTE]
> Segmenti di percorso OData non sempre corrispondono a segmenti URI. Ad esempio, "1" viene considerato un segmento di percorso.


**Nomi di controller.** Il nome del controller viene sempre derivato dal set alla radice del percorso della risorsa di entità. Ad esempio, se il percorso della risorsa è `/Products(1)/Supplier`, API Web esegue la ricerca di un controller denominato `ProductsController`.

**Nomi di azione.** I nomi di azione sono derivati dai segmenti di percorso più entity data model (EDM), come indicato nelle tabelle seguenti. In alcuni casi, sono disponibili due opzioni per il nome dell'azione. Ad esempio, "Get" o &quot;GetProducts&quot;.

**Eseguire query sulle entità**

| Richiesta | URI di esempio | Nome dell'azione | Azione di esempio |
| --- | --- | --- | --- |
| OTTENERE /entityset | / Prodotti | GetEntitySet o Get | GetProducts |
| OTTENERE /entityset(key) | /Products(1) | GetEntityType o Get | GetProduct |
| GET /entityset(key)/cast | /Products(1)/Models.Book | GetEntityType o Get | GetBook |

Per ulteriori informazioni, vedere [creare un OData Endpoint di sola lettura](odata-v3/creating-an-odata-endpoint.md).

**Creazione, aggiornamento ed eliminazione di entità**

| Richiesta | URI di esempio | Nome dell'azione | Azione di esempio |
| --- | --- | --- | --- |
| REGISTRARE /entityset | / Prodotti | PostEntityType o Post | PostProduct |
| Inserire /entityset(key) | /Products(1) | PutEntityType o Put | PutProduct |
| PUT /entityset (chiave) / eseguire il cast | /Products(1)/Models.Book | PutEntityType o Put | PutBook |
| PATCH /entityset(key) | /Products(1) | PatchEntityType o Patch | PatchProduct |
| PATCH /entityset (chiave) / eseguire il cast | /Products(1)/Models.Book | PatchEntityType o Patch | PatchBook |
| ELIMINARE /entityset(key) | /Products(1) | DeleteEntityType o Delete | DeleteProduct |
| ELIMINARE /entityset (chiave) / eseguire il cast | /Products(1)/Models.Book | DeleteEntityType o Delete | DeleteBook |

**Esecuzione di query su una proprietà di navigazione**

| Richiesta | URI di esempio | Nome dell'azione | Azione di esempio |
| --- | --- | --- | --- |
| GET /entityset (chiave) / spostamento | / Prodotti (1) / fornitore | GetNavigationFromEntityType o GetNavigation | GetSupplierFromProduct |
| OTTENERE cast/spostamento /entityset (chiave) | / /Models.Book/Author prodotti (1) | GetNavigationFromEntityType o GetNavigation | GetAuthorFromBook |

Per ulteriori informazioni, vedere [Working with Entity Relations](odata-v3/working-with-entity-relations.md).

**Creazione ed eliminazione di collegamenti**

| Richiesta | URI di esempio | Nome dell'azione |
| --- | --- | --- |
| POST /entityset (chiave) / $links/spostamento | / Prodotti (1) / $ collegamenti/fornitore | CreateLink |
| PUT /entityset (chiave) / $links/spostamento | / Prodotti (1) / $ collegamenti/fornitore | CreateLink |
| Elimina /entityset (chiave) / $links/spostamento | / Prodotti (1) / $ collegamenti/fornitore | DeleteLink |
| DELETE /entityset(key)/$links/navigation(relatedKey) | /Products/(1)/$links/Suppliers(1) | DeleteLink |

Per ulteriori informazioni, vedere [Working with Entity Relations](odata-v3/working-with-entity-relations.md).

**Proprietà**

*Richiede di Web API 2*

| Richiesta | URI di esempio | Nome dell'azione | Azione di esempio |
| --- | --- | --- | --- |
| GET /entityset (chiave) / proprietà | / Prodotti (1) / nome | GetPropertyFromEntityType o GetProperty | GetNameFromProduct |
| GET /entityset(key)/cast/property | / /Models.Book/Author prodotti (1) | GetPropertyFromEntityType o GetProperty | GetTitleFromBook |

**Azioni**

| Richiesta | URI di esempio | Nome dell'azione | Azione di esempio |
| --- | --- | --- | --- |
| POST /entityset(key)/action | / Prodotti (1) e velocità | ActionNameOnEntityType o ActionName | RateOnProduct |
| POST /entityset(key)/cast/action | /Products(1)/Models.Book/CheckOut | ActionNameOnEntityType o ActionName | CheckOutOnBook |

Per ulteriori informazioni, vedere [le azioni OData](odata-v3/odata-actions.md).

**Firme del metodo**

Ecco alcune regole per le firme del metodo:

- Se il percorso contiene una chiave, l'azione deve avere un parametro denominato *chiave*.
- Se il percorso contiene una chiave in una proprietà di navigazione, l'azione deve avere un parametro denominato *relatedKey*.
- Decorare *chiave* e *relatedKey* parametri con il **[FromODataUri]** parametro.
- Le richieste PUT e POST accettano un parametro del tipo di entità.
- Le richieste PATCH accettano un parametro di tipo **Delta&lt;T&gt;**, dove *T* è il tipo di entità.

Per riferimento, ecco un esempio che illustra le firme di metodo per ogni convenzione di routing OData predefinite.

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>Convenzioni di Routing personalizzate

Attualmente le convenzioni predefinite non comprendono tutti i possibili OData URIs. È possibile aggiungere nuove convenzioni implementando il **IODataRoutingConvention** interfaccia. Questa interfaccia dispone di due metodi:

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController** restituisce il nome del controller.
- **SelectAction** restituisce il nome dell'azione.

Per entrambi i metodi, se non si applica la convenzione a tale richiesta, è possibile che il metodo deve restituire null.

Il **ODataPath** parametro rappresenta il percorso della risorsa OData analizzato. Contiene un elenco di **[ODataPathSegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** istanze, uno per ogni segmento del percorso della risorsa. **ODataPathSegment** è una classe astratta; ogni tipo di segmento è rappresentato da una classe che deriva da **ODataPathSegment**.

Il **ODataPath.TemplatePath** proprietà è una stringa che rappresenta la concatenazione di tutti i segmenti di percorso. Ad esempio, se l'URI è `/Products(1)/Supplier`, il modello di percorso è &quot;~/entityset/key/navigation&quot;. Si noti che i segmenti non corrispondono direttamente a segmenti URI. Ad esempio, la chiave di entità (1) è rappresentata come propria **ODataPathSegment**.

In genere, un'implementazione di **IODataRoutingConvention** esegue le operazioni seguenti:

1. Confrontare il modello di percorso per verificare se questa convenzione viene applicata la richiesta corrente. Se non è applicabile, restituisce null.
2. Se si applica la convenzione, utilizzare le proprietà del **ODataPathSegment** istanze per derivare i nomi di azione e del controller.
3. Per le azioni, aggiungere tutti i valori per il dizionario della route che devono essere associati ai parametri di azione (in genere le chiavi di entità).

Esaminiamo un esempio specifico. Le convenzioni di routing predefinite non supportano l'indicizzazione in una raccolta di navigazione. In altre parole, non sono previste convenzioni per gli URI simile al seguente:

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

Ecco una convenzione di routing personalizzata per gestire questo tipo di query.

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

Note:

1. La derivazione da **EntitySetRoutingConvention**, in quanto il **SelectController** è appropriato per questa convenzione di routing nuovo metodo nella classe. Ciò significa che non è necessario implementare nuovamente **SelectController**.
2. La convenzione si applica solo alle richieste GET, e solo quando il modello di percorso è &quot;~/entityset/key/navigation/key&quot;.
3. Il nome dell'azione è &quot;ottenere {EntityType}&quot;, dove *{EntityType}* è il tipo della raccolta di navigazione. Ad esempio, &quot;GetSupplier&quot;. È possibile utilizzare qualsiasi convenzione di denominazione che si desidera & #8212; Assicurarsi che le azioni del controller corrispondono.
4. L'azione accetta due parametri denominati *chiave* e *relatedKey*. (Per un elenco di alcuni nomi di parametro predefiniti, vedere [ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx).)

Il passaggio successivo consiste nell'aggiunta la convenzione di nuovo all'elenco di convenzioni di routing. Ciò si verifica durante la configurazione, come illustrato nel codice seguente:

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

Ecco alcuni altri esempio convenzioni di routing che essere utile esaminare:

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

E naturalmente stessa interfaccia API Web open source, in modo da visualizzare il [codice sorgente](http://aspnetwebstack.codeplex.com/) per le convenzioni di routing predefinite. Questi sono definiti nel file di **System.Web.Http.OData.Routing.Conventions** dello spazio dei nomi.
