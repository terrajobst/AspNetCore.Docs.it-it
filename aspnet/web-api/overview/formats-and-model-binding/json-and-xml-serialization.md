---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: JSON e serializzazione XML in ASP.NET Web API | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2012
ms.topic: article
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: b1fcaf70cc38d73da0a454764520197b97f34b26
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
ms.locfileid: "28038101"
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="7cd67-102">La serializzazione XML in ASP.NET Web API e JSON</span><span class="sxs-lookup"><span data-stu-id="7cd67-102">JSON and XML Serialization in ASP.NET Web API</span></span>
====================
<span data-ttu-id="7cd67-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7cd67-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="7cd67-104">Questo articolo vengono descritti i formattatori JSON e XML in ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="7cd67-104">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="7cd67-105">In ASP.NET Web API, un *formattatore di media type* è un oggetto che può essere:</span><span class="sxs-lookup"><span data-stu-id="7cd67-105">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="7cd67-106">Corpo del messaggio agli oggetti CLR di lettura da un HTTP</span><span class="sxs-lookup"><span data-stu-id="7cd67-106">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="7cd67-107">Scrittura di oggetti CLR in un corpo del messaggio HTTP</span><span class="sxs-lookup"><span data-stu-id="7cd67-107">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="7cd67-108">API Web fornisce formattatori di media type per JSON e XML.</span><span class="sxs-lookup"><span data-stu-id="7cd67-108">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="7cd67-109">Per impostazione predefinita, il framework inserisce questi formattatori nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="7cd67-109">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="7cd67-110">I client possono richiedere JSON o XML nell'intestazione Accept della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="7cd67-110">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="7cd67-111">Sommario</span><span class="sxs-lookup"><span data-stu-id="7cd67-111">Contents</span></span>

