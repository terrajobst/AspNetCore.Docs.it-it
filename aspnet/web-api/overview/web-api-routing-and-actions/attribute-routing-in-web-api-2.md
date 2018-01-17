---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Attributo di Routing in ASP.NET Web API 2 | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 7c563f566b8456b63ffe0a3c4876432c60a19e89
ms.sourcegitcommit: 87168cdc409e7a7257f92a0f48f9c5ab320b5b28
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/17/2018
---
<a name="attribute-routing-in-aspnet-web-api-2"></a>Attributo di Routing in ASP.NET Web API 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

*Routing* è come API Web corrisponde a un URI a un'azione. Web API 2 supporta un nuovo tipo di routing, denominata *attributo routing*. Come suggerisce il nome, l'attributo routing utilizza gli attributi per definire le route. Attributo routing offre maggiore controllo sull'URI dell'API web. Ad esempio, è possibile creare facilmente gli URI che descrivono le gerarchie di risorse.

Lo stile di versioni precedenti di routing, denominato basato sulle convenzioni di routing, è ancora completamente supportato. In effetti, è possibile combinare entrambe le tecniche nello stesso progetto.

In questo argomento viene illustrato come abilitare il routing di attributo e descrive le varie opzioni per il routing di attributo. Per un'esercitazione end-to-end che utilizza il routing di attributo, vedere [creare un'API REST con il Routing degli attributi in Web API 2](create-a-rest-api-with-attribute-routing.md).


## <a name="prerequisites"></a>Prerequisiti

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional o Enterprise Edition

In alternativa, è possibile utilizzare Gestione pacchetti NuGet per installare i pacchetti necessari. Dal **strumenti** menu in Visual Studio, selezionare **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**. Immettere il comando seguente nella finestra della Console di gestione pacchetti:

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>Attributo perché Routing?

La prima versione dell'API Web utilizzato *basato sulle convenzioni* routing. In tale tipo di routing, si definisce uno o più modelli di route, che sono fondamentalmente stringhe con parametri. Quando il framework riceve una richiesta, corrisponde a URI di base del modello di route. (Per ulteriori informazioni sul routing basato sulle convenzioni, vedere [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).

Un vantaggio offerto da basato sulle convenzioni di routing è che i modelli sono definiti in un'unica posizione e le regole di routing vengono applicate in modo coerente in tutti i controller. Purtroppo, basato sulle convenzioni di routing rende difficile supportare alcuni modelli di URI che sono comuni nelle API REST. Ad esempio, le risorse spesso contengono risorse figlio: sono presenti ordini, filmati hanno attori, documentazione avere autori e così via. È naturale per creare URI che riflette queste relazioni:

`/customers/1/orders`

Questo tipo di URI è difficile creare utilizzando basato sulle convenzioni di routing. Anche se può essere eseguito, i risultati non scalabilità anche se si dispone di numerosi controller o tipi di risorse.

Con il routing di attributo, risulta semplice per definire una route per questo URI. È sufficiente aggiungere un attributo azione del controller:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

Ecco alcuni altri modelli di tale attributo routing rende facile.

**Controllo delle versioni API**

In questo esempio, "/ v1/api/prodotti" sarà indirizzato a un controller diverso rispetto a "/ v2/api/prodotti".

`/api/v1/products`  
`/api/v2/products`

**Segmenti URI overload**

In questo esempio, "1" è un numero di ordine, ma "pending" esegue il mapping a una raccolta.

`/orders/1`  
`/orders/pending`

**Più tipi di parametro**

In questo esempio, "1" è un numero di ordine, ma "2013/06/16" specifica una data.

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>Abilitazione del Routing di attributo

Per abilitare il routing di attributo, chiamare **MapHttpAttributeRoutes** durante la configurazione. Questo metodo di estensione è definito nel **System.Web.Http.HttpConfigurationExtensions** classe.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

