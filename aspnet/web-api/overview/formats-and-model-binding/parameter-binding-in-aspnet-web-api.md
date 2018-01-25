---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Parametro di associazione in ASP.NET Web API | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5aa532137436922519c86246ebfa834910ac0d86
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="parameter-binding-in-aspnet-web-api"></a>Parametro di associazione in ASP.NET Web API
====================
da [Mike Wasson](https://github.com/MikeWasson)

Quando l'API Web chiama un metodo in un controller, è necessario impostare i valori per i parametri, un processo denominato *associazione*. In questo articolo viene descritto come API Web associa i parametri e come è possibile personalizzare il processo di associazione.

Per impostazione predefinita, l'API Web utilizza le regole seguenti per associare parametri:

- Se il parametro è un tipo "semplice", l'API Web tenta di ottenere il valore dell'URI. I tipi semplici includono .NET [tipi primitivi](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **doppie**e così via), oltre a **TimeSpan**, **DateTime**, **Guid**, **decimale**, e **stringa**, *più* qualsiasi tipo con un convertitore di tipi che è possibile convertire una stringa. (Ulteriori informazioni sui convertitori di tipi in un secondo momento.)
- Per tipi complessi, API Web tenta di leggere il valore dal corpo del messaggio, utilizzando un [formattatore di media type](media-formatters.md).

Ad esempio, ecco un metodo del controller Web API tipico:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

Il *id* parametro è un &quot;semplice&quot; digitare, in modo che l'API Web tenta di ottenere il valore dall'URI della richiesta. Il *elemento* parametro è un tipo complesso, in modo API Web utilizza un formattatore di media type per leggere il valore dal corpo della richiesta.

Per ottenere un valore dall'URI, API Web Cerca nella stringa di query e di dati della route. I dati della route viene popolati quando il sistema di routing analizza l'URI e a esso corrispondente a una route. Per ulteriori informazioni, vedere [Routing e la selezione di azione](../web-api-routing-and-actions/routing-and-action-selection.md).

Nella parte restante di questo articolo, verrà illustrato come è possibile personalizzare il processo di associazione del modello. Per tipi complessi, tuttavia, è consigliabile usare i formattatori di media type laddove possibile. Un principio chiave HTTP è che le risorse vengano inviate nel corpo del messaggio, utilizzando la negoziazione del contenuto per specificare la rappresentazione della risorsa. Formattatori di Media type sono stati progettati per questo scopo.

## <a name="using-fromuri"></a>Utilizzo [FromUri]

Per forzare l'API Web per la lettura di un tipo complesso dall'URI, aggiungere il **[FromUri]** al parametro. L'esempio seguente definisce un `GeoPoint` tipo, insieme a un metodo del controller che ottiene il `GeoPoint` dall'URI.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

Il client è possibile inserire i valori di latitudine e longitudine nella stringa di query e l'API Web verrà utilizzata per creare un `GeoPoint`. Ad esempio:

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>Utilizzo [FromBody]

Per forzare l'API Web da cui leggere il corpo della richiesta di un tipo semplice, aggiungere il **[FromBody]** al parametro:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

In questo esempio, API Web utilizzerà un formattatore di media type per leggere il valore di *nome* dal corpo della richiesta. Di seguito è riportato un esempio di richiesta client.

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

Quando un parametro è [FromBody], API Web utilizza l'intestazione Content-Type per selezionare un formattatore. In questo esempio, il tipo di contenuto è &quot;application/json&quot; e il corpo della richiesta è una stringa JSON non elaborata (non un oggetto JSON).

È consentito al massimo un parametro di leggere il corpo del messaggio. Pertanto, non funzionerà:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

Il motivo per la regola è che il corpo della richiesta potrebbe essere archiviato in un flusso non memorizzato nel buffer che può essere letto una sola volta.

## <a name="type-converters"></a>Convertitori di tipi

È possibile rendere API Web considerare una classe come un tipo semplice (in modo che l'API Web tenterà di associarlo dall'URI) creando un **TypeConverter** e fornire una conversione di stringhe.

Il codice seguente illustra un `GeoPoint` classe che rappresenta un punto geografico, oltre a una **TypeConverter** che consente di convertire da stringhe a `GeoPoint` istanze. Il `GeoPoint` classe è decorata con un **[TypeConverter]** attributo per specificare il convertitore di tipi. (In questo esempio è stato ispirato da post di blog di stallo Mike [l'associazione a oggetti personalizzati nelle firme di azione in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

Ora l'API Web considererà `GeoPoint` come un tipo semplice, ovvero tenterà di associare `GeoPoint` parametri dall'URI. Non è necessario includere **[FromUri]** sul parametro.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

Il client può richiamare il metodo con un URI simile al seguente:

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>Raccoglitori di modelli

Un'opzione più flessibile rispetto a un convertitore di tipi consiste nel creare un raccoglitore di modelli personalizzati. Con un raccoglitore di modelli, si possono accedere a elementi quali la richiesta HTTP, la descrizione dell'azione e i valori non elaborati dai dati di route.

Per creare un raccoglitore di modelli, implementare il **IModelBinder** interfaccia. Questa interfaccia definisce un singolo metodo, **BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

Ecco un raccoglitore di modelli per `GeoPoint` oggetti.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

Un raccoglitore di modelli Ottiene i valori di input non elaborati da un *provider di valori*. Questa progettazione separa due funzioni distinte:

- Il provider di valori accetta la richiesta HTTP e popola un dizionario di coppie chiave-valore.
- Lo strumento di associazione del modello viene utilizzato questo dizionario per popolare il modello.

Il provider di valori predefiniti nell'API Web ottiene i valori di dati della route e la stringa di query. Ad esempio, se l'URI è `http://localhost/api/values/1?location=48,-122`, il provider di valori Crea le coppie chiave-valore seguente:

- id = &quot;1&quot;
- percorso = &quot;48,122&quot;

(Si suppone che il modello di route predefinito, ovvero &quot;api / {controller} / {id}&quot;.)

Il nome del parametro per l'associazione viene archiviato nel **ModelBindingContext.ModelName** proprietà. Lo strumento di associazione del modello esegue la ricerca di una chiave con questo valore nel dizionario. Se il valore è presente e può essere convertito in un `GeoPoint`, lo strumento di associazione del modello assegna il valore associato per il **ModelBindingContext.Model** proprietà.

Si noti che il gestore di associazione del modello non è limitato a una conversione del tipo semplice. In questo esempio, il gestore di associazione del modello prima una ricerca in una tabella dei percorsi noti e se il problema persiste, utilizza la conversione di tipo.

**Impostare il gestore di associazione del modello**

Esistono diversi modi per impostare un raccoglitore di modelli. In primo luogo, è possibile aggiungere un **[ModelBinder]** al parametro.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

È inoltre possibile aggiungere un **[ModelBinder]** per il tipo di attributo. API Web utilizzerà lo strumento di associazione del modello specificato per tutti i parametri di quel tipo.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

Infine, è possibile aggiungere un provider dello strumento di associazione del modello per il **HttpConfiguration**. Un provider dello strumento di associazione del modello è semplicemente una classe factory che crea un raccoglitore di modelli. È possibile creare un provider mediante derivazione da di [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) classe. Tuttavia, se il gestore di associazione del modello gestisce un solo tipo, è più facile da utilizzare l'oggetto incorporato **SimpleModelBinderProvider**, che è progettato per questo scopo. A tale scopo, osservare il codice indicato di seguito.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

Con un provider di associazione di modelli, è comunque necessario aggiungere il **[ModelBinder]** al parametro, per indicare API Web che deve utilizzare un raccoglitore di modelli e non un formattatore di media type. Tuttavia, ora non è necessario specificare il tipo di strumento di associazione nell'attributo:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>Provider di valori

Ho detto che un raccoglitore di modelli Ottiene i valori da un provider di valori. Per scrivere un provider di valori personalizzati, implementare il **IValueProvider** interfaccia. Di seguito è riportato un esempio che inserisce i valori dai cookie nella richiesta:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

È anche necessario creare una factory di provider di valori mediante la derivazione da di **ValueProviderFactory** classe.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

Aggiungere la factory del provider di valore per il **HttpConfiguration** come indicato di seguito.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

API Web consente di comporre tutte i provider di valori, pertanto, quando viene chiamato un raccoglitore di modelli **ValueProvider.GetValue**, lo strumento di associazione del modello riceve il valore del primo provider di valori che è in grado di produrre il.

In alternativa, è possibile impostare la factory del provider di valore a livello di parametro utilizzando il **ValueProvider** attributo, come indicato di seguito:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

In questo modo l'API Web per utilizzare l'associazione di modelli con la factory del provider di valore specificato e non per usare uno degli altri provider di valore registrato.

## <a name="httpparameterbinding"></a>HttpParameterBinding

Raccoglitori di modelli sono un'istanza specifica di un meccanismo più generale. Se si esamina il **[ModelBinder]** attributo, si noterà che deriva dalla classe astratta **ParameterBindingAttribute** classe. Questa classe definisce un singolo metodo, **GetBinding**, che restituisce un **HttpParameterBinding** oggetto:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

Un **HttpParameterBinding** è responsabile per l'associazione di un parametro a un valore. In caso di **[ModelBinder]**, l'attributo restituisce un **HttpParameterBinding** implementazione che utilizza un **IModelBinder** per eseguire l'associazione effettiva. È anche possibile implementare una propria **HttpParameterBinding**.

Ad esempio, si supponga che si desidera ottenere eTag da `if-match` e `if-none-match` intestazioni della richiesta. Si inizierà definendo una classe per rappresentare valori eTag.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

È inoltre verranno definite un'enumerazione che indica se il valore ETag da ottenere il `if-match` intestazione o il `if-none-match` intestazione.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

Ecco un **HttpParameterBinding** che ottiene il valore ETag dall'intestazione desiderato e lo associa a un parametro di tipo ETag:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

Il **ExecuteBindingAsync** metodo esegue l'associazione. All'interno di questo metodo, aggiungere il valore del parametro associato per il **oggetto ActionArgument** dizionario il **HttpActionContext**.

> [!NOTE]
> Se il **ExecuteBindingAsync** metodo legge il corpo del messaggio di richiesta, eseguire l'override di **WillReadBody** proprietà da restituire true. Il corpo della richiesta potrebbe essere un flusso non memorizzato nel buffer che può essere letto solo una volta, in modo API Web consente di applicare una regola che al massimo un binding può leggere il corpo del messaggio.


Per applicare un oggetto personalizzato **HttpParameterBinding**, è possibile definire un attributo che deriva da **ParameterBindingAttribute**. Per `ETagParameterBinding`, definiamo due attributi, uno per `if-match` intestazioni e uno per `if-none-match` intestazioni. Derivano dalla classe base astratta.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

Ecco un metodo del controller che utilizza il `[IfNoneMatch]` attributo.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

Oltre a **ParameterBindingAttribute**, vi è un altro hook per l'aggiunta di un oggetto personalizzato **HttpParameterBinding**. Nel **HttpConfiguration** oggetto, il **ParameterBindingRules** proprietà è una raccolta di funzioni anomymous di tipo (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**). Ad esempio, è possibile aggiungere una regola che utilizza qualsiasi parametro ETag su un metodo GET `ETagParameterBinding` con `if-none-match`:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

La funzione deve restituire `null` per i parametri in cui l'associazione non è applicabile.

## <a name="iactionvaluebinder"></a>IActionValueBinder

L'intero processo di associazione di parametri è controllato da un servizio di collegamento, **IActionValueBinder**. L'implementazione predefinita di **IActionValueBinder** esegue le operazioni seguenti:

1. Cercare un **ParameterBindingAttribute** sul parametro. Ciò include **[FromBody]**, **[FromUri]**, e **[ModelBinder]**, o gli attributi personalizzati.
2. In caso contrario, Cerca in **HttpConfiguration.ParameterBindingRules** per una funzione che restituisce un valore non null **HttpParameterBinding**.
3. In caso contrario, utilizzare le regole predefinite descritte in precedenza. 

    - Se il tipo di parametro è "semplice" o ha un convertitore di tipi, associare dall'URI. Ciò equivale a inserire il **[FromUri]** attributo per il parametro.
    - In caso contrario, provare a leggere il parametro dal corpo del messaggio. Ciò equivale a inserire **[FromBody]** sul parametro.

Se si desidera, è possibile sostituire l'intero **IActionValueBinder** servizio con un'implementazione personalizzata.

## <a name="additional-resources"></a>Risorse aggiuntive

[Esempio di associazione di parametri personalizzati](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Mike stallo scritto una buona serie di post di blog sull'associazione di parametro API Web:

- [Funzionamento delle API Web sull'associazione di parametri](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Associazione di parametro di tipo MVC per l'API Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [L'associazione a oggetti personalizzati nelle firme di azione MVC o Web API](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [Come creare un provider di valori personalizzati in Web API](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [Associazione di parametri di API Web dietro le quinte](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