- [<span data-ttu-id="7cd67-112">Formattatore di Media Type JSON</span><span class="sxs-lookup"><span data-stu-id="7cd67-112">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="7cd67-113">Proprietà di sola lettura</span><span class="sxs-lookup"><span data-stu-id="7cd67-113">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="7cd67-114">Date</span><span class="sxs-lookup"><span data-stu-id="7cd67-114">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="7cd67-115">Stili rientri</span><span class="sxs-lookup"><span data-stu-id="7cd67-115">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="7cd67-116">Maiuscole e minuscole camel</span><span class="sxs-lookup"><span data-stu-id="7cd67-116">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="7cd67-117">Oggetti di tipo anonimi e con tipizzazione debole</span><span class="sxs-lookup"><span data-stu-id="7cd67-117">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="7cd67-118">Formattatore di Media Type XML</span><span class="sxs-lookup"><span data-stu-id="7cd67-118">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="7cd67-119">Proprietà di sola lettura</span><span class="sxs-lookup"><span data-stu-id="7cd67-119">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="7cd67-120">Date</span><span class="sxs-lookup"><span data-stu-id="7cd67-120">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="7cd67-121">Stili rientri</span><span class="sxs-lookup"><span data-stu-id="7cd67-121">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="7cd67-122">Serializzatori XML per ogni tipo di impostazione</span><span class="sxs-lookup"><span data-stu-id="7cd67-122">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="7cd67-123">Rimozione di JSON o un formattatore XML</span><span class="sxs-lookup"><span data-stu-id="7cd67-123">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="7cd67-124">La gestione di riferimenti circolari agli oggetti</span><span class="sxs-lookup"><span data-stu-id="7cd67-124">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="7cd67-125">Serializzazione degli oggetti di test</span><span class="sxs-lookup"><span data-stu-id="7cd67-125">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="7cd67-126">Formattatore di Media Type JSON</span><span class="sxs-lookup"><span data-stu-id="7cd67-126">JSON Media-Type Formatter</span></span>

<span data-ttu-id="7cd67-127">Formattazione JSON viene eseguita il **JsonMediaTypeFormatter** classe.</span><span class="sxs-lookup"><span data-stu-id="7cd67-127">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="7cd67-128">Per impostazione predefinita, **JsonMediaTypeFormatter** utilizza il [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) libreria per eseguire la serializzazione.</span><span class="sxs-lookup"><span data-stu-id="7cd67-128">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="7cd67-129">Json.NET è un progetto di open source di terze parti.</span><span class="sxs-lookup"><span data-stu-id="7cd67-129">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="7cd67-130">Se si preferisce, è possibile configurare il **JsonMediaTypeFormatter** classe da utilizzare il **DataContractJsonSerializer** anziché Json.NET.</span><span class="sxs-lookup"><span data-stu-id="7cd67-130">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="7cd67-131">A tale scopo, impostare il **UseDataContractJsonSerializer** proprietà **true**:</span><span class="sxs-lookup"><span data-stu-id="7cd67-131">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="7cd67-132">Serializzazione JSON</span><span class="sxs-lookup"><span data-stu-id="7cd67-132">JSON Serialization</span></span>

<span data-ttu-id="7cd67-133">In questa sezione descrive alcuni comportamenti specifici del formattatore JSON, utilizzando il valore predefinito [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializzatore.</span><span class="sxs-lookup"><span data-stu-id="7cd67-133">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="7cd67-134">Questo non deve essere una descrizione dettagliata della libreria di Json.NET. Per ulteriori informazioni, vedere il [Json.NET documentazione](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="7cd67-134">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="7cd67-135">Elementi serializzati?</span><span class="sxs-lookup"><span data-stu-id="7cd67-135">What Gets Serialized?</span></span>

<span data-ttu-id="7cd67-136">Per impostazione predefinita, tutti i campi e le proprietà pubbliche sono inclusi nel file JSON serializzato.</span><span class="sxs-lookup"><span data-stu-id="7cd67-136">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="7cd67-137">Per omettere una proprietà o un campo, decorarla con il **JsonIgnore** attributo.</span><span class="sxs-lookup"><span data-stu-id="7cd67-137">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="7cd67-138">Se si preferisce un &quot;registrarsi&quot; approccio, contrassegnare la classe con il **DataContract** attributo.</span><span class="sxs-lookup"><span data-stu-id="7cd67-138">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="7cd67-139">Se questo attributo è presente, i membri vengono ignorati a meno che non hanno il **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="7cd67-139">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="7cd67-140">È inoltre possibile utilizzare **DataMember** serializzare membri privati.</span><span class="sxs-lookup"><span data-stu-id="7cd67-140">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="7cd67-141">Proprietà di sola lettura</span><span class="sxs-lookup"><span data-stu-id="7cd67-141">Read-Only Properties</span></span>

<span data-ttu-id="7cd67-142">Proprietà di sola lettura vengono serializzate per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="7cd67-142">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="7cd67-143">date</span><span class="sxs-lookup"><span data-stu-id="7cd67-143">Dates</span></span>

<span data-ttu-id="7cd67-144">Per impostazione predefinita, Json.NET scrive le date [ISO 8601](http://www.w3.org/TR/NOTE-datetime) formato.</span><span class="sxs-lookup"><span data-stu-id="7cd67-144">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="7cd67-145">Le date in formato UTC (Coordinated Universal Time) vengono scritti con un suffisso "Z".</span><span class="sxs-lookup"><span data-stu-id="7cd67-145">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="7cd67-146">Data nell'ora locale include una differenza di fuso orario.</span><span class="sxs-lookup"><span data-stu-id="7cd67-146">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="7cd67-147">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="7cd67-147">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="7cd67-148">Per impostazione predefinita, Json.NET mantiene il fuso orario.</span><span class="sxs-lookup"><span data-stu-id="7cd67-148">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="7cd67-149">È possibile ignorare questa impostando la proprietà DateTimeZoneHandling:</span><span class="sxs-lookup"><span data-stu-id="7cd67-149">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="7cd67-150">Se si preferisce utilizzare [formato JSON Microsoft](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) anziché ISO 8601, impostare il **DateFormatHandling** proprietà impostazioni del serializzatore:</span><span class="sxs-lookup"><span data-stu-id="7cd67-150">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="7cd67-151">Rientri</span><span class="sxs-lookup"><span data-stu-id="7cd67-151">Indenting</span></span>

<span data-ttu-id="7cd67-152">Per scrivere rientrato JSON, impostare il **formattazione** impostando su **Indented**:</span><span class="sxs-lookup"><span data-stu-id="7cd67-152">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="7cd67-153">Maiuscole e minuscole camel</span><span class="sxs-lookup"><span data-stu-id="7cd67-153">Camel Casing</span></span>

<span data-ttu-id="7cd67-154">Per scrivere i nomi delle proprietà JSON con maiuscole e minuscole camel, senza modificare il modello di dati, impostare il **CamelCasePropertyNamesContractResolver** sul serializzatore:</span><span class="sxs-lookup"><span data-stu-id="7cd67-154">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="7cd67-155">Oggetti di tipo anonimi e con tipizzazione debole</span><span class="sxs-lookup"><span data-stu-id="7cd67-155">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="7cd67-156">Un metodo di azione può restituire un oggetto anonimo ed eseguirne la serializzazione in JSON.</span><span class="sxs-lookup"><span data-stu-id="7cd67-156">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="7cd67-157">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="7cd67-157">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="7cd67-158">Il corpo del messaggio di risposta conterrà il codice JSON seguente:</span><span class="sxs-lookup"><span data-stu-id="7cd67-158">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="7cd67-159">Se il sito web API riceve regime di controllo libero strutturati oggetti JSON dai client, è possibile deserializzare il corpo della richiesta per un **Newtonsoft.Json.Linq.JObject** tipo.</span><span class="sxs-lookup"><span data-stu-id="7cd67-159">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="7cd67-160">Tuttavia, è preferibile utilizzare gli oggetti dati fortemente tipizzati.</span><span class="sxs-lookup"><span data-stu-id="7cd67-160">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="7cd67-161">Quindi non è necessario analizzare i dati manualmente, e si ottengono i vantaggi di convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="7cd67-161">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="7cd67-162">Il serializzatore XML non supporta i tipi anonimi o **JObject** istanze.</span><span class="sxs-lookup"><span data-stu-id="7cd67-162">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="7cd67-163">Se si utilizzano queste funzionalità per i dati JSON, è necessario rimuovere il formattatore XML dalla pipeline, come descritto più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="7cd67-163">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="7cd67-164">Formattatore di Media Type XML</span><span class="sxs-lookup"><span data-stu-id="7cd67-164">XML Media-Type Formatter</span></span>

<span data-ttu-id="7cd67-165">Formattazione XML viene eseguita il **XmlMediaTypeFormatter** classe.</span><span class="sxs-lookup"><span data-stu-id="7cd67-165">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="7cd67-166">Per impostazione predefinita, **XmlMediaTypeFormatter** utilizza il **DataContractSerializer** classe per eseguire la serializzazione.</span><span class="sxs-lookup"><span data-stu-id="7cd67-166">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="7cd67-167">Se si preferisce, è possibile configurare il **XmlMediaTypeFormatter** per utilizzare il **XmlSerializer** anziché il **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="7cd67-167">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="7cd67-168">A tale scopo, impostare il **/usexmlserializer** proprietà **true**:</span><span class="sxs-lookup"><span data-stu-id="7cd67-168">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="7cd67-169">Il **XmlSerializer** classe supporta un set di tipi più ristretto **DataContractSerializer**, ma offre maggiore controllo sul codice XML risultante.</span><span class="sxs-lookup"><span data-stu-id="7cd67-169">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="7cd67-170">È consigliabile utilizzare **XmlSerializer** se è necessario associare uno schema XML esistente.</span><span class="sxs-lookup"><span data-stu-id="7cd67-170">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="7cd67-171">Serializzazione XML</span><span class="sxs-lookup"><span data-stu-id="7cd67-171">XML Serialization</span></span>

<span data-ttu-id="7cd67-172">In questa sezione descrive alcuni comportamenti specifici del formattatore XML, utilizzando il valore predefinito **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="7cd67-172">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="7cd67-173">Per impostazione predefinita, DataContractSerializer si comporta come segue:</span><span class="sxs-lookup"><span data-stu-id="7cd67-173">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="7cd67-174">Tutti i campi e le proprietà di lettura/scrittura pubblici vengono serializzati.</span><span class="sxs-lookup"><span data-stu-id="7cd67-174">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="7cd67-175">Per omettere una proprietà o un campo, decorarla con il **IgnoreDataMember** attributo.</span><span class="sxs-lookup"><span data-stu-id="7cd67-175">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="7cd67-176">Membri privati e protetti non vengono serializzati.</span><span class="sxs-lookup"><span data-stu-id="7cd67-176">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="7cd67-177">Proprietà di sola lettura non vengono serializzate.</span><span class="sxs-lookup"><span data-stu-id="7cd67-177">Read-only properties are not serialized.</span></span> <span data-ttu-id="7cd67-178">(Tuttavia, il contenuto di una proprietà di raccolta di sola lettura viene serializzato.)</span><span class="sxs-lookup"><span data-stu-id="7cd67-178">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="7cd67-179">Nomi di classe e membro vengono scritti nel file XML, esattamente come appaiono nella dichiarazione di classe.</span><span class="sxs-lookup"><span data-stu-id="7cd67-179">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="7cd67-180">Viene utilizzato uno spazio dei nomi XML predefinito.</span><span class="sxs-lookup"><span data-stu-id="7cd67-180">A default XML namespace is used.</span></span>

<span data-ttu-id="7cd67-181">Se è necessario un maggiore controllo sulla serializzazione, è possibile contrassegnare la classe con il **DataContract** attributo.</span><span class="sxs-lookup"><span data-stu-id="7cd67-181">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="7cd67-182">Quando questo attributo è presente, la classe viene serializzata nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="7cd67-182">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="7cd67-183">&quot;Consente di partecipare&quot; approccio: non vengono serializzati i campi e proprietà per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="7cd67-183">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="7cd67-184">Per serializzare una proprietà o un campo, decorarla con il **DataMember** attributo.</span><span class="sxs-lookup"><span data-stu-id="7cd67-184">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="7cd67-185">Per serializzare un membro privato o protetto, decorarla con il **DataMember** attributo.</span><span class="sxs-lookup"><span data-stu-id="7cd67-185">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="7cd67-186">Proprietà di sola lettura non vengono serializzate.</span><span class="sxs-lookup"><span data-stu-id="7cd67-186">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="7cd67-187">Per modificare la modalità con cui il nome della classe visualizzato nel documento XML, impostare il *nome* parametro il **DataContract** attributo.</span><span class="sxs-lookup"><span data-stu-id="7cd67-187">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="7cd67-188">Per modificare il nome di un membro visualizzato nel documento XML, impostare il *nome* parametro il **DataMember** attributo.</span><span class="sxs-lookup"><span data-stu-id="7cd67-188">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="7cd67-189">Per modificare lo spazio dei nomi XML, impostare il *Namespace* parametro il **DataContract** classe.</span><span class="sxs-lookup"><span data-stu-id="7cd67-189">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="7cd67-190">Proprietà di sola lettura</span><span class="sxs-lookup"><span data-stu-id="7cd67-190">Read-Only Properties</span></span>

<span data-ttu-id="7cd67-191">Proprietà di sola lettura non vengono serializzate.</span><span class="sxs-lookup"><span data-stu-id="7cd67-191">Read-only properties are not serialized.</span></span> <span data-ttu-id="7cd67-192">Se una proprietà di sola lettura ha un campo privato sottostante, è possibile contrassegnare il campo privato con le **DataMember** attributo.</span><span class="sxs-lookup"><span data-stu-id="7cd67-192">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="7cd67-193">Questo approccio richiede il **DataContract** attributo della classe.</span><span class="sxs-lookup"><span data-stu-id="7cd67-193">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="7cd67-194">date</span><span class="sxs-lookup"><span data-stu-id="7cd67-194">Dates</span></span>

<span data-ttu-id="7cd67-195">Le date vengono scritti in formato ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="7cd67-195">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="7cd67-196">Ad esempio, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="7cd67-196">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="7cd67-197">Rientri</span><span class="sxs-lookup"><span data-stu-id="7cd67-197">Indenting</span></span>

<span data-ttu-id="7cd67-198">Per scrivere codice XML rientrato, impostare il **rientro** proprietà **true**:</span><span class="sxs-lookup"><span data-stu-id="7cd67-198">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="7cd67-199">Serializzatori XML per ogni tipo di impostazione</span><span class="sxs-lookup"><span data-stu-id="7cd67-199">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="7cd67-200">È possibile impostare diversi serializzatori XML per tipi CLR diversi.</span><span class="sxs-lookup"><span data-stu-id="7cd67-200">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="7cd67-201">Ad esempio, potrebbe essere un oggetto dati specifico che richiede **XmlSerializer** per garantire la compatibilità con le versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="7cd67-201">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="7cd67-202">È possibile utilizzare **XmlSerializer** per questo oggetto e continuare a usare **DataContractSerializer** per altri tipi.</span><span class="sxs-lookup"><span data-stu-id="7cd67-202">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="7cd67-203">Per impostare un serializzatore XML per un determinato tipo, chiamare **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="7cd67-203">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="7cd67-204">È possibile specificare un **XmlSerializer** o qualsiasi oggetto che deriva da **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="7cd67-204">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="7cd67-205">Rimozione di JSON o un formattatore XML</span><span class="sxs-lookup"><span data-stu-id="7cd67-205">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="7cd67-206">È possibile rimuovere il formattatore JSON o il formattatore XML dall'elenco di formattatori, se non si desidera utilizzarli.</span><span class="sxs-lookup"><span data-stu-id="7cd67-206">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="7cd67-207">I motivi principali per eseguire questa operazione sono:</span><span class="sxs-lookup"><span data-stu-id="7cd67-207">The main reasons to do this are:</span></span>

- <span data-ttu-id="7cd67-208">Per limitare le risposte di API web per un determinato tipo di supporto.</span><span class="sxs-lookup"><span data-stu-id="7cd67-208">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="7cd67-209">Ad esempio, si potrebbe decidere di supporta solo le risposte JSON e rimuovere il formattatore XML.</span><span class="sxs-lookup"><span data-stu-id="7cd67-209">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="7cd67-210">Per sostituire il formattatore predefinito con un formattatore personalizzato.</span><span class="sxs-lookup"><span data-stu-id="7cd67-210">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="7cd67-211">Ad esempio, è possibile sostituire il formattatore JSON con l'implementazione personalizzata di un formattatore JSON.</span><span class="sxs-lookup"><span data-stu-id="7cd67-211">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="7cd67-212">Il codice seguente viene illustrato come rimuovere i formattatori predefinita.</span><span class="sxs-lookup"><span data-stu-id="7cd67-212">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="7cd67-213">Chiamare questo metodo dal **applicazione\_avviare** metodo, definito in Global. asax.</span><span class="sxs-lookup"><span data-stu-id="7cd67-213">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="7cd67-214">La gestione di riferimenti circolari agli oggetti</span><span class="sxs-lookup"><span data-stu-id="7cd67-214">Handling Circular Object References</span></span>

<span data-ttu-id="7cd67-215">Per impostazione predefinita, i formattatori JSON e XML scrivono tutti gli oggetti come valori.</span><span class="sxs-lookup"><span data-stu-id="7cd67-215">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="7cd67-216">Se due proprietà che fanno riferimento allo stesso oggetto oppure se l'oggetto stesso viene visualizzato due volte in una raccolta, il formattatore verrà serializzato l'oggetto due volte.</span><span class="sxs-lookup"><span data-stu-id="7cd67-216">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="7cd67-217">In questo modo un particolare problema se l'oggetto grafico contiene cicli, il serializzatore genererà un'eccezione quando viene rilevato un ciclo nel grafico.</span><span class="sxs-lookup"><span data-stu-id="7cd67-217">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="7cd67-218">Considerare i seguenti modelli a oggetti e i controller.</span><span class="sxs-lookup"><span data-stu-id="7cd67-218">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="7cd67-219">Richiamo di questa azione genererà il formattatore da generata un'eccezione che viene convertita in un stato codice 500 (errore Server interno) di risposta al client.</span><span class="sxs-lookup"><span data-stu-id="7cd67-219">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="7cd67-220">Per mantenere i riferimenti agli oggetti in JSON, aggiungere il seguente codice al **applicazione\_avviare** metodo nel file Global. asax:</span><span class="sxs-lookup"><span data-stu-id="7cd67-220">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="7cd67-221">Ora azione del controller restituirà JSON che è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="7cd67-221">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="7cd67-222">Si noti che il serializzatore aggiunge un &quot;$id&quot; proprietà sia agli oggetti.</span><span class="sxs-lookup"><span data-stu-id="7cd67-222">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="7cd67-223">Inoltre, viene rilevato che la proprietà Employee.Department crea un ciclo, in modo che sostituisce il valore con un riferimento all'oggetto: {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="7cd67-223">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="7cd67-224">Riferimenti a oggetti non sono standard in JSON.</span><span class="sxs-lookup"><span data-stu-id="7cd67-224">Object references are not standard in JSON.</span></span> <span data-ttu-id="7cd67-225">Prima di utilizzare questa funzionalità, considerare se i client saranno in grado di analizzare i risultati.</span><span class="sxs-lookup"><span data-stu-id="7cd67-225">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="7cd67-226">Potrebbe essere meglio rimuovere cicli dal grafico.</span><span class="sxs-lookup"><span data-stu-id="7cd67-226">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="7cd67-227">Ad esempio, il collegamento da Employee al reparto non è realmente necessaria in questo esempio.</span><span class="sxs-lookup"><span data-stu-id="7cd67-227">For example, the link from Employee back to Department is not really needed in this example.</span></span>


<span data-ttu-id="7cd67-228">Per mantenere i riferimenti agli oggetti in XML, sono disponibili due opzioni.</span><span class="sxs-lookup"><span data-stu-id="7cd67-228">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="7cd67-229">L'opzione più semplice consiste nell'aggiungere `[DataContract(IsReference=true)]` alla classe del modello.</span><span class="sxs-lookup"><span data-stu-id="7cd67-229">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="7cd67-230">Il *IsReference* parametro consente di riferimenti a oggetti.</span><span class="sxs-lookup"><span data-stu-id="7cd67-230">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="7cd67-231">Tenere presente che **DataContract** rende serializzazione acconsentire esplicitamente, pertanto è necessario aggiungere **DataMember** gli attributi per le proprietà:</span><span class="sxs-lookup"><span data-stu-id="7cd67-231">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="7cd67-232">Ora il formattatore produrrà XML simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="7cd67-232">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="7cd67-233">Se si desidera evitare attributi nella classe di modello, è disponibile un'altra opzione: creare una nuova specifica del tipo **DataContractSerializer** istanza e impostare *preserveObjectReferences* a **true**  nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="7cd67-233">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="7cd67-234">Quindi è possibile impostare questa istanza come un serializzatore per tipo sul formattatore di media type XML.</span><span class="sxs-lookup"><span data-stu-id="7cd67-234">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="7cd67-235">Il codice seguente viene illustrato come eseguire questa operazione:</span><span class="sxs-lookup"><span data-stu-id="7cd67-235">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="7cd67-236">Serializzazione degli oggetti di test</span><span class="sxs-lookup"><span data-stu-id="7cd67-236">Testing Object Serialization</span></span>

<span data-ttu-id="7cd67-237">Quando si progetta l'API web, è utile verificare la modalità di serializzazione di oggetti dati.</span><span class="sxs-lookup"><span data-stu-id="7cd67-237">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="7cd67-238">È possibile farlo senza la creazione di un controller o di richiamare un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="7cd67-238">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