Routing degli attributi possono essere combinati con [basato sulle convenzioni](routing-in-aspnet-web-api.md) routing. Per definire le route basato sulle convenzioni, chiamare il **MapHttpRoute** metodo.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Per ulteriori informazioni sulla configurazione Web API, vedere [la configurazione di ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>Nota: La migrazione da API Web 1

Prima di API Web 2, i modelli di progetto API Web generato codice simile al seguente:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

Se è attivato il routing di attributo, il codice genererà un'eccezione. Se si aggiorna un progetto di API Web esistente per l'utilizzo di routing degli attributi, assicurarsi di aggiornare il codice di configurazione per le operazioni seguenti:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> Per ulteriori informazioni, vedere [la configurazione di API Web con Hosting ASP.NET](../advanced/configuring-aspnet-web-api.md#webhost).


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>Aggiunta di attributi delle Route

Di seguito è riportato un esempio di una route definita utilizzando un attributo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

La stringa &quot;clienti / {customerId} / ordini&quot; è il modello URI per la route. API Web tenta di far corrispondere l'URI della richiesta per il modello. In questo esempio, "customers" e "orders" sono valori letterali segmenti e "{customerId}" è un parametro della variabile. Gli URI seguenti corrisponderebbe a questo modello:

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

È possibile limitare l'individuazione delle corrispondenze utilizzando [vincoli](#constraints), descritto più avanti in questo argomento.

Si noti che il &quot;{customerId}&quot; parametro nel modello di route corrisponde al nome del *customerId* parametro del metodo. Quando l'API Web richiama l'azione del controller, tenta di associare i parametri di route. Ad esempio, se l'URI è `http://example.com/customers/1/orders`, API Web tenta di associare il valore "1" per il *customerId* parametro dell'azione.

Un modello URI può includere diversi parametri:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

Metodi controller che non dispongono di un attributo route utilizzano basato sulle convenzioni di routing. In questo modo, è possibile combinare entrambi i tipi di routing nello stesso progetto.

## <a name="http-methods"></a>Metodi HTTP

API Web seleziona anche azioni in base al metodo HTTP della richiesta (GET, POST e così via). Per impostazione predefinita, Web API Cerca una corrispondenza tra maiuscole e minuscole con l'inizio del nome di metodo del controller. Ad esempio, un metodo del controller denominato `PutCustomers` corrisponde a una richiesta HTTP PUT.

È possibile ignorare questa convenzione dichiarando il metodo con i seguenti attributi:

- **[HttpDelete]**
- **[HttpGet]**
- **[HttpHead]**
- **[HttpOptions]**
- **[HttpPatch]**
- **[HttpPost]**
- **[HttpPut]**

Nell'esempio seguente viene eseguito il mapping del metodo CreateBook alle richieste HTTP POST.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

Per tutti gli altri metodi HTTP, compresi i metodi non standard, utilizzano il **AcceptVerbs** attributo, che accetta un elenco di metodi HTTP.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>Prefissi di route

Spesso, le route presenti in un controller avviino tutti con lo stesso prefisso. Ad esempio:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

È possibile impostare un prefisso comune per un intero controller utilizzando il **[RoutePrefix]** attributo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

Utilizzare una tilde (~) dell'attributo del metodo per sostituire il prefisso della route:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

Il prefisso della route può includere parametri:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>Vincoli della route

Vincoli della route consentono di limitare la modalità in cui vengono confrontati i parametri nel modello di route. La sintassi generale è &quot;{vincolo: parametro}&quot;. Ad esempio:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

In questo caso, la prima route verrà solo selezionata se il &quot;id&quot; segmento dell'URI è un numero intero. In caso contrario, verrà scelta la seconda route.

Nella tabella seguente vengono elencati i vincoli che sono supportati.

| Vincolo | Descrizione | Esempio |
| --- | --- | --- |
| alpha | Corrispondenze lettere maiuscole o minuscole dell'alfabeto latino caratteri (a-z, A-Z) | {x:alpha} |
| bool | Corrisponde a un valore booleano. | {x:bool} |
| datetime | Corrispondenze un **DateTime** valore. | {x:datetime} |
| decimal | Corrisponde a un valore decimale. | {x:decimal} |
| double | Corrisponde a un valore a virgola mobile a 64 bit. | {x:double} |
| float | Corrisponde a un valore a virgola mobile a 32 bit. | {x:float} |
| guid | Corrisponde a un valore GUID. | {x:guid} |
| int | Corrisponde a un valore integer a 32 bit. | {x:int} |
| length | Corrisponde a una stringa con la lunghezza specificata o in un determinato intervallo di lunghezza. | {x: length(6)} {x: length(1,20)} |
| long | Corrisponde a un valore integer a 64 bit. | {x:long} |
| max | Consente di ricercare un numero intero con un valore massimo. | {x:max(10)} |
| MaxLength | Corrisponde a una stringa con una lunghezza massima. | {x: maxlength(10)} |
| min | Consente di ricercare un numero intero con un valore minimo. | {x:min(10)} |
| minLength | Corrisponde a una stringa con una lunghezza minima. | {x: minlength(10)} |
| range | Consente di ricercare un numero intero in un intervallo di valori. | {x:range(10,50)} |
| regex | Corrisponde a un'espressione regolare. | {x:regex(^\d{3}-\d{3}-\d{4}$)} |

Si noti che alcuni dei vincoli, ad esempio &quot;min&quot;, accettano argomenti tra parentesi. È possibile applicare più vincoli a un parametro, separato da due punti.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>Vincoli della Route personalizzato

È possibile creare vincoli della route personalizzate implementando il **IHttpRouteConstraint** interfaccia. Ad esempio, il vincolo seguente limita un parametro a un valore intero diverso da zero.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

Il codice seguente viene illustrato come registrare il vincolo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

Ora è possibile applicare il vincolo nei percorsi di:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

È inoltre possibile sostituire l'intero **DefaultInlineConstraintResolver** classe mediante l'implementazione di **IInlineConstraintResolver** interfaccia. In questo modo sostituirà tutti i vincoli predefiniti, a meno che l'implementazione di **IInlineConstraintResolver** li aggiunge in modo specifico.

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>I parametri URI facoltativo e i valori predefiniti

È possibile apportare un parametro URI facoltativo mediante l'aggiunta di un punto interrogativo al parametro di route. Se un parametro di route è facoltativo, è necessario definire un valore predefinito per il parametro del metodo.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

In questo esempio, `/api/books/locale/1033` e `/api/books/locale` restituiscono la stessa risorsa.

In alternativa, è possibile specificare un valore predefinito all'interno del modello di route, come indicato di seguito:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

Questo è pressoché lo stesso dell'esempio precedente, ma è presente una leggera differenza di comportamento quando viene applicato il valore predefinito.

- Nel primo esempio ("{lcid?}"), il valore predefinito di 1033 viene assegnato direttamente al parametro del metodo, pertanto il parametro avrà il valore esatto.
- Nel secondo esempio ("{lcid = 1033}"), il valore predefinito di "1033" passa attraverso il processo di associazione di modelli. Raccoglitore di modelli predefinito convertirà "1033" al valore numerico 1033. Tuttavia, è possibile collegare un raccoglitore di modelli personalizzati, che potrebbe eseguire un'operazione diversa.

(Nella maggior parte dei casi, a meno che non si dispone di raccoglitori di modelli personalizzati nella pipeline, le due forme sarà equivalente.)

<a id="route-names"></a>
## <a name="route-names"></a>Nomi di route

Nell'API Web, ogni route ha un nome. I nomi di route sono utili per la generazione di collegamenti, in modo che è possibile includere un collegamento in una risposta HTTP.

Per specificare il nome della route, impostare il **nome** proprietà dell'attributo. Nell'esempio seguente viene illustrato come impostare il nome della route, nonché come utilizzare il nome di route per la generazione di un collegamento.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>Ordine della route

Quando il framework tenta di abbinare un URI con una route, valuta le route presenti in un ordine particolare. Per specificare l'ordine, impostare il **RouteOrder** proprietà dell'attributo della route. I valori più bassi vengono valutati per primi. Il valore dell'ordine predefinito è zero.

Ecco come viene determinata l'ordinamento totale:

1. Confrontare il **RouteOrder** proprietà dell'attributo di route.
2. Esaminare ogni segmento URI nel modello di route. Per ogni segmento, ordine come indicato di seguito: 

    1. Valore letterale segmenti.
    2. Parametri di route con vincoli.
    3. Parametri di route senza vincoli.
    4. Segmenti di parametro con caratteri jolly con vincoli.
    5. Segmenti di parametro con caratteri jolly senza vincoli.
3. Nel caso di pareggio, le route vengono ordinate in un confronto ordinale tra stringhe tra maiuscole e minuscole ([OrdinalIgnoreCase](https://msdn.microsoft.com/en-us/library/system.stringcomparer.ordinalignorecase.aspx)) del modello di route.

Di seguito è riportato un esempio. Si supponga di che definisce il seguente controller:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

Queste route vengono ordinate nel modo seguente.

1. orders/details
2. orders/{id}
3. orders/{customerName}
4. orders/{\*date}
5. orders/pending

Si noti che "Dettagli" sono un valore letterale segmento e viene visualizzato prima di "{id}", ma "in sospeso" viene visualizzato ultima perché il **RouteOrder** proprietà è 1. (In questo esempio presuppone che vi sono Nessun cliente denominato "Dettagli" o "in sospeso". In generale, tentare di evitare cicli ambigui. In questo esempio, un modello di route migliore per `GetByCustomer` è "clienti / {NomeCliente}")
