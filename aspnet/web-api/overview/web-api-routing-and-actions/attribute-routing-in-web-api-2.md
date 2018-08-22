---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Attributo di Routing in API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: f13720c5e9de99fb4ae5b27a757c257cac881f89
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826780"
---
<a name="attribute-routing-in-aspnet-web-api-2"></a>Routing mediante attributi n API Web ASP.NET 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

*Routing* è come API Web corrisponde a un URI per un'azione. API Web 2 supporta un nuovo tipo di routing, chiamato *routing con attributi*. Come suggerisce il nome, il routing con attributi Usa gli attributi per definire le route. Routing con attributi offre maggiore controllo sugli URI nell'API web. Ad esempio, è possibile creare con facilità gli URI che descrivono le gerarchie di risorse.

Lo stile di versioni precedenti del routing, denominato basato sulle convenzioni di routing, è ancora completamente supportato. In effetti, è possibile combinare entrambe le tecniche nello stesso progetto.

Questo argomento viene illustrato come abilitare il routing con attributi e descrive le varie opzioni per il routing con attributi. Per un'esercitazione end-to-end che usa il routing con attributi, vedere [creare un'API REST con il Routing con attributi nell'API Web 2](create-a-rest-api-with-attribute-routing.md).


## <a name="prerequisites"></a>Prerequisiti

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional o Enterprise Edition

In alternativa, usare Gestione pacchetti NuGet per installare i pacchetti necessari. Dal **degli strumenti** dal menu di Visual Studio, selezionare **Library Package Manager**, quindi selezionare **Package Manager Console**. Immettere il comando seguente nella finestra della Console di gestione pacchetti:

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>Il motivo per cui Routing con attributi?

