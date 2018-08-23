---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Associazione di parametri nell'API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/11/2013
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4d29f087cd658faf1fadb0d9a85e9f32c03a2b3f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827640"
---
<a name="parameter-binding-in-aspnet-web-api"></a>Associazione di parametri nell'API Web ASP.NET
====================
da [Mike Wasson](https://github.com/MikeWasson)

Quando l'API Web chiama un metodo in un controller, è necessario impostare i valori per i parametri, un processo denominato *associazione*. Questo articolo descrive come API Web associa parametri e come è possibile personalizzare il processo di associazione.

Per impostazione predefinita, API Web Usa le regole seguenti per associare i parametri:

- Se il parametro è un tipo "semplice", API Web prova a ottenere il valore dall'URI. I tipi semplici includono .NET [i tipi primitivi](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**e così via), oltre a **TimeSpan**, **DateTime**, **Guid**, **decimal**, e **stringa**, *plus* qualsiasi tipo con un convertitore di tipi può convertire da una stringa. (Ulteriori informazioni sui convertitori di tipi in un secondo momento.)
- Per i tipi complessi, API Web tenta di leggere il valore dal corpo del messaggio, usando un [formattatore di media type](media-formatters.md).

Ad esempio, ecco un metodo del controller API Web tipico:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

Il *id* parametro è un &quot;semplice&quot; digitare, in modo che l'API Web prova a ottenere il valore dall'URI della richiesta. Il *elemento* parametro è un tipo complesso, API Web Usa quindi un formattatore di media type per leggere il valore dal corpo della richiesta.

Per ottenere un valore dall'URI, API Web Cerca nei dati di route e la stringa di query URI. I dati della route viene popolati quando il sistema di routing analizza l'URI e lo fa corrisponda a una route. Per altre informazioni, vedere [Routing e selezione dell'azione](../web-api-routing-and-actions/routing-and-action-selection.md).

Nella parte restante di questo articolo, mostrerò come è possibile personalizzare il processo di associazione del modello. Per i tipi complessi, tuttavia, è consigliabile usare i formattatori di media type laddove possibile. Un principio chiave del protocollo HTTP è che le risorse vengano inviate nel corpo del messaggio, utilizzando la negoziazione del contenuto per specificare la rappresentazione della risorsa. Formattatori di Media type sono stati progettati per esattamente questo scopo.

## <a name="using-fromuri"></a>Utilizzo [FromUri]

Per forzare l'API Web per la lettura di un tipo complesso dall'URI, aggiungere il **[FromUri]** al parametro. L'esempio seguente definisce una `GeoPoint` tipo, insieme a un metodo del controller che ottiene il `GeoPoint` dall'URI.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

Il client può inserire i valori di latitudine e longitudine nella stringa di query e API Web verranno usati per costruire un `GeoPoint`. Ad esempio:

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>Utilizzo [FromBody]

Per forzare l'API Web da cui leggere il corpo della richiesta di un tipo semplice, aggiungere il **[FromBody]** al parametro:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

In questo esempio, API Web utilizzerà un formattatore di media type per leggere il valore della *nome* dal corpo della richiesta. Di seguito è riportato un esempio di richiesta client.

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

Quando un parametro è [FromBody], API Web Usa l'intestazione Content-Type per selezionare un formattatore. In questo esempio, è il tipo di contenuto &quot;application/json&quot; e il corpo della richiesta è una stringa JSON non elaborata (non un oggetto JSON).

Al massimo un parametro è consentito leggere dal corpo del messaggio. Pertanto, non funzionerà:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

Il motivo di questa regola è che il corpo della richiesta potrebbe essere archiviato in un flusso non memorizzato nel buffer che può essere letto una sola volta.

## <a name="type-converters"></a>Convertitori di tipi

È possibile rendere API Web considera una classe come un tipo semplice (in modo che l'API Web proverà a eseguirne l'associazione dall'URI) mediante la creazione di un **TypeConverter** e fornendo una conversione di stringhe.

Il codice seguente illustra un `GeoPoint` classe che rappresenta un punto geografico, oltre a una **TypeConverter** che consente di convertire da stringhe a `GeoPoint` istanze. Il `GeoPoint` classe è decorata con un **[TypeConverter]** attributo per specificare il convertitore di tipi. (In questo esempio è stato ispirato dal post di blog di Mike stallo [come eseguire l'associazione agli oggetti personalizzati in firme di azione in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

A questo punto l'API Web considererà `GeoPoint` come un tipo semplice, vale a dire si tenterà di associare `GeoPoint` parametri dall'URI. Non è necessario includere **[FromUri]** sul parametro.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

Il client può richiamare il metodo con un URI simile al seguente:

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>Strumenti di associazione

Un'opzione più flessibile rispetto a un convertitore di tipi consiste nel creare uno strumento di associazione di modelli personalizzato. Con uno strumento di associazione, è necessario accedere come richiesta HTTP, la descrizione dell'azione e i valori non elaborati dai dati di route.

Per creare uno strumento di associazione, implementare il **IModelBinder** interfaccia. Questa interfaccia definisce un singolo metodo, **BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

Di seguito è uno strumento di associazione per `GeoPoint` oggetti.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

Uno strumento di associazione ottiene i valori di input non elaborati da un *provider di valori*. Questo progetto separa le due funzioni distinte:

- Il provider di valori accetta la richiesta HTTP e popola un dizionario di coppie chiave-valore.
- Lo strumento di associazione di modelli Usa questo dizionario per popolare il modello.

Il provider di valori predefiniti nell'API Web ottiene i valori di dati della route e la stringa di query. Ad esempio, se l'URI è `http://localhost/api/values/1?location=48,-122`, il provider di valori Crea le seguenti coppie chiave-valore:

- id = &quot;1&quot;
- percorso = &quot;48,122&quot;

(Si suppone che il modello di route predefinito, ovvero &quot;api / {controller} / {id}&quot;.)

Il nome del parametro da associare è archiviato nel **ModelBindingContext.ModelName** proprietà. Lo strumento di associazione di modelli cerca di una chiave con questo valore nel dizionario. Se il valore presente e può essere convertito in un `GeoPoint`, lo strumento individuerebbe assegna il valore associato per il **ModelBindingContext.Model** proprietà.

Si noti che lo strumento di associazione del modello non è limitata a una conversione del tipo semplice. In questo esempio viene innanzitutto esaminato lo strumento di associazione di modelli in una tabella di percorsi noti e se il problema persiste, utilizza la conversione di tipo.

**Impostare il gestore di associazione di modelli**

Esistono diversi modi per impostare un raccoglitore di modelli. In primo luogo, è possibile aggiungere un **[ModelBinder]** al parametro.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

È anche possibile aggiungere un **[ModelBinder]** al tipo di attributo. API Web utilizzerà lo strumento di associazione del modello specificato per tutti i parametri di quel tipo.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

Infine, è possibile aggiungere un provider dello strumento di associazione del modello per il **HttpConfiguration**. Un provider dello strumento di associazione del modello è semplicemente una classe factory che crea uno strumento di associazione. È possibile creare un provider mediante la derivazione dal [elemento ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) classe. Tuttavia, se il gestore di associazione di modello gestisce un solo tipo, è più facile da utilizzare l'oggetto incorporato **SimpleModelBinderProvider**, che è progettato per questo scopo. A tale scopo, osservare il codice indicato di seguito.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

Con un provider di associazione di modelli, è comunque necessario aggiungere il **[ModelBinder]** al parametro, a indicare API Web che è necessario utilizzare uno strumento di associazione di modello e non un formattatore di media type. Ma ora non è necessario specificare il tipo di strumento di associazione nell'attributo:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>Provider di valori

Ho accennato che uno strumento di associazione ottiene i valori da un provider di valori. Per scrivere un provider di valori personalizzati, implementare il **IValueProvider** interfaccia. Di seguito è riportato un esempio che esegue il pull dei valori dai cookie nella richiesta:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

È anche necessario creare una factory del provider di valore mediante la derivazione dal **ValueProviderFactory** classe.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

Aggiungere la factory del provider di valore per il **HttpConfiguration** come indicato di seguito.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

API Web di comporre tutte i provider di valori, pertanto, quando chiama uno strumento di associazione **ValueProvider.GetValue**, lo strumento individuerebbe riceve il valore del primo provider di valori che è in grado di produrre.

In alternativa, è possibile impostare la factory del provider di valore a livello di parametro usando il **ValueProvider** attributo, come indicato di seguito:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

Questa impostazione, API Web Usa l'associazione di modelli con la factory del provider di valore specificato di non usare uno qualsiasi di altri provider di valore registrato.

## <a name="httpparameterbinding"></a>HttpParameterBinding

Gli strumenti di associazione sono un'istanza specifica di un meccanismo più generale. Se si esamina il **[ModelBinder]** attributo, si noterà che deriva dalla classe astratta **ParameterBindingAttribute** classe. Questa classe definisce un singolo metodo, **GetBinding**, che restituisce un' **HttpParameterBinding** oggetto:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

Un' **HttpParameterBinding** è responsabile per l'associazione di un parametro a un valore. Nel caso del **[ModelBinder]**, l'attributo restituisce un **HttpParameterBinding** implementazione che usa un' **interfaccia IModelBinder** per eseguire l'associazione effettiva. È anche possibile implementare una propria **HttpParameterBinding**.

Ad esempio, si supponga di voler ottenere eTag dalla `if-match` e `if-none-match` intestazioni nella richiesta. Si inizierà con la definizione di una classe per rappresentare gli ETag.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

Si definirà anche un'enumerazione per indicare se il valore ETag da ottenere il `if-match` intestazione o il `if-none-match` intestazione.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

Di seguito è riportato un **HttpParameterBinding** che ottiene il valore ETag nell'intestazione desiderato e lo associa a un parametro di tipo ETag:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

Il **ExecuteBindingAsync** metodo esegue l'associazione. All'interno di questo metodo, aggiungere il valore del parametro associato per il **oggetto ActionArgument** dizionario nel **HttpActionContext**.

> [!NOTE]
> Se il **ExecuteBindingAsync** metodo legge il corpo del messaggio di richiesta, sostituire il **WillReadBody** proprietà da restituire true. Il corpo della richiesta potrebbe essere un flusso non memorizzato nel buffer che può essere letto una sola volta, in modo che l'API Web consente di applicare una regola che al massimo uno di associazione può leggere il corpo del messaggio.


Per applicare un oggetto personalizzato **HttpParameterBinding**, è possibile definire un attributo che deriva da **ParameterBindingAttribute**. Per la `ETagParameterBinding`, verranno definiti due attributi, uno per `if-match` informazioni sulle intestazioni e uno per `if-none-match` intestazioni. Entrambi derivano dalla classe base astratta.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

Ecco un metodo del controller che utilizza il `[IfNoneMatch]` attributo.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

Altre origini oltre a **ParameterBindingAttribute**, è presente un altro hook per l'aggiunta di un oggetto personalizzato **HttpParameterBinding**. Nel **HttpConfiguration** oggetto, il **ParameterBindingRules** proprietà è una raccolta di funzioni anomymous typu (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**). Ad esempio, è possibile aggiungere una regola che utilizza qualsiasi parametro ETag in un metodo GET `ETagParameterBinding` con `if-none-match`:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

La funzione deve restituire `null` per i parametri in cui l'associazione non è applicabile.

## <a name="iactionvaluebinder"></a>IActionValueBinder

L'intero processo di associazione di parametri è controllato da un servizio modulare **IActionValueBinder**. L'implementazione predefinita di **IActionValueBinder** esegue le operazioni seguenti:

1. Cercare un **ParameterBindingAttribute** sul parametro. Sono inclusi **[FromBody]**, **[FromUri]**, e **[ModelBinder]**, o gli attributi personalizzati.
2. In caso contrario, esaminare **HttpConfiguration.ParameterBindingRules** per una funzione che restituisce un valore non null **HttpParameterBinding**.
3. In caso contrario, usare le regole predefinite che descritti in precedenza. 

    - Se il tipo di parametro è "semplice" o ha un convertitore di tipi, eseguire l'associazione dall'URI. Ciò equivale a inserire il **[FromUri]** attributo sul parametro.
    - In caso contrario, provare a leggere il parametro dal corpo del messaggio. Ciò equivale a inserire **[FromBody]** sul parametro.

Se si desidera, è possibile sostituire l'intera **IActionValueBinder** servizio con un'implementazione personalizzata.

## <a name="additional-resources"></a>Risorse aggiuntive

[Esempio di associazione di parametri personalizzati](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Mike stallo ha scritto una buona serie di post di blog sull'associazione di parametri di API Web:

- [Funzionamento delle API Web sull'associazione di parametri](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Associazione di parametro di tipo MVC per API Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [Come eseguire l'associazione agli oggetti personalizzati in firme di azione MVC o Web API](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [Come creare un provider di valori personalizzati in API Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [Associazione di parametri di API Web dietro le quinte](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
