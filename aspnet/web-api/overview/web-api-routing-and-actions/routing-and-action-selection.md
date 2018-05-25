---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Routing e la selezione di azione in ASP.NET Web API | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2012
ms.topic: article
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 997582263bd48590b74434ee0ffc6be928fa1e08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a>Routing e la selezione di azione in ASP.NET Web API
====================
da [Mike Wasson](https://github.com/MikeWasson)

In questo articolo viene descritto come ASP.NET Web API invia una richiesta HTTP a una determinata azione su un controller.

> [!NOTE]
> Per una panoramica del routing, vedere [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).


In questo articolo esamina i dettagli del processo di routing. Se si crea un progetto di Web API e trova che alcune richieste non indirizzate come che previsto, probabilmente consente di questo articolo.

Il routing offre tre fasi principali:

1. Corrispondenza dell'URI per un modello di route.
2. Selezione di un controller.
3. Selezionando un'azione.

È possibile sostituire alcune parti del processo con comportamenti personalizzati. In questo articolo, descrivo il comportamento predefinito. Al termine, nota le posizioni in cui è possibile personalizzare il comportamento.

## <a name="route-templates"></a>Modelli di route

Un modello di route è simile a un percorso URI, ma può avere i valori segnaposto, indicati tra parentesi graffe:

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

Quando si crea una route, è possibile fornire valori predefiniti per alcuni o tutti i segnaposto:

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

È inoltre possibile specificare vincoli, ovvero limitano come un segmento URI può corrispondere a un segnaposto:

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

Il framework tenta di abbinare i segmenti nel percorso dell'URI per il modello. Valori letterali nel modello devono corrispondere esattamente. Un segnaposto corrisponde a qualsiasi valore, a meno che non si specificano vincoli. Il framework non corrisponde a altre parti dell'URI, ad esempio il nome host o i parametri di query. Il framework seleziona la prima route nella tabella di route che corrisponde all'URI specificato.

Esistono due segnaposto speciale: "{controller}" e "{action}".

- "{controller}" fornisce il nome del controller.
- "{action}" fornisce il nome dell'azione. Nell'API Web, la convenzione di solito è omettere "{action}".

### <a name="defaults"></a>attività

Se si specificano valori predefiniti, la route corrisponde un URI a cui mancano i segmenti. Ad esempio:

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

L'URI "`http://localhost/api/products`" corrisponde a questa route. Il segmento '{categoria}' è assegnato il valore predefinito "tutti".

### <a name="route-dictionary"></a>Dizionario della route

Se il framework trova una corrispondenza per un URI, viene creato un dizionario che contiene il valore per ogni segnaposto. Le chiavi sono i nomi dei segnaposto, senza includere le parentesi graffe. I valori vengono ricavati dal percorso URI o dalle impostazioni predefinite. Il dizionario viene archiviato nel **IHttpRouteData** oggetto.

Durante questa fase di corrispondenza di route le speciali "{controller}" segnaposto "{action}" vengano considerati come altri segnaposto. Questi sono semplicemente archiviati nel dizionario con gli altri valori.

Valore predefinito può avere il valore speciale **RouteParameter.Optional**. Se questo valore assegnato un segnaposto, il valore non viene aggiunto per il dizionario della route. Ad esempio:

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

Per il percorso dell'URI "api/prodotti", conterrà il dizionario della route:

- controller: "prodotti"
- category: "all"

Per "api/prodotti/toys/123", tuttavia, il dizionario della route conterrà:

- controller: "prodotti"
- category: "toys"
- id: "123"

Le impostazioni predefinite possono inoltre includere un valore che non viene visualizzata in qualsiasi punto nel modello di route. Se la route corrisponde, tale valore viene archiviato nel dizionario. Ad esempio:

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

Se il percorso dell'URI è "root/api/8", il dizionario contiene due valori:

- controller: "customers"
- id: "8"

## <a name="selecting-a-controller"></a>La selezione di un Controller

La selezione del controller è gestita dal **IHttpControllerSelector.SelectController** metodo. Questo metodo accetta un **HttpRequestMessage** istanza e restituisce un **HttpControllerDescriptor**. L'implementazione predefinita viene eseguito il **DefaultHttpControllerSelector** classe. Questa classe Usa un semplice algoritmo:

1. Esaminare il dizionario della route per la chiave "controller".
2. Accettare il valore per questa chiave e aggiungere la stringa "Controller" per ottenere il nome del tipo di controller.
3. Cercare un controller Web API con questo nome del tipo.

Ad esempio, se il dizionario della route contiene il coppia chiave-valore Microsoft "controller" = "prodotti", il tipo di controller è "ProductsController". Se è presente alcun tipo corrispondente, o più corrispondenze, il framework restituisce un errore al client.

Per il passaggio 3, **DefaultHttpControllerSelector** utilizza il **IHttpControllerTypeResolver** interfaccia da ottenere l'elenco dei tipi di controller API Web. L'implementazione predefinita di **IHttpControllerTypeResolver** restituisce tutte le classi pubbliche che implementano (a) **IHttpController**, (b) sono non astratta e (c) dispone di un nome che termina con "Controller".

## <a name="action-selection"></a>Selezione di azione

Dopo aver selezionato il controller, il framework seleziona l'azione chiamando il **IHttpActionSelector.SelectAction** metodo. Questo metodo accetta un **HttpControllerContext** e restituisce un **HttpActionDescriptor**.

L'implementazione predefinita viene eseguito il **ApiControllerActionSelector** classe. Per selezionare un'azione, esamina le operazioni seguenti:

- Il metodo HTTP della richiesta.
- Il segnaposto "{action}" nel modello di route, se presente.
- I parametri delle operazioni nel controller.

Prima di esaminare l'algoritmo di selezione, è necessario comprendere alcuni aspetti di azioni del controller.

**I metodi sul controller sono considerati "azioni"?** Quando si seleziona un'azione, il framework esamina solo i metodi di istanza pubblici nel controller. Inoltre, sono esclusi ["nome speciale"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) metodi (costruttori, eventi, gli overload degli operatori e così via) e metodi ereditati dal **ApiController** classe.

**Metodi HTTP.** Il framework sceglie solo le azioni che corrispondano al metodo HTTP della richiesta, determinata come segue:

1. È possibile specificare il metodo HTTP con un attributo: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**,  **HttpOptions**, **HttpPatch**, **HttpPost**, o **HttpPut**.
2. In caso contrario, se il nome del metodo controller inizia con "Get", "Post", "Put", "Delete", "Head", "Opzioni" o "Patch", quindi per convenzione l'azione supporta il metodo HTTP.
3. Se nessuna delle precedenti, il metodo supporta POST.

**Associazioni di parametri.** Associazione di un parametro è un valore per un parametro di creazione delle API Web. Di seguito è riportata la regola predefinita per l'associazione di parametri:

- Tipi semplici vengono eseguiti dall'URI.
- Tipi complessi vengono estratti dal corpo della richiesta.

I tipi semplici includono tutti il [tipi primitivi di .NET Framework](https://msdn.microsoft.com/library/system.type.isprimitive), oltre a **DateTime**, **decimale**, **Guid**, **stringa** , e **TimeSpan**. Per ogni azione, al massimo un parametro può leggere il corpo della richiesta.

> [!NOTE]
> È possibile ignorare le regole di associazione predefinito. Vedere [l'associazione di parametri WebAPI dietro le quinte](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).


Con lo sfondo, di seguito è l'algoritmo di selezione dell'azione.

1. Creare un elenco di tutte le azioni sul controller a cui corrisponde il metodo di richiesta HTTP.
2. Se il dizionario della route è una voce "action", rimuovere le azioni il cui nome corrisponde a questo valore.
3. Provare a corrispondere ai parametri azione per l'URI, come indicato di seguito: 

    1. Per ogni azione, è possibile ottenere un elenco di parametri che sono un tipo semplice, in cui l'associazione ottiene il parametro dall'URI. Escludere i parametri facoltativi.
    2. Da questo elenco, provare a trovare una corrispondenza per ogni nome di parametro, il dizionario della route o nella stringa di query dell'URI. Corrispondenze vengono fatta distinzione tra maiuscole e minuscole e non dipendono dall'ordine dei parametri.
    3. Selezionare un'azione in cui ogni parametro nell'elenco ha una corrispondenza nell'URI.
    4. Se più di un'azione soddisfa questi criteri, selezionare quella con la maggior parte delle corrispondenze di parametro.
4. Ignorare le azioni con il **[NonAction]** attributo.

Passaggio 3 # è probabilmente il più intuitive. L'idea di base è che un parametro è possibile ottenere il valore dell'URI, dal corpo della richiesta o da un'associazione personalizzata. Per i parametri forniti dall'URI, si desidera assicurarsi che l'URI contiene effettivamente un valore per il parametro, nel percorso (tramite il dizionario della route) o nella stringa di query.

Ad esempio, si consideri l'azione seguente:

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

Il *id* il parametro viene associato all'URI. Pertanto, questa azione può corrispondere solo un URI che contiene un valore per "id", il dizionario della route o nella stringa di query.

Parametri facoltativi sono un'eccezione, perché sono facoltativi. Per un parametro facoltativo, è OK se l'associazione non è possibile ottenere il valore dell'URI.

I tipi complessi sono un'eccezione per un motivo diverso. Un tipo complesso può associarsi solo a URI tramite un'associazione personalizzata. Ma in tal caso, cui il framework non è possibile sapere in anticipo se il parametro sarebbe associato a un particolare URI. Per individuare, sarebbe necessario richiamare l'associazione. L'obiettivo dell'algoritmo consiste nel selezionare un'azione in base alla descrizione statica, prima di richiamare le associazioni. Di conseguenza, i tipi complessi sono escluse dall'algoritmo di corrispondenza.

Dopo aver selezionato l'azione, tutte le associazioni di parametro vengono richiamate.

Riepilogo:

- L'azione deve corrispondere il metodo HTTP della richiesta.
- Il nome dell'azione deve corrispondere alla voce di "azione" nel dizionario di route, se presente.
- Per ogni parametro dell'azione, se il parametro non viene eseguito dall'URI, quindi il nome del parametro deve essere trovato nel dizionario della route o nella stringa di query dell'URI. (Parametri facoltativi e parametri con tipi complessi sono esclusi.)
- Provare a cui corrisponde il maggior numero di parametri. La corrispondenza migliore potrebbe essere un metodo senza parametri.

## <a name="extended-example"></a>Esempio esteso

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

Il dizionario della route non contiene parametri di stringa di query "version" e "Dettagli", ma questi verranno ancora considerati durante la selezione di azione.

### <a name="controller-selection"></a>Selezione del controller

Voce "controller" nel dizionario della route, è il tipo di controller `ProductsController`.

### <a name="action-selection"></a>Selezione di azione

La richiesta HTTP è una richiesta GET. Le azioni del controller che supportano il recupero sono `GetAll`, `GetById`, e `FindProductsByName`. Il dizionario della route non contiene una voce per "action", è necessario associare il nome dell'azione.

Successivamente, si tenta di corrispondere i nomi dei parametri per le azioni, esaminare solo le operazioni GET.

| Operazione | Parametri di corrispondenza |
| --- | --- |
| `GetAll` | none |
| `GetById` | "id" |
| `FindProductsByName` | "nome" |

Si noti che il *versione* parametro di `GetById` non viene considerato, perché è un parametro facoltativo.

Il `GetAll` metodo corrisponde in modo semplice. Il `GetById` metodo inoltre corrisponde, perché il dizionario della route contiene "id". Il `FindProductsByName` (metodo) non corrisponde.

Il `GetById` metodo wins, poiché corrisponde a un parametro, e nessun parametro per `GetAll`. Il metodo viene richiamato con i valori dei parametri seguenti:

- *id* = 1
- *versione* = 1.5

Si noti che anche se *versione* non è stato utilizzato nell'algoritmo di selezione, il valore del parametro proviene dalla stringa di query URI.

## <a name="extension-points"></a>Punti di estensione

API Web fornisce punti di estensione per alcune parti del processo di routing.

| Interfaccia | Descrizione |
| --- | --- |
| **IHttpControllerSelector** | Seleziona il controller. |
| **IHttpControllerTypeResolver** | Ottiene l'elenco dei tipi di controller. Il **DefaultHttpControllerSelector** sceglie il tipo di controller da questo elenco. |
| **IAssembliesResolver** | Ottiene l'elenco degli assembly di progetto. Il **IHttpControllerTypeResolver** interfaccia utilizza questo elenco per trovare i tipi di controller. |
| **IHttpControllerActivator** | Crea nuove istanze di controller. |
| **IHttpActionSelector** | Seleziona l'azione. |
| **IHttpActionInvoker** | Richiama l'azione. |

Per garantire la propria implementazione di una di queste interfacce, utilizzare il **servizi** insieme il **HttpConfiguration** oggetto:

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
