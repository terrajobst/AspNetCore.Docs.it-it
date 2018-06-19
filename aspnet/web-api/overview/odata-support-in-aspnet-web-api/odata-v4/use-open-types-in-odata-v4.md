---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Aprire i tipi in OData v4 con ASP.NET Web API | Documenti Microsoft
author: microsoft
description: In OData v4, un tipo aperto è un tipo stuctured che contiene le proprietà dinamiche, oltre a tutte le proprietà che vengono dichiarate nella definizione del tipo. Apri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2014
ms.topic: article
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: fe67b9a11a82b55d5f3e0e5f1b0cee10a58833d2
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2018
ms.locfileid: "29153272"
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="c20dc-104">Aprire i tipi in OData v4 con ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="c20dc-104">Open Types in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="c20dc-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c20dc-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="c20dc-106">In OData v4, un *aprire tipo* è un tipo stuctured che contiene le proprietà dinamiche, oltre a tutte le proprietà che vengono dichiarate nella definizione del tipo.</span><span class="sxs-lookup"><span data-stu-id="c20dc-106">In OData v4, an *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="c20dc-107">I tipi aperti consentono di aggiungere la flessibilità per i modelli di data.</span><span class="sxs-lookup"><span data-stu-id="c20dc-107">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="c20dc-108">In questa esercitazione viene illustrato come utilizzare i tipi aperti in ASP.NET Web API OData.</span><span class="sxs-lookup"><span data-stu-id="c20dc-108">This tutorial shows how to use open types in ASP.NET Web API OData.</span></span>
> 
> <span data-ttu-id="c20dc-109">In questa esercitazione si presuppone che si conosce già la modalità creare un endpoint OData in ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="c20dc-109">This tutorial assumes that you already know how to create an OData endpoint in ASP.NET Web API.</span></span> <span data-ttu-id="c20dc-110">In caso contrario, iniziare leggendo [creare un Endpoint di OData v4](create-an-odata-v4-endpoint.md) prima.</span><span class="sxs-lookup"><span data-stu-id="c20dc-110">If not, start by reading [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md) first.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c20dc-111">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="c20dc-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="c20dc-112">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="c20dc-112">Web API OData 5.3</span></span>
> - <span data-ttu-id="c20dc-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="c20dc-113">OData v4</span></span>


<span data-ttu-id="c20dc-114">Innanzitutto, la terminologia OData:</span><span class="sxs-lookup"><span data-stu-id="c20dc-114">First, some OData terminology:</span></span>

- <span data-ttu-id="c20dc-115">Tipo di entità: un tipo strutturato con una chiave.</span><span class="sxs-lookup"><span data-stu-id="c20dc-115">Entity type: A structured type with a key.</span></span>
- <span data-ttu-id="c20dc-116">Tipo complesso: un tipo strutturato senza una chiave.</span><span class="sxs-lookup"><span data-stu-id="c20dc-116">Complex type: A structured type without a key.</span></span>
- <span data-ttu-id="c20dc-117">Tipo Open: un tipo con le proprietà dinamiche.</span><span class="sxs-lookup"><span data-stu-id="c20dc-117">Open type: A type with dynamic properties.</span></span> <span data-ttu-id="c20dc-118">È possibile aprire i tipi di entità e complessi.</span><span class="sxs-lookup"><span data-stu-id="c20dc-118">Both entity types and complex types can be open.</span></span>

