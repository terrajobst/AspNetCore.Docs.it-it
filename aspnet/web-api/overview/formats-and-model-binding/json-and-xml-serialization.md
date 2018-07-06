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
<a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="ca559-102">Serializzazione JSON e XML nell'API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ca559-102">JSON and XML Serialization in ASP.NET Web API</span></span>
====================
<span data-ttu-id="ca559-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ca559-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ca559-104">Questo articolo descrive i formattatori XML e JSON nell'API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ca559-104">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="ca559-105">Nell'API Web ASP.NET, un *formattatore di media type* è un oggetto che può essere:</span><span class="sxs-lookup"><span data-stu-id="ca559-105">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="ca559-106">Corpo del messaggio di oggetti CLR di lettura da HTTP</span><span class="sxs-lookup"><span data-stu-id="ca559-106">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="ca559-107">Scrivere gli oggetti CLR in un corpo del messaggio HTTP</span><span class="sxs-lookup"><span data-stu-id="ca559-107">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="ca559-108">API Web fornisce formattatori di media type per JSON e XML.</span><span class="sxs-lookup"><span data-stu-id="ca559-108">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="ca559-109">Per impostazione predefinita, il framework inserisce i formattatori nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="ca559-109">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="ca559-110">I client possono richiedere JSON o XML nell'intestazione Accept della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="ca559-110">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="ca559-111">Sommario</span><span class="sxs-lookup"><span data-stu-id="ca559-111">Contents</span></span>

