---
title: Formattatori personalizzati nell'API web ASP.NET MVC di base
author: tdykstra
description: Informazioni su come creare e utilizzare i formattatori personalizzati per il web API in ASP.NET Core.
keywords: ASP.NET Core, web api, formattatori personalizzati
ms.author: tdykstra
manager: wpickett
ms.date: 02/08/2017
ms.topic: article
ms.assetid: 1fb6fdc2-e199-4469-9012-b909d1913422
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/custom-formatters
ms.openlocfilehash: 792e007232c751d3db9dc5e50adbedfb2bb1a7ae
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="custom-formatters-in-aspnet-core-mvc-web-apis"></a><span data-ttu-id="fbbd4-104">Formattatori personalizzati nell'API web ASP.NET MVC di base</span><span class="sxs-lookup"><span data-stu-id="fbbd4-104">Custom formatters in ASP.NET Core MVC web APIs</span></span>

<span data-ttu-id="fbbd4-105">Da [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="fbbd4-105">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="fbbd4-106">ASP.NET MVC di base dispone di supporto integrato per lo scambio di dati nell'API web usando i formati di JSON, XML o testo normale.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-106">ASP.NET Core MVC has built-in support for data exchange in web APIs by using JSON, XML, or plain text formats.</span></span> <span data-ttu-id="fbbd4-107">In questo articolo viene illustrato come aggiungere il supporto per formati aggiuntivi tramite la creazione di formattatori personalizzati.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-107">This article shows how to add support for additional formats by creating custom formatters.</span></span>

<span data-ttu-id="fbbd4-108">[Consente di visualizzare o scaricare l'esempio da GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).</span><span class="sxs-lookup"><span data-stu-id="fbbd4-108">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).</span></span>

## <a name="when-to-use-custom-formatters"></a><span data-ttu-id="fbbd4-109">Quando utilizzare formattatori personalizzati</span><span class="sxs-lookup"><span data-stu-id="fbbd4-109">When to use custom formatters</span></span>

<span data-ttu-id="fbbd4-110">Utilizzare un formattatore personalizzato quando si desidera che il [negoziazione del contenuto](xref:mvc/models/formatting) processo per supportare un tipo di contenuto che non è supportato dai formattatori incorporati (JSON, XML e testo normale).</span><span class="sxs-lookup"><span data-stu-id="fbbd4-110">Use a custom formatter when you want the [content negotiation](xref:mvc/models/formatting) process to support a content type that isn't supported by the built-in formatters (JSON, XML, and plain text).</span></span>

<span data-ttu-id="fbbd4-111">Ad esempio, se alcuni dei client per l'API web in grado di gestire il [Protobuf](https://github.com/google/protobuf) formato, si potrebbe voler usare Protobuf con i client perché risulta più efficiente.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-111">For example, if some of the clients for your web API can handle the [Protobuf](https://github.com/google/protobuf) format, you might want to use Protobuf with those clients because it's more efficient.</span></span>  <span data-ttu-id="fbbd4-112">È necessario che l'API web per inviare i nomi dei contatti e indirizzi in [vCard](https://wikipedia.org/wiki/VCard) formato, un formato di uso comune per lo scambio di dati di contatto.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-112">Or you might want your web API to send contact names and addresses in [vCard](https://wikipedia.org/wiki/VCard) format, a commonly used format for exchanging contact data.</span></span> <span data-ttu-id="fbbd4-113">L'app di esempio fornito in questo articolo implementa un formattatore vCard semplice.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-113">The sample app provided with this article implements a simple vCard formatter.</span></span>

## <a name="overview-of-how-to-use-a-custom-formatter"></a><span data-ttu-id="fbbd4-114">Panoramica di come utilizzare un formattatore personalizzato</span><span class="sxs-lookup"><span data-stu-id="fbbd4-114">Overview of how to use a custom formatter</span></span>

<span data-ttu-id="fbbd4-115">Ecco i passaggi per creare e utilizzare un formattatore personalizzato:</span><span class="sxs-lookup"><span data-stu-id="fbbd4-115">Here are the steps to create and use a custom formatter:</span></span>

* <span data-ttu-id="fbbd4-116">Se si desidera serializzare i dati da inviare al client, creare una classe di output del formattatore.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-116">Create an output formatter class if you want to serialize data to send to the client.</span></span>
* <span data-ttu-id="fbbd4-117">Se si desidera deserializzare i dati ricevuti dal client, creare una classe formattatore di input.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-117">Create an input formatter class if you want to deserialize data received from the client.</span></span> 
* <span data-ttu-id="fbbd4-118">Aggiungere istanze dei formattatori dal `InputFormatters` e `OutputFormatters` raccolte in [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span><span class="sxs-lookup"><span data-stu-id="fbbd4-118">Add instances of your formatters to the `InputFormatters` and `OutputFormatters` collections in [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span></span>

<span data-ttu-id="fbbd4-119">Le sezioni seguenti forniscono istruzioni ed esempi di codice per ognuno di questi passaggi.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-119">The following sections provide guidance and code examples for each of these steps.</span></span>

## <a name="how-to-create-a-custom-formatter-class"></a><span data-ttu-id="fbbd4-120">Come creare una classe formattatore personalizzato</span><span class="sxs-lookup"><span data-stu-id="fbbd4-120">How to create a custom formatter class</span></span>

<span data-ttu-id="fbbd4-121">Per creare un formattatore:</span><span class="sxs-lookup"><span data-stu-id="fbbd4-121">To create a formatter:</span></span>

* <span data-ttu-id="fbbd4-122">Derivare la classe dalla classe di base appropriata.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-122">Derive the class from the appropriate base class.</span></span>
* <span data-ttu-id="fbbd4-123">Specificare i tipi di supporto valido e le codifiche nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-123">Specify valid media types and encodings in the constructor.</span></span>
* <span data-ttu-id="fbbd4-124">Eseguire l'override `CanReadType` / `CanWriteType` metodi</span><span class="sxs-lookup"><span data-stu-id="fbbd4-124">Override `CanReadType`/`CanWriteType` methods</span></span>
* <span data-ttu-id="fbbd4-125">Eseguire l'override `ReadRequestBodyAsync` / `WriteResponseBodyAsync` metodi</span><span class="sxs-lookup"><span data-stu-id="fbbd4-125">Override `ReadRequestBodyAsync`/`WriteResponseBodyAsync` methods</span></span>
  
### <a name="derive-from-the-appropriate-base-class"></a><span data-ttu-id="fbbd4-126">Derivare dalla classe di base appropriata</span><span class="sxs-lookup"><span data-stu-id="fbbd4-126">Derive from the appropriate base class</span></span>

<span data-ttu-id="fbbd4-127">Per tipi di file di testo (ad esempio, vCard), derivare la [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) o [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) classe di base.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-127">For text media types (for example, vCard), derive from the [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) or [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) base class.</span></span>

<span data-ttu-id="fbbd4-128">[!code-csharp[Principale](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]</span><span class="sxs-lookup"><span data-stu-id="fbbd4-128">[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]</span></span>

<span data-ttu-id="fbbd4-129">Per i tipi binari, derivare la [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) o [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) classe di base.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-129">For binary types, derive from the [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) or [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) base class.</span></span>

### <a name="specify-valid-media-types-and-encodings"></a><span data-ttu-id="fbbd4-130">Specificare le codifiche e tipi di supporto valido</span><span class="sxs-lookup"><span data-stu-id="fbbd4-130">Specify valid media types and encodings</span></span>

<span data-ttu-id="fbbd4-131">Nel costruttore, specificare tipi di supporto valido e codifiche aggiungendo il `SupportedMediaTypes` e `SupportedEncodings` raccolte.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-131">In the constructor, specify valid media types and encodings by adding to the `SupportedMediaTypes` and `SupportedEncodings` collections.</span></span>

<span data-ttu-id="fbbd4-132">[!code-csharp[Principale](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]</span><span class="sxs-lookup"><span data-stu-id="fbbd4-132">[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]</span></span>

> [!NOTE]  
> <span data-ttu-id="fbbd4-133">È possibile eseguire l'inserimento di dipendenze di costruttore in una classe del formattatore.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-133">You can't do constructor dependency injection in a formatter class.</span></span> <span data-ttu-id="fbbd4-134">Ad esempio, è possibile ottenere un logger mediante l'aggiunta di un parametro di logger al costruttore.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-134">For example, you can't get a logger by adding a logger parameter to the constructor.</span></span> <span data-ttu-id="fbbd4-135">Per accedere ai servizi, è necessario utilizzare l'oggetto di contesto passato ai metodi di.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-135">To access services, you have to use the context object that gets passed in to your methods.</span></span> <span data-ttu-id="fbbd4-136">Un esempio di codice [seguito](#read-write) viene illustrato come eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-136">A code example [below](#read-write) shows how to do this.</span></span>

### <a name="override-canreadtypecanwritetype"></a><span data-ttu-id="fbbd4-137">Eseguire l'override CanReadType/CanWriteType</span><span class="sxs-lookup"><span data-stu-id="fbbd4-137">Override CanReadType/CanWriteType</span></span> 

<span data-ttu-id="fbbd4-138">Specificare il tipo è possibile deserializzare in o serializzare da eseguendo l'override di `CanReadType` o `CanWriteType` metodi.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-138">Specify the type you can deserialize into or serialize from by overriding the `CanReadType` or `CanWriteType` methods.</span></span> <span data-ttu-id="fbbd4-139">Ad esempio, potrebbe essere solo in grado di creare testo vCard da un `Contact` tipo e viceversa.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-139">For example, you might only be able to create vCard text from a `Contact` type and vice versa.</span></span>

<span data-ttu-id="fbbd4-140">[!code-csharp[Principale](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]</span><span class="sxs-lookup"><span data-stu-id="fbbd4-140">[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]</span></span>

#### <a name="the-canwriteresult-method"></a><span data-ttu-id="fbbd4-141">Il metodo CanWriteResult</span><span class="sxs-lookup"><span data-stu-id="fbbd4-141">The CanWriteResult method</span></span>

<span data-ttu-id="fbbd4-142">In alcuni scenari è necessario eseguire l'override `CanWriteResult` anziché `CanWriteType`.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-142">In some scenarios you have to override `CanWriteResult` instead of `CanWriteType`.</span></span> <span data-ttu-id="fbbd4-143">Utilizzare `CanWriteResult` se vengono soddisfatte le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="fbbd4-143">Use `CanWriteResult` if the following conditions are true:</span></span>

  * <span data-ttu-id="fbbd4-144">Il metodo di azione restituisce una classe di modello.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-144">Your action method returns a model class.</span></span>
  * <span data-ttu-id="fbbd4-145">Sono disponibili le classi derivate che potrebbero essere restituite in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-145">There are derived classes which might be returned at runtime.</span></span>
  * <span data-ttu-id="fbbd4-146">È necessario conoscere in fase di esecuzione che derivata a classe è stata restituita dall'azione.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-146">You need to know at runtime which derived class was returned by the action.</span></span>  

<span data-ttu-id="fbbd4-147">Ad esempio, si supponga che la firma del metodo di azione restituisce un `Person` tipo, ma può restituire un `Student` o `Instructor` tipo che deriva da `Person`.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-147">For example, suppose your action method signature returns a `Person` type, but it may return a `Student` or `Instructor` type that derives from `Person`.</span></span> <span data-ttu-id="fbbd4-148">Se si desidera che il formattatore di gestire solo `Student` oggetti, controllare il tipo di [oggetto](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) nell'oggetto di contesto fornito per il `CanWriteResult` metodo.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-148">If you want your formatter to handle only `Student` objects, check the type of [Object](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) in the context object provided to the `CanWriteResult` method.</span></span> <span data-ttu-id="fbbd4-149">Si noti che non è necessario utilizzare `CanWriteResult` quando il metodo di azione restituisce `IActionResult`; in tal caso, il `CanWriteType` metodo riceve il tipo di runtime.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-149">Note that it's not necessary to use `CanWriteResult` when the action method returns `IActionResult`; in that case, the `CanWriteType` method receives the runtime type.</span></span>

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a><span data-ttu-id="fbbd4-150">Eseguire l'override ReadRequestBodyAsync/WriteResponseBodyAsync</span><span class="sxs-lookup"><span data-stu-id="fbbd4-150">Override ReadRequestBodyAsync/WriteResponseBodyAsync</span></span> 

<span data-ttu-id="fbbd4-151">Eseguire il lavoro effettivo del deserializzazione o la serializzazione `ReadRequestBodyAsync` o `WriteResponseBodyAsync`.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-151">You do the actual work of deserializing or serializing in `ReadRequestBodyAsync` or `WriteResponseBodyAsync`.</span></span>  <span data-ttu-id="fbbd4-152">Le righe evidenziate nell'esempio seguente viene illustrato come ottenere servizi dal contenitore dell'inserimento di dipendenza (non è possibile ottenere tali da parametri del costruttore).</span><span class="sxs-lookup"><span data-stu-id="fbbd4-152">The highlighted lines in the following example show how to get services from the dependency injection container (you can't get them from constructor parameters).</span></span>

<span data-ttu-id="fbbd4-153">[!code-csharp[Principale](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]</span><span class="sxs-lookup"><span data-stu-id="fbbd4-153">[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]</span></span>

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a><span data-ttu-id="fbbd4-154">Come configurare MVC per utilizzare un formattatore personalizzato</span><span class="sxs-lookup"><span data-stu-id="fbbd4-154">How to configure MVC to use a custom formatter</span></span>
 
<span data-ttu-id="fbbd4-155">Per utilizzare un formattatore personalizzato, aggiungere un'istanza della classe del formattatore per la `InputFormatters` o `OutputFormatters` insieme.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-155">To use a custom formatter, add an instance of the formatter class to the `InputFormatters` or `OutputFormatters` collection.</span></span>

<span data-ttu-id="fbbd4-156">[!code-csharp[Principale](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]</span><span class="sxs-lookup"><span data-stu-id="fbbd4-156">[!code-csharp[Main](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]</span></span>

<span data-ttu-id="fbbd4-157">Formattatori vengono valutati nell'ordine che inserirli.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-157">Formatters are evaluated in the order you insert them.</span></span> <span data-ttu-id="fbbd4-158">Il primo ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-158">The first one takes precedence.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="fbbd4-159">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="fbbd4-159">Next steps</span></span>

<span data-ttu-id="fbbd4-160">Vedere il [applicazione di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), che implementa vCard semplice input e output formattatori.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-160">See the [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), which implements simple vCard input and output formatters.</span></span>  <span data-ttu-id="fbbd4-161">L'applicazione legge e scrive vCard che come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="fbbd4-161">The application reads and writes vCards that look like the following example:</span></span>

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

<span data-ttu-id="fbbd4-162">Per visualizzare vCard output, eseguire l'applicazione, inviare una richiesta Get con Accept intestazione "text/vcard" per `http://localhost:63313/api/contacts/` (se eseguito da Visual Studio) o `http://localhost:5000/api/contacts/` (durante l'esecuzione dalla riga di comando).</span><span class="sxs-lookup"><span data-stu-id="fbbd4-162">To see vCard output, run the application and send a Get request with Accept header "text/vcard" to `http://localhost:63313/api/contacts/` (when running from Visual Studio) or `http://localhost:5000/api/contacts/` (when running from the command line).</span></span>

<span data-ttu-id="fbbd4-163">Per aggiungere un vCard alla raccolta in memoria dei contatti, inviare una richiesta Post all'URL stesso, con l'intestazione Content-Type "text/vcard" e con vCard testo nel corpo, formattato come illustrato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="fbbd4-163">To add a vCard to the in-memory collection of contacts, send a Post request to the same URL, with Content-Type header "text/vcard" and with vCard text in the body, formatted like the example above.</span></span>