La prima versione dell'API Web usata *basato su convenzioni* routing. In tale tipo di routing, si definisce uno o più modelli di route, si tratta fondamentalmente di stringhe con parametri. Quando il framework riceve la richiesta, l'URI in base al modello di route corrisponde a. (Per altre informazioni sul routing basato su convenzioni, vedere [Routing in API Web ASP.NET](routing-in-aspnet-web-api.md).

Uno dei vantaggi di routing basato su convenzione è che i modelli vengono definiti in un'unica posizione e le regole di routing vengono applicate in modo coerente tra tutti i controller. Sfortunatamente, il routing basato su convenzione rende difficile supportare determinati modelli di URI che sono comuni per le API RESTful. Ad esempio, le risorse spesso contengono le risorse figlio: sono presenti ordini, film sono gli attori, sono gli autori di libri e così via. È naturale per creare gli URI che riflettono le relazioni:

`/customers/1/orders`

Questo tipo di URI è difficile creare usando il routing basato su convenzione. Anche se può essere eseguita, i risultati non vengono scalati bene se si dispone di numerosi controller o tipi di risorse.

Con il routing con attributi, è semplice per definire una route per questo URI. Sufficiente aggiungere un attributo per l'azione del controller:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

Ecco alcuni altri modelli di tale attributo routing rende facile.

**Controllo delle versioni API**

In questo esempio, "/ prodotti/api/v1" sarà indirizzato a un controller diverso rispetto a "/ prodotti/api/v2".

`/api/v1/products`  
`/api/v2/products`

**Segmenti URI overload**

In questo esempio, "1" è un numero di ordine, ma "pending" viene eseguito il mapping a una raccolta.

`/orders/1`  
`/orders/pending`

**Più tipi di parametro**

In questo esempio, "1" è un numero di ordine, ma "2013/06/16" specifica una data.

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>Abilitare il Routing con attributi

Per abilitare il routing con attributi, chiamare **MapHttpAttributeRoutes** durante la configurazione. Questo metodo di estensione definito nel **System.Web.Http.HttpConfigurationExtensions** classe.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

Routing con attributi può essere combinato con [basato su convenzioni](routing-in-aspnet-web-api.md) routing. Per definire le route basata sulle convenzioni, chiamare il **MapHttpRoute** (metodo).

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Per altre informazioni sulla configurazione Web API, vedere [configurazione di ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>Nota: La migrazione da API Web 1

Prima di Web API 2, i modelli di progetto API Web generato codice simile al seguente:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

Se il routing con attributi è abilitato, questo codice genererà un'eccezione. Se si aggiorna un progetto API Web esistente per usare il routing con attributi, assicurarsi di aggiornare il codice di configurazione al seguente:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> Per altre informazioni, vedere [configurazione di API Web con Hosting ASP.NET](../advanced/configuring-aspnet-web-api.md#webhost).


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>Aggiunta di attributi di Route

Di seguito è riportato un esempio di una route definita utilizzando un attributo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

La stringa &quot;clienti / {customerId} /Orders&quot; è il modello URI per la route. API Web tenta di far corrispondere l'URI della richiesta per il modello. In questo esempio, "customers" e "orders" sono valori letterali segmenti e "{customerId}" è un parametro della variabile. Corrisponde a questo modello gli URI seguenti:

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

È possibile limitare la corrispondenza usando [vincoli](#constraints), come descritto più avanti in questo argomento.

Si noti che il &quot;{customerId}&quot; parametro nel modello di route corrisponde al nome delle *customerId* parametro del metodo. Quando l'API Web richiama l'azione del controller, tenta di associare i parametri di route. Ad esempio, se è l'URI `http://example.com/customers/1/orders`, API Web tenta di associare il valore "1" per il *customerId* parametro dell'azione.

Un modello URI può includere alcuni parametri:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

Eventuali metodi del controller che non è un attributo di route usano il routing basato su convenzione. In questo modo, è possibile combinare entrambi i tipi di routing nello stesso progetto.

## <a name="http-methods"></a>Metodi HTTP

API Web è inoltre possibile selezionare azioni sulla base del metodo HTTP della richiesta (GET, POST e così via). Per impostazione predefinita, API Web Cerca una corrispondenza tra maiuscole e minuscole con l'inizio del nome del metodo controller. Ad esempio, un metodo del controller denominato `PutCustomers` corrisponde a una richiesta HTTP PUT.

È possibile sostituire questa convenzione assegnando il metodo con gli attributi seguenti:

- **[HttpDelete]**
- **[HttpGet]**
- **[HttpHead]**
- **[HttpOptions]**
- **[HttpPatch]**
- **[HttpPost]**
- **[HttpPut]**

Nell'esempio seguente viene eseguito il mapping del metodo CreateBook alle richieste HTTP POST.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

Per tutti gli altri metodi HTTP, inclusi i metodi non standard, usano il **AcceptVerbs** attributo, che accetta un elenco di metodi HTTP.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>Prefissi di route

Spesso, le route in un controller iniziano tutte con lo stesso prefisso. Ad esempio:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

È possibile impostare un prefisso comune per un intero controller usando il **[RoutePrefix]** attributo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

Utilizzare una tilde (~) dell'attributo del metodo per sostituire il prefisso della route:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

Il prefisso della route può includere parametri:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>Vincoli di route

I vincoli di route consentono di limitare la modalità di corrispondenza dei parametri nel modello di route. La sintassi generale è &quot;{: vincolo del parametro}&quot;. Ad esempio:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

In questo caso, la prima route sarà essere selezionata solo se il &quot;id&quot; segmento dell'URI è un numero intero. In caso contrario, verrà scelta la seconda route.

Nella tabella seguente sono elencati i vincoli che sono supportati.

| Vincolo | Descrizione | Esempio |
| --- | --- | --- |
| alfa | Corrispondenze maiuscolo o minuscolo i caratteri dell'alfabeto latino (a-z, A-Z) | {x:alpha} |
| bool | Corrisponde a un valore booleano. | {x:bool} |
| datetime | Corrisponde a un **DateTime** valore. | {x:datetime} |
| decimal | Corrisponde a un valore decimale. | {x:decimal} |
| double | Corrisponde a un valore a virgola mobile a 64 bit. | {x:double} |
| float | Corrisponde a un valore a virgola mobile a 32 bit. | {float: x} |
| guid | Corrisponde a un valore GUID. | {x: guid} |
| int | Corrisponde a un valore integer a 32 bit. | {x:int} |
| length | Corrisponde a una stringa con la lunghezza specificata o in un determinato intervallo di lunghezza. | {x: length(6)} {x: length(1,20)} |
| long | Corrisponde a un valore integer a 64 bit. | {x:long} |
| max | Corrisponde a un intero con un valore massimo. | {x:max(10)} |
| MaxLength | Corrisponde a una stringa con lunghezza massima. | {x: maxlength(10)} |
| min | Corrisponde a un intero con un valore minimo. | {x:min(10)} |
| minLength | Corrisponde a una stringa con una lunghezza minima. | {x: minlength(10)} |
| range | Consente di ricercare un numero intero compreso in un intervallo di valori. | {x:range(10,50)} |
| regex | Corrisponde a un'espressione regolare. | {x: regex(^\d{3}-\d{3}-\d{4}$)} |

Si noti che alcuni dei vincoli, ad esempio &quot;min&quot;, accettano argomenti racchiuso tra parentesi. È possibile applicare più vincoli a un parametro, separato da due punti.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>Vincoli di Route personalizzati

È possibile creare i vincoli di route personalizzate implementando il **IHttpRouteConstraint** interfaccia. Ad esempio, il vincolo seguente limita un parametro a un valore intero diverso da zero.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

Il codice seguente viene illustrato come registrare il vincolo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

A questo punto è possibile applicare il vincolo di route:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

È inoltre possibile sostituire l'intera **DefaultInlineConstraintResolver** classe implementando il **IInlineConstraintResolver** interfaccia. In questo modo sostituirà tutti i vincoli predefiniti, a meno che l'implementazione di **IInlineConstraintResolver** li aggiunge in modo specifico.

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>I parametri URI facoltativo e i valori predefiniti

È possibile rendere parametro URI facoltativo mediante l'aggiunta di un punto interrogativo al parametro di route. Se un parametro di route è facoltativo, è necessario definire un valore predefinito per il parametro del metodo.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

In questo esempio `/api/books/locale/1033` e `/api/books/locale` restituiscono la stessa risorsa.

In alternativa, è possibile specificare un valore predefinito all'interno del modello di route, come indicato di seguito:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

Si tratta quasi lo stesso dell'esempio precedente, ma è presente una leggera differenza di comportamento quando viene applicato il valore predefinito.

- Nel primo esempio ("{lcid?}"), il valore predefinito di 1033 viene assegnato direttamente al parametro del metodo, pertanto il parametro avrà questo valore esatto.
- Nel secondo esempio ("{lcid = 1033}"), il valore predefinito di "1033" esegue il processo di associazione di modelli. Il raccoglitore di modelli predefinito convertirà "1033" al valore numerico 1033. Tuttavia, è possibile inserire in uno strumento di associazione di modelli personalizzato, che potrebbe eseguire un'operazione diversa.

(Nella maggior parte dei casi, a meno che non si dispone di strumenti di associazione di modelli personalizzato nella pipeline, le due forme sarà equivalente.)

<a id="route-names"></a>
## <a name="route-names"></a>Nomi di route

Nell'API Web, ogni route ha un nome. I nomi di route sono utili per la generazione di collegamenti, in modo che sia possibile includere un collegamento in una risposta HTTP.

Per specificare il nome della route, impostare il **nome** proprietà sull'attributo. Nell'esempio seguente viene illustrato come impostare il nome della route e anche come usare il nome della route durante la generazione di un collegamento.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>Ordine della route

Quando il framework cerca la corrispondenza con un URI con una route, la valuta le route in un ordine particolare. Per specificare l'ordine, impostare il **RouteOrder** proprietà dell'attributo di route. I valori più bassi vengono valutati per primi. Il valore dell'ordine predefinito è zero.

Ecco come viene determinata l'ordinamento totale:

1. Confrontare le **RouteOrder** proprietà dell'attributo di route.
2. Esaminare ogni segmento URI nel modello di route. Per ogni segmento, ordine come indicato di seguito: 

    1. Segmenti del valore letterale.
    2. Parametri di route con vincoli.
    3. Parametri di route senza vincoli.
    4. Segmenti di parametri con caratteri jolly con vincoli.
    5. Segmenti di parametri con caratteri jolly senza vincoli.
3. Nel caso di parità, le route sono ordinate per un confronto tra maiuscole e minuscole ordinale tra stringhe ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) del modello di route.

Di seguito è riportato un esempio. Si supponga di che definisce il controller seguente:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

Queste route vengono ordinate come indicato di seguito.

1. orders/details
2. gli ordini / {id}
3. orders/{customerName}
4. gli ordini / {\*date}
5. gli ordini / in sospeso

Si noti che "Dettagli" sono un segmento valore letterale e viene visualizzata prima di "{id}" ma "in sospeso" viene visualizzato ultimo perché il **RouteOrder** proprietà è 1. (In questo esempio presuppone che si sono Nessun cliente denominato "Dettagli" o "pending". In generale, provare a evitare route ambigue. In questo esempio, un modello di route migliorato per `GetByCustomer` è "customers / {customerName}")