- [<span data-ttu-id="ca559-112">Formattatore di Media Type JSON</span><span class="sxs-lookup"><span data-stu-id="ca559-112">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="ca559-113">Proprietà di sola lettura</span><span class="sxs-lookup"><span data-stu-id="ca559-113">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="ca559-114">Date</span><span class="sxs-lookup"><span data-stu-id="ca559-114">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="ca559-115">Rientro</span><span class="sxs-lookup"><span data-stu-id="ca559-115">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="ca559-116">Convenzione camel</span><span class="sxs-lookup"><span data-stu-id="ca559-116">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="ca559-117">Oggetti anonimi e con tipizzazione debole</span><span class="sxs-lookup"><span data-stu-id="ca559-117">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="ca559-118">Formattatore di Media Type XML</span><span class="sxs-lookup"><span data-stu-id="ca559-118">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="ca559-119">Proprietà di sola lettura</span><span class="sxs-lookup"><span data-stu-id="ca559-119">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="ca559-120">Date</span><span class="sxs-lookup"><span data-stu-id="ca559-120">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="ca559-121">Rientro</span><span class="sxs-lookup"><span data-stu-id="ca559-121">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="ca559-122">L'impostazione di serializzatori XML Per tipo</span><span class="sxs-lookup"><span data-stu-id="ca559-122">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="ca559-123">Rimozione di JSON o un formattatore XML</span><span class="sxs-lookup"><span data-stu-id="ca559-123">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="ca559-124">La gestione di riferimenti circolari agli oggetti</span><span class="sxs-lookup"><span data-stu-id="ca559-124">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="ca559-125">Serializzazione di un oggetto di test</span><span class="sxs-lookup"><span data-stu-id="ca559-125">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="ca559-126">Formattatore di Media Type JSON</span><span class="sxs-lookup"><span data-stu-id="ca559-126">JSON Media-Type Formatter</span></span>

<span data-ttu-id="ca559-127">Formattazione JSON avviene tramite il **JsonMediaTypeFormatter** classe.</span><span class="sxs-lookup"><span data-stu-id="ca559-127">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="ca559-128">Per impostazione predefinita **JsonMediaTypeFormatter** Usa le [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) libreria per eseguire la serializzazione.</span><span class="sxs-lookup"><span data-stu-id="ca559-128">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="ca559-129">Json.NET è un progetto di open source di terze parti.</span><span class="sxs-lookup"><span data-stu-id="ca559-129">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="ca559-130">Se si preferisce, è possibile configurare il **JsonMediaTypeFormatter** classe da utilizzare il **DataContractJsonSerializer** invece di Json.NET.</span><span class="sxs-lookup"><span data-stu-id="ca559-130">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="ca559-131">A tale scopo, impostare il **UseDataContractJsonSerializer** proprietà **true**:</span><span class="sxs-lookup"><span data-stu-id="ca559-131">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="ca559-132">Serializzazione JSON</span><span class="sxs-lookup"><span data-stu-id="ca559-132">JSON Serialization</span></span>

<span data-ttu-id="ca559-133">In questa sezione descrive alcuni comportamenti specifici del formattatore JSON, usando il valore predefinito [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializzatore.</span><span class="sxs-lookup"><span data-stu-id="ca559-133">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="ca559-134">Questo non è intende essere una documentazione completa della libreria di Json.NET. per altre informazioni, vedere la [documentazione di Json.NET](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="ca559-134">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="ca559-135">Elementi serializzati?</span><span class="sxs-lookup"><span data-stu-id="ca559-135">What Gets Serialized?</span></span>

<span data-ttu-id="ca559-136">Per impostazione predefinita, tutti i campi e proprietà pubbliche sono inclusi nel file JSON serializzato.</span><span class="sxs-lookup"><span data-stu-id="ca559-136">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="ca559-137">Per omettere una proprietà o un campo, decorarla con il **JsonIgnore** attributo.</span><span class="sxs-lookup"><span data-stu-id="ca559-137">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="ca559-138">Se si preferisce un' &quot;acconsenti esplicitamente&quot; approccio, decorare la classe con il **DataContract** attributo.</span><span class="sxs-lookup"><span data-stu-id="ca559-138">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="ca559-139">Se questo attributo è presente, i membri vengono ignorati a meno che non hanno le **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="ca559-139">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="ca559-140">È anche possibile usare **DataMember** serializzare membri privati.</span><span class="sxs-lookup"><span data-stu-id="ca559-140">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="ca559-141">Proprietà di sola lettura</span><span class="sxs-lookup"><span data-stu-id="ca559-141">Read-Only Properties</span></span>

<span data-ttu-id="ca559-142">Le proprietà di sola lettura vengono serializzate per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="ca559-142">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="ca559-143">Date</span><span class="sxs-lookup"><span data-stu-id="ca559-143">Dates</span></span>

<span data-ttu-id="ca559-144">Per impostazione predefinita, Json.NET scrive le date [ISO 8601](http://www.w3.org/TR/NOTE-datetime) formato.</span><span class="sxs-lookup"><span data-stu-id="ca559-144">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="ca559-145">Le date in formato UTC (Coordinated Universal Time) vengono scritti con un suffisso "Z".</span><span class="sxs-lookup"><span data-stu-id="ca559-145">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="ca559-146">Le date nell'ora locale includono una differenza di fuso orario.</span><span class="sxs-lookup"><span data-stu-id="ca559-146">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="ca559-147">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="ca559-147">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="ca559-148">Per impostazione predefinita, Json.NET mantiene il fuso orario.</span><span class="sxs-lookup"><span data-stu-id="ca559-148">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="ca559-149">È possibile eseguire l'override di questo impostando la proprietà DateTimeZoneHandling:</span><span class="sxs-lookup"><span data-stu-id="ca559-149">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="ca559-150">Se si preferisce usare [formato di data JSON Microsoft](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) invece di ISO 8601, impostare il **DateFormatHandling** proprietà le impostazioni del serializzatore:</span><span class="sxs-lookup"><span data-stu-id="ca559-150">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="ca559-151">Rientri</span><span class="sxs-lookup"><span data-stu-id="ca559-151">Indenting</span></span>

<span data-ttu-id="ca559-152">Per scrivere il JSON con rientro, impostare il **formattazione** se si imposta su **Indented**:</span><span class="sxs-lookup"><span data-stu-id="ca559-152">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="ca559-153">Convenzione camel</span><span class="sxs-lookup"><span data-stu-id="ca559-153">Camel Casing</span></span>

<span data-ttu-id="ca559-154">Per scrivere i nomi di proprietà JSON con le iniziali maiuscole e minuscole, senza modificare il modello di dati, impostare il **CamelCasePropertyNamesContractResolver** sul serializzatore:</span><span class="sxs-lookup"><span data-stu-id="ca559-154">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="ca559-155">Oggetti anonimi e con tipizzazione debole</span><span class="sxs-lookup"><span data-stu-id="ca559-155">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="ca559-156">Un metodo di azione può restituire un oggetto anonimo ed eseguirne la serializzazione in JSON.</span><span class="sxs-lookup"><span data-stu-id="ca559-156">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="ca559-157">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="ca559-157">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="ca559-158">Il corpo del messaggio di risposta conterrà il codice JSON seguente:</span><span class="sxs-lookup"><span data-stu-id="ca559-158">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="ca559-159">Se il web API riceve regime di controllo libero strutturati oggetti JSON dai client, è possibile deserializzare il corpo della richiesta a un **Newtonsoft.Json.Linq.JObject** tipo.</span><span class="sxs-lookup"><span data-stu-id="ca559-159">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="ca559-160">Tuttavia, è in genere preferibile usare gli oggetti dati fortemente tipizzati.</span><span class="sxs-lookup"><span data-stu-id="ca559-160">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="ca559-161">Non è necessario analizzare i dati manualmente e si ottengono i vantaggi di convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="ca559-161">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="ca559-162">Il serializzatore XML non supporta i tipi anonimi o **JObject** istanze.</span><span class="sxs-lookup"><span data-stu-id="ca559-162">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="ca559-163">Se si usano queste funzionalità per i dati JSON, è necessario rimuovere il formattatore XML dalla pipeline, come descritto più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="ca559-163">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="ca559-164">Formattatore di Media Type XML</span><span class="sxs-lookup"><span data-stu-id="ca559-164">XML Media-Type Formatter</span></span>

<span data-ttu-id="ca559-165">Formattazione XML viene fornita per il **XmlMediaTypeFormatter** classe.</span><span class="sxs-lookup"><span data-stu-id="ca559-165">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="ca559-166">Per impostazione predefinita **XmlMediaTypeFormatter** Usa le **DataContractSerializer** classe per eseguire la serializzazione.</span><span class="sxs-lookup"><span data-stu-id="ca559-166">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="ca559-167">Se si preferisce, è possibile configurare il **XmlMediaTypeFormatter** da utilizzare il **XmlSerializer** anziché il **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="ca559-167">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="ca559-168">A tale scopo, impostare il **/usexmlserializer** proprietà **true**:</span><span class="sxs-lookup"><span data-stu-id="ca559-168">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="ca559-169">Il **XmlSerializer** classe supporta un set di tipi rispetto ai più ristretto **DataContractSerializer**, ma offre maggiore controllo sul codice XML risultante.</span><span class="sxs-lookup"><span data-stu-id="ca559-169">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="ca559-170">È consigliabile usare **XmlSerializer** se è necessario associare uno schema XML esistente.</span><span class="sxs-lookup"><span data-stu-id="ca559-170">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="ca559-171">Serializzazione XML</span><span class="sxs-lookup"><span data-stu-id="ca559-171">XML Serialization</span></span>

<span data-ttu-id="ca559-172">In questa sezione descrive alcuni comportamenti specifici del formattatore XML, usando il valore predefinito **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="ca559-172">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="ca559-173">Per impostazione predefinita, la classe DataContractSerializer si comporta come segue:</span><span class="sxs-lookup"><span data-stu-id="ca559-173">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="ca559-174">Vengono serializzati tutti i campi e proprietà di lettura/scrittura pubblica.</span><span class="sxs-lookup"><span data-stu-id="ca559-174">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="ca559-175">Per omettere una proprietà o un campo, decorarla con il **IgnoreDataMember** attributo.</span><span class="sxs-lookup"><span data-stu-id="ca559-175">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="ca559-176">Membri privati e protetti non vengono serializzati.</span><span class="sxs-lookup"><span data-stu-id="ca559-176">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="ca559-177">Le proprietà di sola lettura non vengono serializzate.</span><span class="sxs-lookup"><span data-stu-id="ca559-177">Read-only properties are not serialized.</span></span> <span data-ttu-id="ca559-178">(Tuttavia, il contenuto di una proprietà della raccolta di sola lettura viene serializzato.)</span><span class="sxs-lookup"><span data-stu-id="ca559-178">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="ca559-179">Nomi di classe e membro vengono scritti nel file XML, esattamente come appaiono nella dichiarazione di classe.</span><span class="sxs-lookup"><span data-stu-id="ca559-179">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="ca559-180">Viene usato uno spazio dei nomi XML predefinito.</span><span class="sxs-lookup"><span data-stu-id="ca559-180">A default XML namespace is used.</span></span>

<span data-ttu-id="ca559-181">Se è necessario maggiore controllo sulla serializzazione, è possibile decorare la classe con il **DataContract** attributo.</span><span class="sxs-lookup"><span data-stu-id="ca559-181">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="ca559-182">Quando questo attributo è presente, la classe viene serializzata come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="ca559-182">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="ca559-183">&quot;Fornire il consenso esplicito&quot; approccio: proprietà e i campi non vengono serializzati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="ca559-183">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="ca559-184">Per serializzare una proprietà o un campo, decorarla con il **DataMember** attributo.</span><span class="sxs-lookup"><span data-stu-id="ca559-184">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="ca559-185">Per serializzare un membro privato o protetto, decorarla con il **DataMember** attributo.</span><span class="sxs-lookup"><span data-stu-id="ca559-185">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="ca559-186">Le proprietà di sola lettura non vengono serializzate.</span><span class="sxs-lookup"><span data-stu-id="ca559-186">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="ca559-187">Per modificare come viene visualizzato il nome della classe nel codice XML, impostare il *Name* parametro nel **DataContract** attributo.</span><span class="sxs-lookup"><span data-stu-id="ca559-187">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="ca559-188">Per modificare l'aspetto di un nome di membro nel codice XML, impostare il *Name* parametro nel **DataMember** attributo.</span><span class="sxs-lookup"><span data-stu-id="ca559-188">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="ca559-189">Per modificare lo spazio dei nomi XML, impostare il *Namespace* parametro le **DataContract** classe.</span><span class="sxs-lookup"><span data-stu-id="ca559-189">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="ca559-190">Proprietà di sola lettura</span><span class="sxs-lookup"><span data-stu-id="ca559-190">Read-Only Properties</span></span>

<span data-ttu-id="ca559-191">Le proprietà di sola lettura non vengono serializzate.</span><span class="sxs-lookup"><span data-stu-id="ca559-191">Read-only properties are not serialized.</span></span> <span data-ttu-id="ca559-192">Se una proprietà di sola lettura ha un campo privato sottostante, è possibile contrassegnare il campo privato con il **DataMember** attributo.</span><span class="sxs-lookup"><span data-stu-id="ca559-192">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="ca559-193">Questo approccio richiede la **DataContract** attributo della classe.</span><span class="sxs-lookup"><span data-stu-id="ca559-193">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="ca559-194">Date</span><span class="sxs-lookup"><span data-stu-id="ca559-194">Dates</span></span>

<span data-ttu-id="ca559-195">Le date vengono scritte nel formato ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="ca559-195">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="ca559-196">Ad esempio, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="ca559-196">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="ca559-197">Rientri</span><span class="sxs-lookup"><span data-stu-id="ca559-197">Indenting</span></span>

<span data-ttu-id="ca559-198">Per scrivere codice XML rientrato, impostare il **rientro** proprietà **true**:</span><span class="sxs-lookup"><span data-stu-id="ca559-198">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="ca559-199">L'impostazione di serializzatori XML Per tipo</span><span class="sxs-lookup"><span data-stu-id="ca559-199">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="ca559-200">È possibile impostare diversi serializzatori XML per tipi CLR diversi.</span><span class="sxs-lookup"><span data-stu-id="ca559-200">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="ca559-201">Ad esempio, potrebbe essere un oggetto dati specifico che richiede **XmlSerializer** per garantire la compatibilità con le versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="ca559-201">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="ca559-202">È possibile usare **XmlSerializer** per questo oggetto e continuare a usare **DataContractSerializer** per altri tipi.</span><span class="sxs-lookup"><span data-stu-id="ca559-202">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="ca559-203">Per impostare un serializzatore XML per un determinato tipo, chiamare **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="ca559-203">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="ca559-204">È possibile specificare una **XmlSerializer** o qualsiasi oggetto che deriva da **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="ca559-204">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="ca559-205">Rimozione di JSON o un formattatore XML</span><span class="sxs-lookup"><span data-stu-id="ca559-205">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="ca559-206">È possibile rimuovere il formattatore JSON o il formattatore XML dall'elenco di formattatori, se si preferisce non usarle.</span><span class="sxs-lookup"><span data-stu-id="ca559-206">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="ca559-207">I motivi principali per eseguire questa operazione sono:</span><span class="sxs-lookup"><span data-stu-id="ca559-207">The main reasons to do this are:</span></span>

- <span data-ttu-id="ca559-208">Per limitare le risposte di API web a un tipo di supporti particolare.</span><span class="sxs-lookup"><span data-stu-id="ca559-208">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="ca559-209">Ad esempio, si potrebbe decidere di supportano solo le risposte JSON e rimuovere il formattatore XML.</span><span class="sxs-lookup"><span data-stu-id="ca559-209">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="ca559-210">Per sostituire il formattatore predefinito con un formattatore personalizzato.</span><span class="sxs-lookup"><span data-stu-id="ca559-210">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="ca559-211">Ad esempio, è Impossibile sostituire il formattatore JSON con la propria implementazione personalizzata di un formattatore JSON.</span><span class="sxs-lookup"><span data-stu-id="ca559-211">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="ca559-212">Il codice seguente viene illustrato come rimuovere i formattatori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="ca559-212">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="ca559-213">Chiamarla dalle **Application\_avviare** metodo, definito in Global. asax.</span><span class="sxs-lookup"><span data-stu-id="ca559-213">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="ca559-214">La gestione di riferimenti circolari agli oggetti</span><span class="sxs-lookup"><span data-stu-id="ca559-214">Handling Circular Object References</span></span>

<span data-ttu-id="ca559-215">Per impostazione predefinita, i formattatori # UID JSON e XML scrivono tutti gli oggetti come valori.</span><span class="sxs-lookup"><span data-stu-id="ca559-215">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="ca559-216">Se due proprietà si riferiscono allo stesso oggetto oppure se l'oggetto stesso viene visualizzato due volte in una raccolta, il formattatore eseguirà la serializzazione dell'oggetto due volte.</span><span class="sxs-lookup"><span data-stu-id="ca559-216">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="ca559-217">Questo è un problema specifico se l'oggetto grafico contiene cicli, in quanto il serializzatore genera un'eccezione quando viene rilevato un ciclo nel grafico.</span><span class="sxs-lookup"><span data-stu-id="ca559-217">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="ca559-218">Considerare i seguenti modelli a oggetti e i controller.</span><span class="sxs-lookup"><span data-stu-id="ca559-218">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="ca559-219">Richiama l'azione causerà il formattatore da generata un'eccezione, si traduce in un stato codice 500 (errore Server interno) di risposta al client.</span><span class="sxs-lookup"><span data-stu-id="ca559-219">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="ca559-220">Per conservare i riferimenti agli oggetti in JSON, aggiungere il codice seguente al **Application\_avviare** metodo nel file Global. asax:</span><span class="sxs-lookup"><span data-stu-id="ca559-220">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="ca559-221">A questo punto l'azione del controller restituirà JSON simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="ca559-221">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="ca559-222">Si noti che il serializzatore aggiunge un' &quot;$id&quot; proprietà per entrambi gli oggetti.</span><span class="sxs-lookup"><span data-stu-id="ca559-222">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="ca559-223">Inoltre, rileva che la proprietà Employee.Department crea un ciclo, in modo che sostituisce il valore con un riferimento all'oggetto: {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="ca559-223">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="ca559-224">Riferimenti a oggetti non sono standard in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="ca559-224">Object references are not standard in JSON.</span></span> <span data-ttu-id="ca559-225">Prima di usare questa funzionalità, valutare se i client saranno in grado di analizzare i risultati.</span><span class="sxs-lookup"><span data-stu-id="ca559-225">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="ca559-226">Potrebbe essere preferibile semplicemente rimuovere cicli dal grafico.</span><span class="sxs-lookup"><span data-stu-id="ca559-226">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="ca559-227">Ad esempio, il collegamento da Employee al reparto non è realmente necessaria in questo esempio.</span><span class="sxs-lookup"><span data-stu-id="ca559-227">For example, the link from Employee back to Department is not really needed in this example.</span></span>


<span data-ttu-id="ca559-228">Per conservare i riferimenti agli oggetti in XML, sono disponibili due opzioni.</span><span class="sxs-lookup"><span data-stu-id="ca559-228">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="ca559-229">L'opzione più semplice consiste nell'aggiungere `[DataContract(IsReference=true)]` alla classe del modello.</span><span class="sxs-lookup"><span data-stu-id="ca559-229">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="ca559-230">Il *IsReference* parametro consente riferimenti a oggetti.</span><span class="sxs-lookup"><span data-stu-id="ca559-230">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="ca559-231">Tenere presente che **DataContract** rende serializzazione acconsenti esplicitamente, pertanto è necessario aggiungere **DataMember** attributi alle proprietà:</span><span class="sxs-lookup"><span data-stu-id="ca559-231">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="ca559-232">A questo punto il formattatore produrrà XML simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="ca559-232">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="ca559-233">Se si desidera evitare attributi nella classe di modello, è disponibile un'altra opzione: creare una nuova specifica del tipo **DataContractSerializer** dell'istanza e impostare *preserveObjectReferences* a **true**  nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="ca559-233">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="ca559-234">Impostare quindi questa istanza come un serializzatore per tipo nel formattatore di media type XML.</span><span class="sxs-lookup"><span data-stu-id="ca559-234">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="ca559-235">Il codice seguente mostra come eseguire questa operazione:</span><span class="sxs-lookup"><span data-stu-id="ca559-235">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="ca559-236">Serializzazione di un oggetto di test</span><span class="sxs-lookup"><span data-stu-id="ca559-236">Testing Object Serialization</span></span>

<span data-ttu-id="ca559-237">Quando si progetta l'API web, è utile testare la modalità di serializzazione di oggetti dati.</span><span class="sxs-lookup"><span data-stu-id="ca559-237">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="ca559-238">È possibile farlo senza la creazione di un controller o richiamare un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="ca559-238">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
