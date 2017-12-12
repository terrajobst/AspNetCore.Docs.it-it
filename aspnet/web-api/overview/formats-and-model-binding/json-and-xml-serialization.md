---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: JSON e serializzazione XML in ASP.NET Web API | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2012
ms.topic: article
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: 7aafe4823d3a6090fae4a63f1a66fb2670ecb025
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a>La serializzazione XML in ASP.NET Web API e JSON
====================
da [Mike Wasson](https://github.com/MikeWasson)

Questo articolo vengono descritti i formattatori JSON e XML in ASP.NET Web API.

In ASP.NET Web API, un *formattatore di media type* è un oggetto che può essere:

- Corpo del messaggio agli oggetti CLR di lettura da un HTTP
- Scrittura di oggetti CLR in un corpo del messaggio HTTP

API Web fornisce formattatori di media type per JSON e XML. Per impostazione predefinita, il framework inserisce questi formattatori nella pipeline. I client possono richiedere JSON o XML nell'intestazione Accept della richiesta HTTP.

## <a name="contents"></a>Contenuto

- [Formattatore di Media Type JSON](#json_media_type_formatter)

    - [Proprietà di sola lettura](#json_readonly)
    - [Date](#json_dates)
    - [Stili rientri](#json_indenting)
    - [Maiuscole e minuscole camel](#json_camelcasing)
    - [Oggetti di tipo anonimi e con tipizzazione debole](#json_anon)
- [Formattatore di Media Type XML](#xml_media_type_formatter)

    - [Proprietà di sola lettura](#xml_readonly)
    - [Date](#xml_dates)
    - [Stili rientri](#xml_indenting)
    - [Serializzatori XML per ogni tipo di impostazione](#xml_pertype)
- [Rimozione di JSON o un formattatore XML](#removing_the_json_or_xml_formatter)
- [La gestione di riferimenti circolari agli oggetti](#handling_circular_object_references)
- [Serializzazione degli oggetti di test](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>Formattatore di Media Type JSON

Formattazione JSON viene eseguita il **JsonMediaTypeFormatter** classe. Per impostazione predefinita, **JsonMediaTypeFormatter** utilizza il [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) libreria per eseguire la serializzazione. Json.NET è un progetto di open source di terze parti.

Se si preferisce, è possibile configurare il **JsonMediaTypeFormatter** classe da utilizzare il **DataContractJsonSerializer** anziché Json.NET. A tale scopo, impostare il **UseDataContractJsonSerializer** proprietà **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>Serializzazione JSON

In questa sezione descrive alcuni comportamenti specifici del formattatore JSON, utilizzando il valore predefinito [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializzatore. Questo non deve essere una descrizione dettagliata della libreria di Json.NET. Per ulteriori informazioni, vedere il [Json.NET documentazione](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>Elementi serializzati?

Per impostazione predefinita, tutti i campi e le proprietà pubbliche sono inclusi nel file JSON serializzato. Per omettere una proprietà o un campo, decorarla con il **JsonIgnore** attributo.

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Se si preferisce un &quot;registrarsi&quot; approccio, contrassegnare la classe con il **DataContract** attributo. Se questo attributo è presente, i membri vengono ignorati a meno che non hanno il **DataMember**. È inoltre possibile utilizzare **DataMember** serializzare membri privati.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Proprietà di sola lettura

Proprietà di sola lettura vengono serializzate per impostazione predefinita.

<a id="json_dates"></a>
### <a name="dates"></a>date

Per impostazione predefinita, Json.NET scrive le date [ISO 8601](http://www.w3.org/TR/NOTE-datetime) formato. Le date in formato UTC (Coordinated Universal Time) vengono scritti con un suffisso "Z". Data nell'ora locale include una differenza di fuso orario. Ad esempio:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

Per impostazione predefinita, Json.NET mantiene il fuso orario. È possibile ignorare questa impostando la proprietà DateTimeZoneHandling:

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Se si preferisce utilizzare [formato JSON Microsoft](https://msdn.microsoft.com/en-us/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) anziché ISO 8601, impostare il **DateFormatHandling** proprietà impostazioni del serializzatore:

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Rientri

Per scrivere rientrato JSON, impostare il **formattazione** impostando su **Indented**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Maiuscole e minuscole camel

Per scrivere i nomi delle proprietà JSON con maiuscole e minuscole camel, senza modificare il modello di dati, impostare il **CamelCasePropertyNamesContractResolver** sul serializzatore:

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Oggetti di tipo anonimi e con tipizzazione debole

Un metodo di azione può restituire un oggetto anonimo ed eseguirne la serializzazione in JSON. Ad esempio:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

Il corpo del messaggio di risposta conterrà il codice JSON seguente:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Se il sito web API riceve regime di controllo libero strutturati oggetti JSON dai client, è possibile deserializzare il corpo della richiesta per un **Newtonsoft.Json.Linq.JObject** tipo.

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

Tuttavia, è preferibile utilizzare gli oggetti dati fortemente tipizzati. Quindi non è necessario analizzare i dati manualmente, e si ottengono i vantaggi di convalida del modello.

Il serializzatore XML non supporta i tipi anonimi o **JObject** istanze. Se si utilizzano queste funzionalità per i dati JSON, è necessario rimuovere il formattatore XML dalla pipeline, come descritto più avanti in questo articolo.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>Formattatore di Media Type XML

Formattazione XML viene eseguita il **XmlMediaTypeFormatter** classe. Per impostazione predefinita, **XmlMediaTypeFormatter** utilizza il **DataContractSerializer** classe per eseguire la serializzazione.

Se si preferisce, è possibile configurare il **XmlMediaTypeFormatter** per utilizzare il **XmlSerializer** anziché il **DataContractSerializer**. A tale scopo, impostare il **/usexmlserializer** proprietà **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

Il **XmlSerializer** classe supporta un set di tipi più ristretto **DataContractSerializer**, ma offre maggiore controllo sul codice XML risultante. È consigliabile utilizzare **XmlSerializer** se è necessario associare uno schema XML esistente.

### <a name="xml-serialization"></a>Serializzazione XML

In questa sezione descrive alcuni comportamenti specifici del formattatore XML, utilizzando il valore predefinito **DataContractSerializer**.

Per impostazione predefinita, DataContractSerializer si comporta come segue:

- Tutti i campi e le proprietà di lettura/scrittura pubblici vengono serializzati. Per omettere una proprietà o un campo, decorarla con il **IgnoreDataMember** attributo.
- Membri privati e protetti non vengono serializzati.
- Proprietà di sola lettura non vengono serializzate. (Tuttavia, il contenuto di una proprietà di raccolta di sola lettura viene serializzato.)
- Nomi di classe e membro vengono scritti nel file XML, esattamente come appaiono nella dichiarazione di classe.
- Viene utilizzato uno spazio dei nomi XML predefinito.

Se è necessario un maggiore controllo sulla serializzazione, è possibile contrassegnare la classe con il **DataContract** attributo. Quando questo attributo è presente, la classe viene serializzata nel modo seguente:

- &quot;Consente di partecipare&quot; approccio: non vengono serializzati i campi e proprietà per impostazione predefinita. Per serializzare una proprietà o un campo, decorarla con il **DataMember** attributo.
- Per serializzare un membro privato o protetto, decorarla con il **DataMember** attributo.
- Proprietà di sola lettura non vengono serializzate.
- Per modificare la modalità con cui il nome della classe visualizzato nel documento XML, impostare il *nome* parametro il **DataContract** attributo.
- Per modificare il nome di un membro visualizzato nel documento XML, impostare il *nome* parametro il **DataMember** attributo.
- Per modificare lo spazio dei nomi XML, impostare il *Namespace* parametro il **DataContract** classe.

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Proprietà di sola lettura

Proprietà di sola lettura non vengono serializzate. Se una proprietà di sola lettura ha un campo privato sottostante, è possibile contrassegnare il campo privato con le **DataMember** attributo. Questo approccio richiede il **DataContract** attributo della classe.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>date

Le date vengono scritti in formato ISO 8601. Ad esempio, &quot;2012-05-23T20:21:37.9116538Z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Rientri

Per scrivere codice XML rientrato, impostare il **rientro** proprietà **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Serializzatori XML per ogni tipo di impostazione

È possibile impostare diversi serializzatori XML per tipi CLR diversi. Ad esempio, potrebbe essere un oggetto dati specifico che richiede **XmlSerializer** per garantire la compatibilità con le versioni precedenti. È possibile utilizzare **XmlSerializer** per questo oggetto e continuare a usare **DataContractSerializer** per altri tipi.

Per impostare un serializzatore XML per un determinato tipo, chiamare **SetSerializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

È possibile specificare un **XmlSerializer** o qualsiasi oggetto che deriva da **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Rimozione di JSON o un formattatore XML

È possibile rimuovere il formattatore JSON o il formattatore XML dall'elenco di formattatori, se non si desidera utilizzarli. I motivi principali per eseguire questa operazione sono:

- Per limitare le risposte di API web per un determinato tipo di supporto. Ad esempio, si potrebbe decidere di supporta solo le risposte JSON e rimuovere il formattatore XML.
- Per sostituire il formattatore predefinito con un formattatore personalizzato. Ad esempio, è possibile sostituire il formattatore JSON con l'implementazione personalizzata di un formattatore JSON.

Il codice seguente viene illustrato come rimuovere i formattatori predefinita. Chiamare questo metodo dal **applicazione\_avviare** metodo, definito in Global. asax.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>La gestione di riferimenti circolari agli oggetti

Per impostazione predefinita, i formattatori JSON e XML scrivono tutti gli oggetti come valori. Se due proprietà che fanno riferimento allo stesso oggetto oppure se l'oggetto stesso viene visualizzato due volte in una raccolta, il formattatore verrà serializzato l'oggetto due volte. In questo modo un particolare problema se l'oggetto grafico contiene cicli, il serializzatore genererà un'eccezione quando viene rilevato un ciclo nel grafico.

Considerare i seguenti modelli a oggetti e i controller.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

Richiamo di questa azione genererà il formattatore da generata un'eccezione che viene convertita in un stato codice 500 (errore Server interno) di risposta al client.

Per mantenere i riferimenti agli oggetti in JSON, aggiungere il seguente codice al **applicazione\_avviare** metodo nel file Global. asax:

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

Ora azione del controller restituirà JSON che è simile al seguente:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Si noti che il serializzatore aggiunge un &quot;$id&quot; proprietà sia agli oggetti. Inoltre, viene rilevato che la proprietà Employee.Department crea un ciclo, in modo che sostituisce il valore con un riferimento all'oggetto: {&quot;$ref&quot;:&quot;1&quot;}.

> [!NOTE]
> Riferimenti a oggetti non sono standard in JSON. Prima di utilizzare questa funzionalità, considerare se i client saranno in grado di analizzare i risultati. Potrebbe essere meglio rimuovere cicli dal grafico. Ad esempio, il collegamento da Employee al reparto non è realmente necessaria in questo esempio.


Per mantenere i riferimenti agli oggetti in XML, sono disponibili due opzioni. L'opzione più semplice consiste nell'aggiungere `[DataContract(IsReference=true)]` alla classe del modello. Il *IsReference* parametro consente di riferimenti a oggetti. Tenere presente che **DataContract** rende serializzazione acconsentire esplicitamente, pertanto è necessario aggiungere **DataMember** gli attributi per le proprietà:

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Ora il formattatore produrrà XML simile al seguente:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Se si desidera evitare attributi nella classe di modello, è disponibile un'altra opzione: creare una nuova specifica del tipo **DataContractSerializer** istanza e impostare *preserveObjectReferences* a **true**  nel costruttore. Quindi è possibile impostare questa istanza come un serializzatore per tipo sul formattatore di media type XML. Il codice seguente viene illustrato come eseguire questa operazione:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Serializzazione degli oggetti di test

Quando si progetta l'API web, è utile verificare la modalità di serializzazione di oggetti dati. È possibile farlo senza la creazione di un controller o di richiamare un'azione del controller.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
