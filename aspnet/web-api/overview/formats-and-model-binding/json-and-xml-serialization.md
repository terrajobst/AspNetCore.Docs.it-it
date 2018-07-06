---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: Serializzazione JSON e XML nell'API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 05/30/2012
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: c6610eebbc6bd03426771087f0112c1ffa63aa15
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836321"
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a>Serializzazione JSON e XML nell'API Web ASP.NET
====================
da [Mike Wasson](https://github.com/MikeWasson)

Questo articolo descrive i formattatori XML e JSON nell'API Web ASP.NET.

Nell'API Web ASP.NET, un *formattatore di media type* è un oggetto che può essere:

- Corpo del messaggio di oggetti CLR di lettura da HTTP
- Scrivere gli oggetti CLR in un corpo del messaggio HTTP

API Web fornisce formattatori di media type per JSON e XML. Per impostazione predefinita, il framework inserisce i formattatori nella pipeline. I client possono richiedere JSON o XML nell'intestazione Accept della richiesta HTTP.

## <a name="contents"></a>Sommario

- [Formattatore di Media Type JSON](#json_media_type_formatter)

    - [Proprietà di sola lettura](#json_readonly)
    - [Date](#json_dates)
    - [Rientro](#json_indenting)
    - [Convenzione camel](#json_camelcasing)
    - [Oggetti anonimi e con tipizzazione debole](#json_anon)
- [Formattatore di Media Type XML](#xml_media_type_formatter)

    - [Proprietà di sola lettura](#xml_readonly)
    - [Date](#xml_dates)
    - [Rientro](#xml_indenting)
    - [L'impostazione di serializzatori XML Per tipo](#xml_pertype)
- [Rimozione di JSON o un formattatore XML](#removing_the_json_or_xml_formatter)
- [La gestione di riferimenti circolari agli oggetti](#handling_circular_object_references)
- [Serializzazione di un oggetto di test](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>Formattatore di Media Type JSON

Formattazione JSON avviene tramite il **JsonMediaTypeFormatter** classe. Per impostazione predefinita **JsonMediaTypeFormatter** Usa le [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) libreria per eseguire la serializzazione. Json.NET è un progetto di open source di terze parti.

Se si preferisce, è possibile configurare il **JsonMediaTypeFormatter** classe da utilizzare il **DataContractJsonSerializer** invece di Json.NET. A tale scopo, impostare il **UseDataContractJsonSerializer** proprietà **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>Serializzazione JSON

In questa sezione descrive alcuni comportamenti specifici del formattatore JSON, usando il valore predefinito [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializzatore. Questo non è intende essere una documentazione completa della libreria di Json.NET. per altre informazioni, vedere la [documentazione di Json.NET](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>Elementi serializzati?

Per impostazione predefinita, tutti i campi e proprietà pubbliche sono inclusi nel file JSON serializzato. Per omettere una proprietà o un campo, decorarla con il **JsonIgnore** attributo.

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Se si preferisce un' &quot;acconsenti esplicitamente&quot; approccio, decorare la classe con il **DataContract** attributo. Se questo attributo è presente, i membri vengono ignorati a meno che non hanno le **DataMember**. È anche possibile usare **DataMember** serializzare membri privati.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Proprietà di sola lettura

Le proprietà di sola lettura vengono serializzate per impostazione predefinita.

<a id="json_dates"></a>
### <a name="dates"></a>Date

Per impostazione predefinita, Json.NET scrive le date [ISO 8601](http://www.w3.org/TR/NOTE-datetime) formato. Le date in formato UTC (Coordinated Universal Time) vengono scritti con un suffisso "Z". Le date nell'ora locale includono una differenza di fuso orario. Ad esempio:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

Per impostazione predefinita, Json.NET mantiene il fuso orario. È possibile eseguire l'override di questo impostando la proprietà DateTimeZoneHandling:

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Se si preferisce usare [formato di data JSON Microsoft](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) invece di ISO 8601, impostare il **DateFormatHandling** proprietà le impostazioni del serializzatore:

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Rientri

Per scrivere il JSON con rientro, impostare il **formattazione** se si imposta su **Indented**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Convenzione camel

Per scrivere i nomi di proprietà JSON con le iniziali maiuscole e minuscole, senza modificare il modello di dati, impostare il **CamelCasePropertyNamesContractResolver** sul serializzatore:

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Oggetti anonimi e con tipizzazione debole

Un metodo di azione può restituire un oggetto anonimo ed eseguirne la serializzazione in JSON. Ad esempio:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

Il corpo del messaggio di risposta conterrà il codice JSON seguente:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Se il web API riceve regime di controllo libero strutturati oggetti JSON dai client, è possibile deserializzare il corpo della richiesta a un **Newtonsoft.Json.Linq.JObject** tipo.

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

Tuttavia, è in genere preferibile usare gli oggetti dati fortemente tipizzati. Non è necessario analizzare i dati manualmente e si ottengono i vantaggi di convalida del modello.

Il serializzatore XML non supporta i tipi anonimi o **JObject** istanze. Se si usano queste funzionalità per i dati JSON, è necessario rimuovere il formattatore XML dalla pipeline, come descritto più avanti in questo articolo.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>Formattatore di Media Type XML

Formattazione XML viene fornita per il **XmlMediaTypeFormatter** classe. Per impostazione predefinita **XmlMediaTypeFormatter** Usa le **DataContractSerializer** classe per eseguire la serializzazione.

Se si preferisce, è possibile configurare il **XmlMediaTypeFormatter** da utilizzare il **XmlSerializer** anziché il **DataContractSerializer**. A tale scopo, impostare il **/usexmlserializer** proprietà **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

Il **XmlSerializer** classe supporta un set di tipi rispetto ai più ristretto **DataContractSerializer**, ma offre maggiore controllo sul codice XML risultante. È consigliabile usare **XmlSerializer** se è necessario associare uno schema XML esistente.

### <a name="xml-serialization"></a>Serializzazione XML

In questa sezione descrive alcuni comportamenti specifici del formattatore XML, usando il valore predefinito **DataContractSerializer**.

Per impostazione predefinita, la classe DataContractSerializer si comporta come segue:

- Vengono serializzati tutti i campi e proprietà di lettura/scrittura pubblica. Per omettere una proprietà o un campo, decorarla con il **IgnoreDataMember** attributo.
- Membri privati e protetti non vengono serializzati.
- Le proprietà di sola lettura non vengono serializzate. (Tuttavia, il contenuto di una proprietà della raccolta di sola lettura viene serializzato.)
- Nomi di classe e membro vengono scritti nel file XML, esattamente come appaiono nella dichiarazione di classe.
- Viene usato uno spazio dei nomi XML predefinito.

Se è necessario maggiore controllo sulla serializzazione, è possibile decorare la classe con il **DataContract** attributo. Quando questo attributo è presente, la classe viene serializzata come indicato di seguito:

- &quot;Fornire il consenso esplicito&quot; approccio: proprietà e i campi non vengono serializzati per impostazione predefinita. Per serializzare una proprietà o un campo, decorarla con il **DataMember** attributo.
- Per serializzare un membro privato o protetto, decorarla con il **DataMember** attributo.
- Le proprietà di sola lettura non vengono serializzate.
- Per modificare come viene visualizzato il nome della classe nel codice XML, impostare il *Name* parametro nel **DataContract** attributo.
- Per modificare l'aspetto di un nome di membro nel codice XML, impostare il *Name* parametro nel **DataMember** attributo.
- Per modificare lo spazio dei nomi XML, impostare il *Namespace* parametro le **DataContract** classe.

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Proprietà di sola lettura

Le proprietà di sola lettura non vengono serializzate. Se una proprietà di sola lettura ha un campo privato sottostante, è possibile contrassegnare il campo privato con il **DataMember** attributo. Questo approccio richiede la **DataContract** attributo della classe.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>Date

Le date vengono scritte nel formato ISO 8601. Ad esempio, &quot;2012-05-23T20:21:37.9116538Z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Rientri

Per scrivere codice XML rientrato, impostare il **rientro** proprietà **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>L'impostazione di serializzatori XML Per tipo

È possibile impostare diversi serializzatori XML per tipi CLR diversi. Ad esempio, potrebbe essere un oggetto dati specifico che richiede **XmlSerializer** per garantire la compatibilità con le versioni precedenti. È possibile usare **XmlSerializer** per questo oggetto e continuare a usare **DataContractSerializer** per altri tipi.

Per impostare un serializzatore XML per un determinato tipo, chiamare **SetSerializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

È possibile specificare una **XmlSerializer** o qualsiasi oggetto che deriva da **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Rimozione di JSON o un formattatore XML

È possibile rimuovere il formattatore JSON o il formattatore XML dall'elenco di formattatori, se si preferisce non usarle. I motivi principali per eseguire questa operazione sono:

- Per limitare le risposte di API web a un tipo di supporti particolare. Ad esempio, si potrebbe decidere di supportano solo le risposte JSON e rimuovere il formattatore XML.
- Per sostituire il formattatore predefinito con un formattatore personalizzato. Ad esempio, è Impossibile sostituire il formattatore JSON con la propria implementazione personalizzata di un formattatore JSON.

Il codice seguente viene illustrato come rimuovere i formattatori predefiniti. Chiamarla dalle **Application\_avviare** metodo, definito in Global. asax.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>La gestione di riferimenti circolari agli oggetti

Per impostazione predefinita, i formattatori # UID JSON e XML scrivono tutti gli oggetti come valori. Se due proprietà si riferiscono allo stesso oggetto oppure se l'oggetto stesso viene visualizzato due volte in una raccolta, il formattatore eseguirà la serializzazione dell'oggetto due volte. Questo è un problema specifico se l'oggetto grafico contiene cicli, in quanto il serializzatore genera un'eccezione quando viene rilevato un ciclo nel grafico.

Considerare i seguenti modelli a oggetti e i controller.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

Richiama l'azione causerà il formattatore da generata un'eccezione, si traduce in un stato codice 500 (errore Server interno) di risposta al client.

Per conservare i riferimenti agli oggetti in JSON, aggiungere il codice seguente al **Application\_avviare** metodo nel file Global. asax:

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

A questo punto l'azione del controller restituirà JSON simile alla seguente:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Si noti che il serializzatore aggiunge un' &quot;$id&quot; proprietà per entrambi gli oggetti. Inoltre, rileva che la proprietà Employee.Department crea un ciclo, in modo che sostituisce il valore con un riferimento all'oggetto: {&quot;$ref&quot;:&quot;1&quot;}.

> [!NOTE]
> Riferimenti a oggetti non sono standard in formato JSON. Prima di usare questa funzionalità, valutare se i client saranno in grado di analizzare i risultati. Potrebbe essere preferibile semplicemente rimuovere cicli dal grafico. Ad esempio, il collegamento da Employee al reparto non è realmente necessaria in questo esempio.


Per conservare i riferimenti agli oggetti in XML, sono disponibili due opzioni. L'opzione più semplice consiste nell'aggiungere `[DataContract(IsReference=true)]` alla classe del modello. Il *IsReference* parametro consente riferimenti a oggetti. Tenere presente che **DataContract** rende serializzazione acconsenti esplicitamente, pertanto è necessario aggiungere **DataMember** attributi alle proprietà:

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

A questo punto il formattatore produrrà XML simile al seguente:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Se si desidera evitare attributi nella classe di modello, è disponibile un'altra opzione: creare una nuova specifica del tipo **DataContractSerializer** dell'istanza e impostare *preserveObjectReferences* a **true**  nel costruttore. Impostare quindi questa istanza come un serializzatore per tipo nel formattatore di media type XML. Il codice seguente mostra come eseguire questa operazione:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Serializzazione di un oggetto di test

Quando si progetta l'API web, è utile testare la modalità di serializzazione di oggetti dati. È possibile farlo senza la creazione di un controller o richiamare un'azione del controller.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
