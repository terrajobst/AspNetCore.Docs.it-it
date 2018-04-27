---
title: Formattatori personalizzati nelle API Web ASP.NET Core MVC
author: tdykstra
description: Informazioni su come creare e usare i formattatori personalizzati nelle API Web ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.date: 02/08/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/models/custom-formatters
ms.openlocfilehash: 8a42f2d885bd0a0c6d2bd05f9c589def2e15d50a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="custom-formatters-in-aspnet-core-mvc-web-apis"></a><span data-ttu-id="29947-103">Formattatori personalizzati nelle API Web ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="29947-103">Custom formatters in ASP.NET Core MVC web APIs</span></span>

<span data-ttu-id="29947-104">[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="29947-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="29947-105">ASP.NET Core MVC offre supporto predefinito per lo scambio di dati in API Web usando i formati JSON, XML o testo normale.</span><span class="sxs-lookup"><span data-stu-id="29947-105">ASP.NET Core MVC has built-in support for data exchange in web APIs by using JSON, XML, or plain text formats.</span></span> <span data-ttu-id="29947-106">In questo articolo viene illustrato come aggiungere supporto per altri formati creando formattatori personalizzati.</span><span class="sxs-lookup"><span data-stu-id="29947-106">This article shows how to add support for additional formats by creating custom formatters.</span></span>

<span data-ttu-id="29947-107">[Visualizzare o scaricare l'esempio da GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).</span><span class="sxs-lookup"><span data-stu-id="29947-107">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).</span></span>

## <a name="when-to-use-custom-formatters"></a><span data-ttu-id="29947-108">Quando usare i formattatori personalizzati</span><span class="sxs-lookup"><span data-stu-id="29947-108">When to use custom formatters</span></span>

<span data-ttu-id="29947-109">È possibile usare un formattatore personalizzato quando il processo di [negoziazione del contenuto](xref:mvc/models/formatting) deve supportare un tipo di contenuto che non è supportato dai formattatori predefiniti (JSON, XML e testo normale).</span><span class="sxs-lookup"><span data-stu-id="29947-109">Use a custom formatter when you want the [content negotiation](xref:mvc/models/formatting) process to support a content type that isn't supported by the built-in formatters (JSON, XML, and plain text).</span></span>

<span data-ttu-id="29947-110">Ad esempio, se alcuni dei client per l'API Web gestiscono il formato [Protobuf](https://github.com/google/protobuf), si potrebbe voler usare Protobuf con tali client per una maggiore efficienza.</span><span class="sxs-lookup"><span data-stu-id="29947-110">For example, if some of the clients for your web API can handle the [Protobuf](https://github.com/google/protobuf) format, you might want to use Protobuf with those clients because it's more efficient.</span></span>  <span data-ttu-id="29947-111">Oppure potrebbe essere necessario che l'API Web invii nomi di contatto e indirizzi nel formato [vCard](https://wikipedia.org/wiki/VCard), un formato usato comunemente per lo scambio di dati di contatto.</span><span class="sxs-lookup"><span data-stu-id="29947-111">Or you might want your web API to send contact names and addresses in [vCard](https://wikipedia.org/wiki/VCard) format, a commonly used format for exchanging contact data.</span></span> <span data-ttu-id="29947-112">L'app di esempio disponibile in questo articolo implementa un semplice formattatore vCard.</span><span class="sxs-lookup"><span data-stu-id="29947-112">The sample app provided with this article implements a simple vCard formatter.</span></span>

## <a name="overview-of-how-to-use-a-custom-formatter"></a><span data-ttu-id="29947-113">Panoramica sull'uso di un formattatore personalizzato</span><span class="sxs-lookup"><span data-stu-id="29947-113">Overview of how to use a custom formatter</span></span>

<span data-ttu-id="29947-114">Di seguito sono elencati i passaggi per creare e usare un formattatore personalizzato:</span><span class="sxs-lookup"><span data-stu-id="29947-114">Here are the steps to create and use a custom formatter:</span></span>

* <span data-ttu-id="29947-115">Creare una classe formattatore di output se si vogliono serializzare i dati da inviare al client.</span><span class="sxs-lookup"><span data-stu-id="29947-115">Create an output formatter class if you want to serialize data to send to the client.</span></span>
* <span data-ttu-id="29947-116">Creare una classe formattatore di input se si vogliono deserializzare i dati ricevuti dal client.</span><span class="sxs-lookup"><span data-stu-id="29947-116">Create an input formatter class if you want to deserialize data received from the client.</span></span> 
* <span data-ttu-id="29947-117">Aggiungere le istanze dei formattatori alle raccolte `InputFormatters` e `OutputFormatters` in [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span><span class="sxs-lookup"><span data-stu-id="29947-117">Add instances of your formatters to the `InputFormatters` and `OutputFormatters` collections in [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span></span>

<span data-ttu-id="29947-118">Nelle sezioni seguenti sono disponibili indicazioni ed esempi di codici per ognuno dei passaggi specificati.</span><span class="sxs-lookup"><span data-stu-id="29947-118">The following sections provide guidance and code examples for each of these steps.</span></span>

## <a name="how-to-create-a-custom-formatter-class"></a><span data-ttu-id="29947-119">Come creare una classe formattatore personalizzato</span><span class="sxs-lookup"><span data-stu-id="29947-119">How to create a custom formatter class</span></span>

<span data-ttu-id="29947-120">Per creare un formattatore:</span><span class="sxs-lookup"><span data-stu-id="29947-120">To create a formatter:</span></span>

* <span data-ttu-id="29947-121">Derivare la classe dalla classe di base appropriata.</span><span class="sxs-lookup"><span data-stu-id="29947-121">Derive the class from the appropriate base class.</span></span>
* <span data-ttu-id="29947-122">Specificare i formati di testo multimediali validi e le codifiche nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="29947-122">Specify valid media types and encodings in the constructor.</span></span>
* <span data-ttu-id="29947-123">Eseguire l'override dei metodi `CanReadType`/`CanWriteType`</span><span class="sxs-lookup"><span data-stu-id="29947-123">Override `CanReadType`/`CanWriteType` methods</span></span>
* <span data-ttu-id="29947-124">Eseguire l'override dei metodi `ReadRequestBodyAsync`/`WriteResponseBodyAsync`</span><span class="sxs-lookup"><span data-stu-id="29947-124">Override `ReadRequestBodyAsync`/`WriteResponseBodyAsync` methods</span></span>
  
### <a name="derive-from-the-appropriate-base-class"></a><span data-ttu-id="29947-125">Derivare dalla classe di base appropriata</span><span class="sxs-lookup"><span data-stu-id="29947-125">Derive from the appropriate base class</span></span>

<span data-ttu-id="29947-126">Per i formati multimediali di testo (ad esempio, vCard), derivare dalla classe [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) o [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter).</span><span class="sxs-lookup"><span data-stu-id="29947-126">For text media types (for example, vCard), derive from the [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) or [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) base class.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

<span data-ttu-id="29947-127">Per i tipi binari, derivare dalla classe di base [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) o [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter).</span><span class="sxs-lookup"><span data-stu-id="29947-127">For binary types, derive from the [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) or [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) base class.</span></span>

### <a name="specify-valid-media-types-and-encodings"></a><span data-ttu-id="29947-128">Specificare i formati multimediali validi e le codifiche</span><span class="sxs-lookup"><span data-stu-id="29947-128">Specify valid media types and encodings</span></span>

<span data-ttu-id="29947-129">Nel costruttore specificare i formati multimediali validi e le codifiche aggiungendo alle raccolte `SupportedMediaTypes` e `SupportedEncodings`.</span><span class="sxs-lookup"><span data-stu-id="29947-129">In the constructor, specify valid media types and encodings by adding to the `SupportedMediaTypes` and `SupportedEncodings` collections.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]  
> <span data-ttu-id="29947-130">In una classe formattatore non è possibile inserire le dipendenze del costruttore.</span><span class="sxs-lookup"><span data-stu-id="29947-130">You can't do constructor dependency injection in a formatter class.</span></span> <span data-ttu-id="29947-131">Ad esempio, non è possibile ottenere un logger aggiungendo un parametro di logger al costruttore.</span><span class="sxs-lookup"><span data-stu-id="29947-131">For example, you can't get a logger by adding a logger parameter to the constructor.</span></span> <span data-ttu-id="29947-132">Per accedere ai servizi, usare l'oggetto di contesto che viene passato ai metodi.</span><span class="sxs-lookup"><span data-stu-id="29947-132">To access services, you have to use the context object that gets passed in to your methods.</span></span> <span data-ttu-id="29947-133">Nell'esempio di codice riportato [di seguito](#read-write) viene illustrato come eseguire tale operazione.</span><span class="sxs-lookup"><span data-stu-id="29947-133">A code example [below](#read-write) shows how to do this.</span></span>

### <a name="override-canreadtypecanwritetype"></a><span data-ttu-id="29947-134">Eseguire l'override di CanReadType/CanWriteType</span><span class="sxs-lookup"><span data-stu-id="29947-134">Override CanReadType/CanWriteType</span></span> 

<span data-ttu-id="29947-135">Eseguire l'override dei metodi `CanReadType` o `CanWriteType` per specificare il tipo in cui eseguire la deserializzazione o il tipo da cui eseguire la serializzazione.</span><span class="sxs-lookup"><span data-stu-id="29947-135">Specify the type you can deserialize into or serialize from by overriding the `CanReadType` or `CanWriteType` methods.</span></span> <span data-ttu-id="29947-136">Ad esempio, potrebbe essere possibile creare solo un testo vCard da un tipo `Contact` e viceversa.</span><span class="sxs-lookup"><span data-stu-id="29947-136">For example, you might only be able to create vCard text from a `Contact` type and vice versa.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a><span data-ttu-id="29947-137">Metodo CanWriteResult</span><span class="sxs-lookup"><span data-stu-id="29947-137">The CanWriteResult method</span></span>

<span data-ttu-id="29947-138">In alcuni scenari è necessario eseguire l'override di `CanWriteResult` anziché di `CanWriteType`.</span><span class="sxs-lookup"><span data-stu-id="29947-138">In some scenarios you have to override `CanWriteResult` instead of `CanWriteType`.</span></span> <span data-ttu-id="29947-139">Usare `CanWriteResult` se vengono soddisfatte le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="29947-139">Use `CanWriteResult` if the following conditions are true:</span></span>

  * <span data-ttu-id="29947-140">Il metodo di azione restituisce una classe modello.</span><span class="sxs-lookup"><span data-stu-id="29947-140">Your action method returns a model class.</span></span>
  * <span data-ttu-id="29947-141">Sono disponibili classi derivate che possono essere restituite in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="29947-141">There are derived classes which might be returned at runtime.</span></span>
  * <span data-ttu-id="29947-142">In fase di esecuzione è necessario conoscere quale classe derivata è stata restituita dall'azione.</span><span class="sxs-lookup"><span data-stu-id="29947-142">You need to know at runtime which derived class was returned by the action.</span></span>  

<span data-ttu-id="29947-143">Ad esempio, si supponga che la firma del metodo di azione restituisca un tipo `Person`, ma può restituire un tipo `Student` o `Instructor` che deriva da `Person`.</span><span class="sxs-lookup"><span data-stu-id="29947-143">For example, suppose your action method signature returns a `Person` type, but it may return a `Student` or `Instructor` type that derives from `Person`.</span></span> <span data-ttu-id="29947-144">Se il formattatore deve gestire solo oggetti `Student`, controllare il tipo di [oggetto](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) nell'oggetto di contesto specificato per il metodo `CanWriteResult`.</span><span class="sxs-lookup"><span data-stu-id="29947-144">If you want your formatter to handle only `Student` objects, check the type of [Object](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) in the context object provided to the `CanWriteResult` method.</span></span> <span data-ttu-id="29947-145">Si noti che non è necessario usare `CanWriteResult` quando il metodo di azione restituisce `IActionResult`; in tal caso, il metodo `CanWriteType` riceve il tipo di runtime.</span><span class="sxs-lookup"><span data-stu-id="29947-145">Note that it's not necessary to use `CanWriteResult` when the action method returns `IActionResult`; in that case, the `CanWriteType` method receives the runtime type.</span></span>

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a><span data-ttu-id="29947-146">Eseguire l'override di ReadRequestBodyAsync/WriteResponseBodyAsync</span><span class="sxs-lookup"><span data-stu-id="29947-146">Override ReadRequestBodyAsync/WriteResponseBodyAsync</span></span> 

<span data-ttu-id="29947-147">Le effettive operazioni di deserializzazione o serializzazione vengono eseguite in `ReadRequestBodyAsync` o `WriteResponseBodyAsync`.</span><span class="sxs-lookup"><span data-stu-id="29947-147">You do the actual work of deserializing or serializing in `ReadRequestBodyAsync` or `WriteResponseBodyAsync`.</span></span>  <span data-ttu-id="29947-148">Le righe evidenziate nell'esempio seguente indicano come ottenere i servizi dal contenitore dell'inserimento delle dipendenze. Non è possibile ottenerli dai parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="29947-148">The highlighted lines in the following example show how to get services from the dependency injection container (you can't get them from constructor parameters).</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a><span data-ttu-id="29947-149">Come configurare MVC per usare un formattatore personalizzato</span><span class="sxs-lookup"><span data-stu-id="29947-149">How to configure MVC to use a custom formatter</span></span>
 
<span data-ttu-id="29947-150">Per usare un formattatore personalizzato, aggiungere un'istanza della classe formattatore alla raccolta `InputFormatters` o `OutputFormatters`.</span><span class="sxs-lookup"><span data-stu-id="29947-150">To use a custom formatter, add an instance of the formatter class to the `InputFormatters` or `OutputFormatters` collection.</span></span>

[!code-csharp[Main](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

<span data-ttu-id="29947-151">I formattatori vengono valutati nell'ordine d'inserimento.</span><span class="sxs-lookup"><span data-stu-id="29947-151">Formatters are evaluated in the order you insert them.</span></span> <span data-ttu-id="29947-152">Il primo ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="29947-152">The first one takes precedence.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="29947-153">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="29947-153">Next steps</span></span>

<span data-ttu-id="29947-154">Vedere l'[applicazione di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), che implementa semplici formattatori di input e output vCard.</span><span class="sxs-lookup"><span data-stu-id="29947-154">See the [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), which implements simple vCard input and output formatters.</span></span>  <span data-ttu-id="29947-155">L'applicazione legge e scrive file multimediali vCard simili all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="29947-155">The application reads and writes vCards that look like the following example:</span></span>

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

<span data-ttu-id="29947-156">Per visualizzare l'output vCard, eseguire l'applicazione, inviare una richiesta Get con intestazione Accept "text/vcard" a `http://localhost:63313/api/contacts/` (se l'applicazione è eseguita da Visual Studio) o a `http://localhost:5000/api/contacts/` (se l'applicazione è eseguita dalla riga di comando).</span><span class="sxs-lookup"><span data-stu-id="29947-156">To see vCard output, run the application and send a Get request with Accept header "text/vcard" to `http://localhost:63313/api/contacts/` (when running from Visual Studio) or `http://localhost:5000/api/contacts/` (when running from the command line).</span></span>

<span data-ttu-id="29947-157">Per aggiungere un file vCard alla raccolta di contatti in memoria, inviare una richiesta Post allo stesso URL, con intestazione Content-Type "text/vcard" e con testo vCard nel corpo, formattato come illustrato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="29947-157">To add a vCard to the in-memory collection of contacts, send a Post request to the same URL, with Content-Type header "text/vcard" and with vCard text in the body, formatted like the example above.</span></span>