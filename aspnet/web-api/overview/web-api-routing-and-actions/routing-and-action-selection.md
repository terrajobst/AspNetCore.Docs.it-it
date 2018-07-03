---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Routing e selezione dell'azione nell'API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2012
ms.topic: article
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: dc0edbdf62ceab1baf441301b47c0293de9a5c5d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388614"
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a>Routing e selezione dell'azione nell'API Web ASP.NET
====================
da [Mike Wasson](https://github.com/MikeWasson)

Questo articolo descrive come API Web ASP.NET consente di indirizzare una richiesta HTTP per una determinata azione su un controller.

> [!NOTE]
> Per una panoramica generale del routing, vedere [Routing in API Web ASP.NET](routing-in-aspnet-web-api.md).


Questo articolo esamina i dettagli del processo di routing. Se si crea un progetto API Web e si scopre che alcune richieste Don ' t get indirizzato nel modo che previsto, si spera in questo articolo spiega come.

Routing prevede tre fasi principali:

1. Corrispondenza dell'URI per un modello di route.
2. Quando si seleziona un controller.
3. Selezionare un'azione.

È possibile sostituire alcune parti del processo con i propri comportamenti personalizzati. In questo articolo, descritto il comportamento predefinito. Al termine, nota le posizioni in cui è possibile personalizzare il comportamento.

## <a name="route-templates"></a>Modelli di route

Un modello di route è simile a un percorso URI, ma può avere i valori segnaposto, indicati tra parentesi graffe:

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

Quando si crea una route, è possibile fornire valori predefiniti per alcuni o tutti i segnaposto:

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

È anche possibile fornire i vincoli, che limitano come un segmento dell'URI può corrispondere a un segnaposto:

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

Il framework tenta di far corrispondere i segmenti nel percorso dell'URI per il modello. Valori letterali nel modello devono corrispondere esattamente. Un segnaposto corrisponde a qualsiasi valore, a meno che non si specificano vincoli. Il framework non corrisponde a altre parti dell'URI, ad esempio il nome host o i parametri di query. Il framework sceglie la prima route nella tabella di route che corrisponde all'URI specificato.

Esistono due segnaposto speciali: "{controller}" e "{action}".

- "{controller}" fornisce il nome del controller.
- "{action}" fornisce il nome dell'azione. Nell'API Web, la convenzione consueto consiste nell'omettere "{action}".

### <a name="defaults"></a>attività

Se si specificano le impostazioni predefinite, route corrisponderà a un URI privo di tali segmenti. Ad esempio:

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

L'URI "`http://localhost/api/products`" corrisponde a questa route. Il segmento "{category}" è assegnato il valore predefinito "all".

### <a name="route-dictionary"></a>Dizionario della route

Se il framework rileva una corrispondenza per un URI, crea un dizionario che contiene il valore di ogni segnaposto. Le chiavi sono i nomi dei segnaposto, senza includere le parentesi graffe. I valori vengono ricavati dal percorso URI o i valori predefiniti. Il dizionario viene archiviato nel **IHttpRouteData** oggetto.

Durante questa fase di corrispondenza dei route, la speciale "{controller}" e il segnaposto "{action}" vengono gestiti come i segnaposto di altri. Sono semplicemente archiviati nel dizionario gli altri valori.

Un valore predefinito può avere il valore speciale **RouteParameter.Optional**. Se questo valore assegnato un segnaposto, il valore non viene aggiunto al dizionario della route. Ad esempio:

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

Per il percorso dell'URI "api/prodotti", conterrà il dizionario della route:

- controller: "prodotti"
- category: "all"

Per "api/prodotti/toys/123", tuttavia, il dizionario della route conterrà:

- controller: "prodotti"
- category: "toys"
- id: "123"

I valori predefiniti possono anche includere un valore che non viene visualizzato in un punto qualsiasi nel modello di route. Se la route corrisponde, tale valore viene archiviato nel dizionario. Ad esempio:

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

Se il percorso dell'URI è "root/api/8", il dizionario contiene due valori:

- controller: "customers"
- id: "8"

## <a name="selecting-a-controller"></a>Quando si seleziona un Controller

La selezione del controller è gestita dal **IHttpControllerSelector.SelectController** (metodo). Questo metodo accetta un **HttpRequestMessage** istanza e restituisce un' **HttpControllerDescriptor**. L'implementazione predefinita avviene tramite il **DefaultHttpControllerSelector** classe. Questa classe Usa un algoritmo molto semplice:

1. Cercare nel dizionario della route per la chiave "controller".
2. Il valore per questa chiave e aggiungere la stringa "Controller" per ottenere il nome del tipo di controller.
3. Cercare un controller API Web con questo nome del tipo.

Ad esempio, se il dizionario della route contiene il coppia chiave-valore "controller" = "prodotti", il tipo di controller è "ProductsController". Se è presente alcun tipo di corrispondenza o più corrispondenze, il framework restituisce un errore al client.

Passaggio 3, **DefaultHttpControllerSelector** Usa le **IHttpControllerTypeResolver** interfaccia da ottenere l'elenco dei tipi di controller API Web. L'implementazione predefinita di **IHttpControllerTypeResolver** restituisce tutte le classi pubbliche che implementano (a) **IHttpController**, (b) sono non astratta e (c) dispone di un nome che termina con "Controller".

## <a name="action-selection"></a>Selezione azione

Dopo aver selezionato il controller, il framework consente di selezionare l'azione chiamando il **IHttpActionSelector.SelectAction** (metodo). Questo metodo accetta un **HttpControllerContext** e restituisce un' **HttpActionDescriptor**.

L'implementazione predefinita avviene tramite il **ApiControllerActionSelector** classe. Per selezionare un'azione, analizza i seguenti:

- Il metodo HTTP della richiesta.
- Il segnaposto "{action}" nel modello di route, se presente.
- I parametri delle operazioni nel controller.

Prima di esaminare l'algoritmo di selezione, è necessario comprendere alcuni aspetti di azioni del controller.

**I metodi nel controller che sono considerati "azioni"?** Quando si seleziona un'azione, il framework esamina solo i metodi di istanza pubblici nel controller. Inoltre, esclude ["nome speciale"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) metodi (costruttori, eventi, gli overload degli operatori e così via) e metodi ereditati dalle **ApiController** classe.

**Metodi HTTP.** Il framework sceglie solo le azioni che corrispondono al metodo HTTP della richiesta, determinata come segue:

1. È possibile specificare il metodo HTTP con un attributo: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**,  **HttpOptions**, **HttpPatch**, **HttpPost**, oppure **HttpPut**.
2. In caso contrario, se il nome del metodo del controller inizia con "Get", "Post", "Put", "Delete", "Head", "Opzioni" o "Patch", quindi per convenzione l'azione supporta tale metodo HTTP.
3. Se nessuna delle precedenti, il metodo supporta POST.

**Associazioni di parametro.** Un'associazione di parametri è come API Web crea un valore per un parametro. Ecco la regola predefinita per l'associazione di parametro:

- Tipi semplici vengono forniti dall'URI.
- I tipi complessi sono tratti dal corpo della richiesta.

Tipi semplici includono tutte le [i tipi primitivi di .NET Framework](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **Guid**, **stringa** , e **TimeSpan**. Per ogni azione, al massimo un parametro può leggere il corpo della richiesta.

> [!NOTE]
> È possibile sostituire le regole di associazione predefinita. Visualizzare [associazione di parametri di API Web dietro le quinte](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).


Detto questo, ecco l'algoritmo di selezione dell'azione.

1. Creare un elenco di tutte le azioni nel controller che corrisponde al metodo di richiesta HTTP.
2. Se il dizionario della route è una voce "action", rimuovere le azioni il cui nome corrisponde a questo valore.
3. Provare a corrispondere ai parametri di azione all'URI, come indicato di seguito: 

    1. Per ogni azione, ottenere un elenco dei parametri sono un tipo semplice, in cui l'associazione ottiene il parametro dall'URI. Escludere i parametri facoltativi.
    2. In questo elenco, provare a trovare una corrispondenza per ogni nome di parametro, il dizionario della route o nella stringa di query dell'URI. Corrispondenze sono maiuscole e minuscole e non dipendono l'ordine dei parametri.
    3. Selezionare un'azione in cui ogni parametro nell'elenco ha una corrispondenza nell'URI.
    4. Se più di una sola azione soddisfa questi criteri, scegliere quella con la maggior parte delle corrispondenze di parametro.
4. Ignorare le azioni con il **[NonAction]** attributo.

Passaggio #3 è probabilmente il più confusione. L'idea di base è che un parametro può ottenere il relativo valore dall'URI, dal corpo della richiesta o da un'associazione personalizzata. Per i parametri forniti dall'URI, è opportuno assicurarsi che l'URI contiene effettivamente un valore per tale parametro, il percorso (tramite il dizionario della route) o nella stringa di query.

Ad esempio, si consideri l'azione seguente:

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

Il *id* il parametro viene associato all'URI. Pertanto, questa azione può corrispondere solo a un URI che contiene un valore per "id", il dizionario della route o nella stringa di query.

I parametri facoltativi sono un'eccezione, perché sono facoltativi. Per un parametro facoltativo, è OK se l'associazione non è possibile ottenere il valore dall'URI.

I tipi complessi sono un'eccezione per un motivo diverso. Un tipo complesso può associarsi solo a URI tramite un'associazione personalizzata. Ma in tal caso, il framework non è possibile tener presente se il parametro sarebbe associato a un particolare URI. Per individuare, sarebbe necessario richiamare l'associazione. L'obiettivo dell'algoritmo consiste nel selezionare un'azione dalla descrizione statica, prima di richiamare tutti i binding. Di conseguenza, i tipi complessi vengono esclusi dall'algoritmo di corrispondenza.

Dopo aver selezionato l'azione, tutte le associazioni di parametro vengono richiamate.

Riepilogo:

- L'azione deve corrispondere al metodo HTTP della richiesta.
- Il nome dell'azione deve corrispondere alla voce "action" nel dizionario della route, se presente.
- Per ogni parametro dell'azione, se il parametro viene estratto dall'URI, quindi il nome del parametro deve essere trovato nel dizionario della route o nella stringa di query dell'URI. (I parametri facoltativi e parametri con tipi complessi vengono esclusi.)
- Tentativo di rendere il maggior numero di parametri. La corrispondenza migliore potrebbe essere un metodo senza parametri.

## <a name="extended-example"></a>Esempio dettagliato

Route:

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

Controller:

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

Richiesta HTTP:

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a>Corrispondenza di route

L'URI corrisponde alla route denominata "DefaultApi". Il dizionario della route contiene le voci seguenti:

- controller: "prodotti"
- id: "1"

Il dizionario della route non contiene i parametri della stringa di query, "version" e "Dettagli", ma questi continueranno ad essere considerati durante la selezione di azione.

### <a name="controller-selection"></a>Selezione del controller

Dalla voce "controller" nel dizionario della route, il tipo di controller è `ProductsController`.

### <a name="action-selection"></a>Selezione azione

La richiesta HTTP è una richiesta GET. Le azioni del controller che supportano il recupero vengono `GetAll`, `GetById`, e `FindProductsByName`. Il dizionario della route non contiene una voce per "action", pertanto non è necessario affinché corrisponda al nome di azione.

Quindi, cerchiamo corrispondano ai nomi di parametro per le azioni, osservando solo le azioni GET.

| Operazione | Parametri di corrispondenza |
| --- | --- |
| `GetAll` | none |
| `GetById` | "id" |
| `FindProductsByName` | "name" |

Si noti che il *versione* parametro di `GetById` non viene considerato, perché è un parametro facoltativo.

Il `GetAll` metodo corrisponde alla facilmente. Il `GetById` metodo anche corrispondenze, perché il dizionario della route contiene "id". Il `FindProductsByName` (metodo) non corrisponde.

Il `GetById` metodo wins, poiché corrisponde a un parametro, rispetto a alcun parametro per `GetAll`. Il metodo viene richiamato con i valori dei parametri seguenti:

- *id* = 1
- *versione* Version=1.5

Si noti che sebbene *versione* non è stata usata nell'algoritmo di selezione, il valore del parametro proviene dalla stringa di query URI.

## <a name="extension-points"></a>Punti di estensione

API Web fornisce punti di estensione per alcune parti del processo di routing.

| Interfaccia | Descrizione |
| --- | --- |
| **IHttpControllerSelector** | Seleziona il controller. |
| **IHttpControllerTypeResolver** | Ottiene l'elenco dei tipi di controller. Il **DefaultHttpControllerSelector** sceglie il tipo di controller da questo elenco. |
| **IAssembliesResolver** | Ottiene l'elenco degli assembly di progetto. Il **IHttpControllerTypeResolver** interfaccia Usa questo elenco per trovare i tipi di controller. |
| **IHttpControllerActivator** | Crea nuove istanze di controller. |
| **IHttpActionSelector** | Seleziona l'azione. |
| **IHttpActionInvoker** | Richiama l'azione. |

Per garantire la propria implementazione di una di queste interfacce, usare il **Services** insieme nel **HttpConfiguration** oggetto:

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
