---
title: Formattatori personalizzati nell'API web ASP.NET MVC di base
author: tdykstra
description: Informazioni su come creare e utilizzare i formattatori personalizzati per il web API in ASP.NET Core.
ms.author: tdykstra
manager: wpickett
ms.date: 02/08/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/custom-formatters
ms.openlocfilehash: 3a6474fdae29b170978226de74d523b20a16cd0c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="custom-formatters-in-aspnet-core-mvc-web-apis"></a><span data-ttu-id="0c8c0-103">Formattatori personalizzati nell'API web ASP.NET MVC di base</span><span class="sxs-lookup"><span data-stu-id="0c8c0-103">Custom formatters in ASP.NET Core MVC web APIs</span></span>

<span data-ttu-id="0c8c0-104">Da [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0c8c0-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="0c8c0-105">ASP.NET MVC di base dispone di supporto integrato per lo scambio di dati nell'API web usando i formati di JSON, XML o testo normale.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-105">ASP.NET Core MVC has built-in support for data exchange in web APIs by using JSON, XML, or plain text formats.</span></span> <span data-ttu-id="0c8c0-106">In questo articolo viene illustrato come aggiungere il supporto per formati aggiuntivi tramite la creazione di formattatori personalizzati.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-106">This article shows how to add support for additional formats by creating custom formatters.</span></span>

<span data-ttu-id="0c8c0-107">[Consente di visualizzare o scaricare l'esempio da GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).</span><span class="sxs-lookup"><span data-stu-id="0c8c0-107">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).</span></span>

## <a name="when-to-use-custom-formatters"></a><span data-ttu-id="0c8c0-108">Quando utilizzare formattatori personalizzati</span><span class="sxs-lookup"><span data-stu-id="0c8c0-108">When to use custom formatters</span></span>

<span data-ttu-id="0c8c0-109">Utilizzare un formattatore personalizzato quando si desidera che il [negoziazione del contenuto](xref:mvc/models/formatting) processo per supportare un tipo di contenuto che non è supportato dai formattatori incorporati (JSON, XML e testo normale).</span><span class="sxs-lookup"><span data-stu-id="0c8c0-109">Use a custom formatter when you want the [content negotiation](xref:mvc/models/formatting) process to support a content type that isn't supported by the built-in formatters (JSON, XML, and plain text).</span></span>

<span data-ttu-id="0c8c0-110">Ad esempio, se alcuni dei client per l'API web in grado di gestire il [Protobuf](https://github.com/google/protobuf) formato, si potrebbe voler usare Protobuf con i client perché risulta più efficiente.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-110">For example, if some of the clients for your web API can handle the [Protobuf](https://github.com/google/protobuf) format, you might want to use Protobuf with those clients because it's more efficient.</span></span>  <span data-ttu-id="0c8c0-111">È necessario che l'API web per inviare i nomi dei contatti e indirizzi in [vCard](https://wikipedia.org/wiki/VCard) formato, un formato di uso comune per lo scambio di dati di contatto.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-111">Or you might want your web API to send contact names and addresses in [vCard](https://wikipedia.org/wiki/VCard) format, a commonly used format for exchanging contact data.</span></span> <span data-ttu-id="0c8c0-112">L'app di esempio fornito in questo articolo implementa un formattatore vCard semplice.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-112">The sample app provided with this article implements a simple vCard formatter.</span></span>

## <a name="overview-of-how-to-use-a-custom-formatter"></a><span data-ttu-id="0c8c0-113">Panoramica di come utilizzare un formattatore personalizzato</span><span class="sxs-lookup"><span data-stu-id="0c8c0-113">Overview of how to use a custom formatter</span></span>

<span data-ttu-id="0c8c0-114">Ecco i passaggi per creare e utilizzare un formattatore personalizzato:</span><span class="sxs-lookup"><span data-stu-id="0c8c0-114">Here are the steps to create and use a custom formatter:</span></span>

* <span data-ttu-id="0c8c0-115">Se si desidera serializzare i dati da inviare al client, creare una classe di output del formattatore.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-115">Create an output formatter class if you want to serialize data to send to the client.</span></span>
* <span data-ttu-id="0c8c0-116">Se si desidera deserializzare i dati ricevuti dal client, creare una classe formattatore di input.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-116">Create an input formatter class if you want to deserialize data received from the client.</span></span> 
* <span data-ttu-id="0c8c0-117">Aggiungere istanze dei formattatori dal `InputFormatters` e `OutputFormatters` raccolte in [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span><span class="sxs-lookup"><span data-stu-id="0c8c0-117">Add instances of your formatters to the `InputFormatters` and `OutputFormatters` collections in [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span></span>

<span data-ttu-id="0c8c0-118">Le sezioni seguenti forniscono istruzioni ed esempi di codice per ognuno di questi passaggi.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-118">The following sections provide guidance and code examples for each of these steps.</span></span>

## <a name="how-to-create-a-custom-formatter-class"></a><span data-ttu-id="0c8c0-119">Come creare una classe formattatore personalizzato</span><span class="sxs-lookup"><span data-stu-id="0c8c0-119">How to create a custom formatter class</span></span>

<span data-ttu-id="0c8c0-120">Per creare un formattatore:</span><span class="sxs-lookup"><span data-stu-id="0c8c0-120">To create a formatter:</span></span>

* <span data-ttu-id="0c8c0-121">Derivare la classe dalla classe di base appropriata.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-121">Derive the class from the appropriate base class.</span></span>
* <span data-ttu-id="0c8c0-122">Specificare i tipi di supporto valido e le codifiche nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-122">Specify valid media types and encodings in the constructor.</span></span>
* <span data-ttu-id="0c8c0-123">Eseguire l'override `CanReadType` / `CanWriteType` metodi</span><span class="sxs-lookup"><span data-stu-id="0c8c0-123">Override `CanReadType`/`CanWriteType` methods</span></span>
* <span data-ttu-id="0c8c0-124">Eseguire l'override `ReadRequestBodyAsync` / `WriteResponseBodyAsync` metodi</span><span class="sxs-lookup"><span data-stu-id="0c8c0-124">Override `ReadRequestBodyAsync`/`WriteResponseBodyAsync` methods</span></span>
  
### <a name="derive-from-the-appropriate-base-class"></a><span data-ttu-id="0c8c0-125">Derivare dalla classe di base appropriata</span><span class="sxs-lookup"><span data-stu-id="0c8c0-125">Derive from the appropriate base class</span></span>

<span data-ttu-id="0c8c0-126">Per tipi di file di testo (ad esempio, vCard), derivare la [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) o [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) classe di base.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-126">For text media types (for example, vCard), derive from the [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) or [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) base class.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

<span data-ttu-id="0c8c0-127">Per i tipi binari, derivare la [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) o [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) classe di base.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-127">For binary types, derive from the [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) or [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) base class.</span></span>

### <a name="specify-valid-media-types-and-encodings"></a><span data-ttu-id="0c8c0-128">Specificare le codifiche e tipi di supporto valido</span><span class="sxs-lookup"><span data-stu-id="0c8c0-128">Specify valid media types and encodings</span></span>

<span data-ttu-id="0c8c0-129">Nel costruttore, specificare tipi di supporto valido e codifiche aggiungendo il `SupportedMediaTypes` e `SupportedEncodings` raccolte.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-129">In the constructor, specify valid media types and encodings by adding to the `SupportedMediaTypes` and `SupportedEncodings` collections.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]  
> <span data-ttu-id="0c8c0-130">È possibile eseguire l'inserimento di dipendenze di costruttore in una classe del formattatore.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-130">You can't do constructor dependency injection in a formatter class.</span></span> <span data-ttu-id="0c8c0-131">Ad esempio, è possibile ottenere un logger mediante l'aggiunta di un parametro di logger al costruttore.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-131">For example, you can't get a logger by adding a logger parameter to the constructor.</span></span> <span data-ttu-id="0c8c0-132">Per accedere ai servizi, è necessario utilizzare l'oggetto di contesto passato ai metodi di.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-132">To access services, you have to use the context object that gets passed in to your methods.</span></span> <span data-ttu-id="0c8c0-133">Un esempio di codice [seguito](#read-write) viene illustrato come eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-133">A code example [below](#read-write) shows how to do this.</span></span>

### <a name="override-canreadtypecanwritetype"></a><span data-ttu-id="0c8c0-134">Eseguire l'override CanReadType/CanWriteType</span><span class="sxs-lookup"><span data-stu-id="0c8c0-134">Override CanReadType/CanWriteType</span></span> 

<span data-ttu-id="0c8c0-135">Specificare il tipo è possibile deserializzare in o serializzare da eseguendo l'override di `CanReadType` o `CanWriteType` metodi.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-135">Specify the type you can deserialize into or serialize from by overriding the `CanReadType` or `CanWriteType` methods.</span></span> <span data-ttu-id="0c8c0-136">Ad esempio, potrebbe essere solo in grado di creare testo vCard da un `Contact` tipo e viceversa.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-136">For example, you might only be able to create vCard text from a `Contact` type and vice versa.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a><span data-ttu-id="0c8c0-137">Il metodo CanWriteResult</span><span class="sxs-lookup"><span data-stu-id="0c8c0-137">The CanWriteResult method</span></span>

<span data-ttu-id="0c8c0-138">In alcuni scenari è necessario eseguire l'override `CanWriteResult` anziché `CanWriteType`.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-138">In some scenarios you have to override `CanWriteResult` instead of `CanWriteType`.</span></span> <span data-ttu-id="0c8c0-139">Utilizzare `CanWriteResult` se vengono soddisfatte le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="0c8c0-139">Use `CanWriteResult` if the following conditions are true:</span></span>

  * <span data-ttu-id="0c8c0-140">Il metodo di azione restituisce una classe di modello.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-140">Your action method returns a model class.</span></span>
  * <span data-ttu-id="0c8c0-141">Sono disponibili le classi derivate che potrebbero essere restituite in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-141">There are derived classes which might be returned at runtime.</span></span>
  * <span data-ttu-id="0c8c0-142">È necessario conoscere in fase di esecuzione che derivata a classe è stata restituita dall'azione.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-142">You need to know at runtime which derived class was returned by the action.</span></span>  

<span data-ttu-id="0c8c0-143">Ad esempio, si supponga che la firma del metodo di azione restituisce un `Person` tipo, ma può restituire un `Student` o `Instructor` tipo che deriva da `Person`.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-143">For example, suppose your action method signature returns a `Person` type, but it may return a `Student` or `Instructor` type that derives from `Person`.</span></span> <span data-ttu-id="0c8c0-144">Se si desidera che il formattatore di gestire solo `Student` oggetti, controllare il tipo di [oggetto](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) nell'oggetto di contesto fornito per il `CanWriteResult` metodo.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-144">If you want your formatter to handle only `Student` objects, check the type of [Object](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) in the context object provided to the `CanWriteResult` method.</span></span> <span data-ttu-id="0c8c0-145">Si noti che non è necessario utilizzare `CanWriteResult` quando il metodo di azione restituisce `IActionResult`; in tal caso, il `CanWriteType` metodo riceve il tipo di runtime.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-145">Note that it's not necessary to use `CanWriteResult` when the action method returns `IActionResult`; in that case, the `CanWriteType` method receives the runtime type.</span></span>

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a><span data-ttu-id="0c8c0-146">Override ReadRequestBodyAsync/WriteResponseBodyAsync</span><span class="sxs-lookup"><span data-stu-id="0c8c0-146">Override ReadRequestBodyAsync/WriteResponseBodyAsync</span></span> 

<span data-ttu-id="0c8c0-147">Eseguire il lavoro effettivo del deserializzazione o la serializzazione `ReadRequestBodyAsync` o `WriteResponseBodyAsync`.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-147">You do the actual work of deserializing or serializing in `ReadRequestBodyAsync` or `WriteResponseBodyAsync`.</span></span>  <span data-ttu-id="0c8c0-148">Le righe evidenziate nell'esempio seguente viene illustrato come ottenere servizi dal contenitore dell'inserimento di dipendenza (non è possibile ottenere tali da parametri del costruttore).</span><span class="sxs-lookup"><span data-stu-id="0c8c0-148">The highlighted lines in the following example show how to get services from the dependency injection container (you can't get them from constructor parameters).</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a><span data-ttu-id="0c8c0-149">Come configurare MVC per utilizzare un formattatore personalizzato</span><span class="sxs-lookup"><span data-stu-id="0c8c0-149">How to configure MVC to use a custom formatter</span></span>
 
<span data-ttu-id="0c8c0-150">Per utilizzare un formattatore personalizzato, aggiungere un'istanza della classe del formattatore per la `InputFormatters` o `OutputFormatters` insieme.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-150">To use a custom formatter, add an instance of the formatter class to the `InputFormatters` or `OutputFormatters` collection.</span></span>

[!code-csharp[Main](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

<span data-ttu-id="0c8c0-151">Formattatori vengono valutati nell'ordine che inserirli.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-151">Formatters are evaluated in the order you insert them.</span></span> <span data-ttu-id="0c8c0-152">Il primo ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-152">The first one takes precedence.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="0c8c0-153">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="0c8c0-153">Next steps</span></span>

<span data-ttu-id="0c8c0-154">Vedere il [applicazione di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), che implementa vCard semplice input e output formattatori.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-154">See the [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), which implements simple vCard input and output formatters.</span></span>  <span data-ttu-id="0c8c0-155">L'applicazione legge e scrive vCard che come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="0c8c0-155">The application reads and writes vCards that look like the following example:</span></span>

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

<span data-ttu-id="0c8c0-156">Per visualizzare vCard output, eseguire l'applicazione, inviare una richiesta Get con Accept intestazione "text/vcard" per `http://localhost:63313/api/contacts/` (se eseguito da Visual Studio) o `http://localhost:5000/api/contacts/` (durante l'esecuzione dalla riga di comando).</span><span class="sxs-lookup"><span data-stu-id="0c8c0-156">To see vCard output, run the application and send a Get request with Accept header "text/vcard" to `http://localhost:63313/api/contacts/` (when running from Visual Studio) or `http://localhost:5000/api/contacts/` (when running from the command line).</span></span>

<span data-ttu-id="0c8c0-157">Per aggiungere un vCard alla raccolta in memoria dei contatti, inviare una richiesta Post all'URL stesso, con l'intestazione Content-Type "text/vcard" e con vCard testo nel corpo, formattato come illustrato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="0c8c0-157">To add a vCard to the in-memory collection of contacts, send a Post request to the same URL, with Content-Type header "text/vcard" and with vCard text in the body, formatted like the example above.</span></span>
