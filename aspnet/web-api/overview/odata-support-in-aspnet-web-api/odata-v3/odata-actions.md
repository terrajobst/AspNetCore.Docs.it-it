---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Supportare le azioni OData nell'API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: 'In OData le azioni sono un modo per aggiungere comportamenti lato server che non sono definiti facilmente come operazioni CRUD su entità. Alcuni usi di azioni includono: implementare...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: b7a968082587120c2a19be86524f9b2eba80856e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370455"
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a>Supportare le azioni OData nell'API Web ASP.NET 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Download progetto completato](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> In OData *azioni* rappresentano un modo per aggiungere comportamenti lato server che non sono definiti facilmente come operazioni CRUD su entità. Alcuni usi della azioni includono:
> 
> - Implementazione di transazioni complesse.
> - La modifica più entità in una sola volta.
> - Consentire gli aggiornamenti solo a determinate proprietà di un'entità.
> - L'invio di informazioni al server che non è definito in un'entità.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - API Web 2
> - OData versione 3
> - Entity Framework 6


## <a name="example-rating-a-product"></a>Esempio: Valutazione di un prodotto

In questo esempio si vuole consentire agli utenti di classificare i prodotti ed esporre le valutazioni medie per ogni prodotto. Il database, Microsoft archivia un elenco di classificazioni, con chiave fornita per i prodotti.

Ecco il modello è possibile che venga utilizzata per rappresentare le classificazioni in Entity Framework:

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

Ma non vogliamo che i client a POST un `ProductRating` oggetto a una raccolta di "Restrizioni". Intuitivamente, la classificazione è associata all'insieme di prodotti e il client è necessario solo registrare il valore di classificazione.

Invece di usare le normali operazioni CRUD, definiamo pertanto un'azione che un client può essere richiamato su un prodotto. Nella terminologia di OData, è l'azione *associata* alle entità Product.

>Le azioni hanno effetti collaterali sul server. Per questo motivo, vengono richiamati usando richieste HTTP POST. Le azioni possono includere i parametri e tipi restituiti, che sono descritti nei metadati del servizio. Il client invia i parametri nel corpo della richiesta e il server invia il valore restituito nel corpo della risposta. Per richiamare l'azione "Frequenza Product", il client invia una richiesta POST a un URI simile al seguente:

[!code-console[Main](odata-actions/samples/sample2.cmd)]

I dati nella richiesta POST sono semplicemente la valutazione del prodotto:

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>Dichiarare l'azione in Entity Data Model

Nella configurazione dell'API Web, aggiungere l'azione di entity data model (EDM):

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

Questo codice definisce "RateProduct" come un'azione che può essere eseguita su entità Product. Dichiara inoltre che l'azione richiede un **int** parametro denominato "Rating" e restituisce un' **int** valore.

## <a name="add-the-action-to-the-controller"></a>Aggiungere l'azione al Controller

L'azione "RateProduct" è associato a entità Product. Per implementare l'azione, aggiungere un metodo denominato `RateProduct` al controller di prodotti:

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

Si noti che il nome del metodo corrisponde al nome dell'azione nel modello EDM. Il metodo ha due parametri:

- *chiave*: la chiave per il prodotto alla velocità.
- *parametri*: un dizionario di valori di parametro di azione.

Se si usa le convenzioni di routing predefinita, il parametro della chiave deve essere denominato "chiavi". È anche importante includere la **[FromOdataUri]** dell'attributo, come illustrato. Questo attributo indica a Web API da usare le regole di sintassi di OData quando analizza la chiave dall'URI della richiesta.

Usare la *parametri* dizionario utilizzato per recuperare i parametri di azione:

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

Se il client invia parametri di azione il corretto di formato, il valore di **ModelState** è true. In tal caso, è possibile usare la **ODataActionParameters** dizionario utilizzato per ottenere i valori dei parametri. In questo esempio, il `RateProduct` azione accetta un singolo parametro denominato "Rating".

## <a name="action-metadata"></a>Metadati di azione

Per visualizzare i metadati del servizio, inviare una richiesta GET a /odata/$ metadata. Questa è la parte dei metadati che dichiara il `RateProduct` azione:

[!code-xml[Main](odata-actions/samples/sample7.xml)]

Il **FunctionImport** elemento dichiara l'azione. La maggior parte dei campi sono di chiara interpretazione, ma sono la pena notare due:

- **È IsBindable** significa che l'azione può essere richiamato almeno su entità di destinazione, alcuni dei casi.
- **IsAlwaysBindable** significa che l'azione può sempre essere richiamato su entità di destinazione.

La differenza è che alcune azioni sono sempre disponibili ai client, ma altre azioni potrebbero dipendere dallo stato dell'entità. Ad esempio, si supponga di definita un'azione "Acquisto". È possibile acquistare solo un elemento che è in magazzino. Se l'elemento è esaurito a distanza, un client non è possibile richiamare tale azione.

Quando si definisce il modello EDM, il **azione** metodo crea un'azione sempre associabile:

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

Verranno descritte le azioni non sempre-associabile (chiamato anche *temporanei* azioni) più avanti in questo argomento.

## <a name="invoking-the-action"></a>Chiamata dell'azione

Ora vediamo come un client potrebbe richiamare questa azione. Si supponga che il client vuole assegnare una classificazione compresa tra 2 al prodotto con ID = 4. Ecco un messaggio di richiesta di esempio, usando il formato JSON per il corpo della richiesta:

[!code-console[Main](odata-actions/samples/sample9.cmd)]

Ecco il messaggio di risposta:

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>Associazione di un'azione a un Set di entità

Nell'esempio precedente, l'azione è associata a una singola entità: il client classifica un singolo prodotto. È anche possibile associare un'azione per una raccolta di entità. Solo apportare le modifiche seguenti:

In EDM, aggiungere l'azione per l'entità **raccolta** proprietà.

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

Nel metodo del controller, omettere il *chiave* parametro.

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

A questo punto il client richiama l'azione sul set di entità Products:

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>Azioni con i parametri della raccolta

Le azioni possono includere i parametri che accettano una raccolta di valori. In EDM, utilizzare **CollectionParameter&lt;T&gt;**  per dichiarare il parametro.

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

Questo codice dichiara un parametro denominato "Simboli valutazione" che accetta una raccolta di **int** valori. Nel metodo del controller, è comunque ottenere il valore del parametro dal **ODataActionParameters** oggetti, ma ora il valore è un **ICollection&lt;int&gt;**  valore:

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>Azioni temporanee

Nell'esempio "RateProduct", gli utenti possono sempre frequenza un prodotto, in modo che l'azione è sempre disponibile. Tuttavia, alcune azioni dipendono dallo stato dell'entità. Ad esempio, in un servizio di noleggio di video, l'azione "Checkpoint" non è sempre disponibile. (Seconda se una copia di tale video è disponibile.) Questo tipo di azione viene chiamato una *temporanei* azione.

Nei metadati del servizio, è un'azione temporanea **IsAlwaysBindable** uguale a false. Che è effettivamente il valore predefinito, in modo che i metadati avrà un aspetto simile al seguente:

[!code-xml[Main](odata-actions/samples/sample16.xml)]

Ecco perché questo è importante: se un'azione è temporanea, il server deve indicare al client quando l'azione è disponibile. Ciò avviene inserendo un collegamento all'azione nell'entità. Di seguito è riportato un esempio per un'entità film:

[!code-console[Main](odata-actions/samples/sample17.cmd)]

La proprietà "#CheckOut" contiene un collegamento all'azione di estrazione. Se l'azione non è disponibile, il server viene omesso il collegamento.

Per dichiarare un'azione temporanea in EDM, chiamare il **TransientAction** metodo:

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

Inoltre, è necessario fornire una funzione che restituisce un collegamento all'azione per una determinata entità. Impostare questa funzione chiamando **HasActionLink**. È possibile scrivere la funzione come un'espressione lambda:

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

Se l'azione è disponibile, l'espressione lambda restituisce un collegamento all'azione. Il serializzatore OData include questo collegamento durante la serializzazione di entità. Quando l'azione non è disponibile, la funzione restituisce `null`.

## <a name="additional-resources"></a>Risorse aggiuntive

[Esempio di azioni OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