<span data-ttu-id="c20dc-119">Il valore di una proprietà dinamica può essere un tipo primitivo, un tipo complesso o un tipo di enumerazione. o una raccolta di uno qualsiasi di tali tipi.</span><span class="sxs-lookup"><span data-stu-id="c20dc-119">The value of a dynamic property can be a primitive type, complex type, or enumeration type; or a collection of any of those types.</span></span> <span data-ttu-id="c20dc-120">Per ulteriori informazioni sui tipi aperti, vedere il [OData v4 specifica](http://www.odata.org/documentation/odata-version-4-0/).</span><span class="sxs-lookup"><span data-stu-id="c20dc-120">For more information about open types, see the [OData v4 specification](http://www.odata.org/documentation/odata-version-4-0/).</span></span>

## <a name="install-the-web-odata-libraries"></a><span data-ttu-id="c20dc-121">Installare le librerie di OData Web</span><span class="sxs-lookup"><span data-stu-id="c20dc-121">Install the Web OData Libraries</span></span>

<span data-ttu-id="c20dc-122">Usare Gestione pacchetti NuGet per installare le librerie più recenti di Web API OData.</span><span class="sxs-lookup"><span data-stu-id="c20dc-122">Use NuGet Package Manager to install the latest Web API OData libraries.</span></span> <span data-ttu-id="c20dc-123">Dalla finestra della Console di gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="c20dc-123">From the Package Manager Console window:</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a><span data-ttu-id="c20dc-124">Definire i tipi CLR</span><span class="sxs-lookup"><span data-stu-id="c20dc-124">Define the CLR Types</span></span>

<span data-ttu-id="c20dc-125">Per iniziare, definire i modelli EDM come tipi CLR.</span><span class="sxs-lookup"><span data-stu-id="c20dc-125">Start by defining the EDM models as CLR types.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="c20dc-126">Quando si crea l'Entity Data Model (EDM),</span><span class="sxs-lookup"><span data-stu-id="c20dc-126">When the Entity Data Model (EDM) is created,</span></span>

- <span data-ttu-id="c20dc-127">`Category`è un tipo di enumerazione.</span><span class="sxs-lookup"><span data-stu-id="c20dc-127">`Category` is an enumeration type.</span></span>
- <span data-ttu-id="c20dc-128">`Address`è un tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="c20dc-128">`Address` is a complex type.</span></span> <span data-ttu-id="c20dc-129">(Non ha una chiave, pertanto non è un tipo di entità.)</span><span class="sxs-lookup"><span data-stu-id="c20dc-129">(It does not have a key, so it is not an entity type.)</span></span>
- <span data-ttu-id="c20dc-130">`Customer`è un tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="c20dc-130">`Customer` is an entity type.</span></span> <span data-ttu-id="c20dc-131">(Con una chiave).</span><span class="sxs-lookup"><span data-stu-id="c20dc-131">(It has a key.)</span></span>
- <span data-ttu-id="c20dc-132">`Press`è un tipo complesso aperto.</span><span class="sxs-lookup"><span data-stu-id="c20dc-132">`Press` is an open complex type.</span></span>
- <span data-ttu-id="c20dc-133">`Book`è un tipo di entità aperto.</span><span class="sxs-lookup"><span data-stu-id="c20dc-133">`Book` is an open entity type.</span></span>

<span data-ttu-id="c20dc-134">Per creare un tipo aperto, il tipo di Common Language Runtime deve avere una proprietà di tipo `IDictionary<string, object>`, che contiene le proprietà dinamiche.</span><span class="sxs-lookup"><span data-stu-id="c20dc-134">To create an open type, the CLR type must have a property of type `IDictionary<string, object>`, which holds the dynamic properties.</span></span>

## <a name="build-the-edm-model"></a><span data-ttu-id="c20dc-135">Compilare il modello EDM</span><span class="sxs-lookup"><span data-stu-id="c20dc-135">Build the EDM Model</span></span>

<span data-ttu-id="c20dc-136">Se si utilizza **ODataConventionModelBuilder** per creare il modello EDM, `Press` e `Book` vengono automaticamente aggiunti come tipi aperti, in base alla presenza di un `IDictionary<string, object>` proprietà.</span><span class="sxs-lookup"><span data-stu-id="c20dc-136">If you use **ODataConventionModelBuilder** to create the EDM, `Press` and `Book` are automatically added as open types, based on the presence of a `IDictionary<string, object>` property.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="c20dc-137">È anche possibile compilare il modello EDM a in modo esplicito, tramite **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="c20dc-137">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a><span data-ttu-id="c20dc-138">Aggiungere un Controller OData</span><span class="sxs-lookup"><span data-stu-id="c20dc-138">Add an OData Controller</span></span>

<span data-ttu-id="c20dc-139">Successivamente, aggiungere un controller OData.</span><span class="sxs-lookup"><span data-stu-id="c20dc-139">Next, add an OData controller.</span></span> <span data-ttu-id="c20dc-140">Per questa esercitazione, si userà un controller semplificato che supporta solo GET e post-richieste e utilizza un elenco in memoria per archiviare entità.</span><span class="sxs-lookup"><span data-stu-id="c20dc-140">For this tutorial, we'll use a simplified controller that just supports GET and POST requests, and uses an in-memory list to store entities.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="c20dc-141">Si noti che il primo `Book` istanza non dispone di alcuna proprietà dinamiche.</span><span class="sxs-lookup"><span data-stu-id="c20dc-141">Notice that the first `Book` instance has no dynamic properties.</span></span> <span data-ttu-id="c20dc-142">Il secondo `Book` istanza ha le proprietà dinamiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="c20dc-142">The second `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="c20dc-143">"Pubblicati": tipo primitivo</span><span class="sxs-lookup"><span data-stu-id="c20dc-143">"Published": Primitive type</span></span>
- <span data-ttu-id="c20dc-144">"Gli autori": raccolta di tipi primitivi</span><span class="sxs-lookup"><span data-stu-id="c20dc-144">"Authors": Collection of primitive types</span></span>
- <span data-ttu-id="c20dc-145">"OtherCategories": raccolta di tipi di enumerazione.</span><span class="sxs-lookup"><span data-stu-id="c20dc-145">"OtherCategories": Collection of enumeration types.</span></span>

<span data-ttu-id="c20dc-146">Inoltre, il `Press` proprietà di tale `Book` istanza ha le proprietà dinamiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="c20dc-146">Also, the `Press` property of that `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="c20dc-147">"Blog": tipo primitivo</span><span class="sxs-lookup"><span data-stu-id="c20dc-147">"Blog": Primitive type</span></span>
- <span data-ttu-id="c20dc-148">"Address": tipo complesso</span><span class="sxs-lookup"><span data-stu-id="c20dc-148">"Address": Complex type</span></span>

## <a name="query-the-metadata"></a><span data-ttu-id="c20dc-149">Query sui metadati</span><span class="sxs-lookup"><span data-stu-id="c20dc-149">Query the Metadata</span></span>

<span data-ttu-id="c20dc-150">Per ottenere il documento di metadati OData, inviare una richiesta GET al `~/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="c20dc-150">To get the OData metadata document, send a GET request to `~/$metadata`.</span></span> <span data-ttu-id="c20dc-151">Il corpo della risposta dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="c20dc-151">The response body should look similar to this:</span></span>

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

<span data-ttu-id="c20dc-152">Nel documento di metadati, è possibile vedere che:</span><span class="sxs-lookup"><span data-stu-id="c20dc-152">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="c20dc-153">Per il `Book` e `Press` tipi, il valore di `OpenType` attributo è true.</span><span class="sxs-lookup"><span data-stu-id="c20dc-153">For the `Book` and `Press` types, the value of the `OpenType` attribute is true.</span></span> <span data-ttu-id="c20dc-154">Il `Customer` e `Address` tipi non dispongono di questo attributo.</span><span class="sxs-lookup"><span data-stu-id="c20dc-154">The `Customer` and `Address` types don't have this attribute.</span></span>
- <span data-ttu-id="c20dc-155">Il `Book` tipo di entità include tre proprietà dichiarate: ISBN, titolo e premere.</span><span class="sxs-lookup"><span data-stu-id="c20dc-155">The `Book` entity type has three declared properties: ISBN, Title, and Press.</span></span> <span data-ttu-id="c20dc-156">I metadati di OData non include il `Book.Properties` proprietà dalla classe CLR.</span><span class="sxs-lookup"><span data-stu-id="c20dc-156">The OData metadata does not include the `Book.Properties` property from the CLR class.</span></span>
- <span data-ttu-id="c20dc-157">Analogamente, il `Press` tipo complesso ha solo due proprietà dichiarate: nome e la categoria.</span><span class="sxs-lookup"><span data-stu-id="c20dc-157">Similarly, the `Press` complex type has only two declared properties: Name and Category.</span></span> <span data-ttu-id="c20dc-158">I metadati includono la `Press.DynamicProperties` proprietà dalla classe CLR.</span><span class="sxs-lookup"><span data-stu-id="c20dc-158">The metadata does not include the `Press.DynamicProperties` property from the CLR class.</span></span>

## <a name="query-an-entity"></a><span data-ttu-id="c20dc-159">Query un'entità</span><span class="sxs-lookup"><span data-stu-id="c20dc-159">Query an Entity</span></span>

<span data-ttu-id="c20dc-160">Per ottenere il libro con codice ISBN uguale a "978-0-7356-7942-9", inviare una richiesta GET al `~/Books('978-0-7356-7942-9')`.</span><span class="sxs-lookup"><span data-stu-id="c20dc-160">To get the book with ISBN equal to "978-0-7356-7942-9", send a GET request to `~/Books('978-0-7356-7942-9')`.</span></span> <span data-ttu-id="c20dc-161">Il corpo della risposta sarà simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="c20dc-161">The response body should look similar to the following.</span></span> <span data-ttu-id="c20dc-162">(Rientrati per rendere più leggibili).</span><span class="sxs-lookup"><span data-stu-id="c20dc-162">(Indented to make it more readable.)</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

<span data-ttu-id="c20dc-163">Si noti che le proprietà dinamiche sono incluse inline con le proprietà dichiarate.</span><span class="sxs-lookup"><span data-stu-id="c20dc-163">Notice that the dynamic properties are included inline with the declared properties.</span></span>

## <a name="post-an-entity"></a><span data-ttu-id="c20dc-164">REGISTRARE un'entità</span><span class="sxs-lookup"><span data-stu-id="c20dc-164">POST an Entity</span></span>

<span data-ttu-id="c20dc-165">Per aggiungere un'entità di libro, inviare una richiesta POST per `~/Books`.</span><span class="sxs-lookup"><span data-stu-id="c20dc-165">To add a Book entity, send a POST request to `~/Books`.</span></span> <span data-ttu-id="c20dc-166">Il client può impostare le proprietà dinamiche nel payload della richiesta.</span><span class="sxs-lookup"><span data-stu-id="c20dc-166">The client can set dynamic properties in the request payload.</span></span>

<span data-ttu-id="c20dc-167">Di seguito è riportato un esempio di richiesta.</span><span class="sxs-lookup"><span data-stu-id="c20dc-167">Here is an example request.</span></span> <span data-ttu-id="c20dc-168">Si noti la proprietà "Price" e "Pubblicato".</span><span class="sxs-lookup"><span data-stu-id="c20dc-168">Note the "Price" and "Published" properties.</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

<span data-ttu-id="c20dc-169">Se si imposta un punto di interruzione nel metodo del controller, è possibile vedere che l'API Web aggiunto queste proprietà per il `Properties` dizionario.</span><span class="sxs-lookup"><span data-stu-id="c20dc-169">If you set a breakpoint in the controller method, you can see that Web API added these properties to the `Properties` dictionary.</span></span>

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="c20dc-170">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c20dc-170">Additional Resources</span></span>

[<span data-ttu-id="c20dc-171">Esempio di tipo Open OData</span><span class="sxs-lookup"><span data-stu-id="c20dc-171">OData Open Type Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
