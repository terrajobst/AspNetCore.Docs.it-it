---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Convalida del modello in API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 75474d5597ba11e09f788269b9a2a278559c63a9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393096"
---
<a name="model-validation-in-aspnet-web-api"></a><span data-ttu-id="2460a-102">Convalida del modello in API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2460a-102">Model Validation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="2460a-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2460a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="2460a-104">Quando un client invia dati all'API Web, spesso si desidera convalidare i dati prima di eseguire qualsiasi elaborazione.</span><span class="sxs-lookup"><span data-stu-id="2460a-104">When a client sends data to your web API, often you want to validate the data before doing any processing.</span></span> <span data-ttu-id="2460a-105">Questo articolo illustra come annotare i modelli, usare le annotazioni per la convalida dei dati e la gestione degli errori di convalida nell'API web.</span><span class="sxs-lookup"><span data-stu-id="2460a-105">This article shows how to annotate your models, use the annotations for data validation, and handle validation errors in your web API.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="2460a-106">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="2460a-106">Data Annotations</span></span>

<span data-ttu-id="2460a-107">Nell'API Web ASP.NET, è possibile usare gli attributi dal [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) dello spazio dei nomi per impostare le regole di convalida per le proprietà nel modello.</span><span class="sxs-lookup"><span data-stu-id="2460a-107">In ASP.NET Web API, you can use attributes from the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace to set validation rules for properties on your model.</span></span> <span data-ttu-id="2460a-108">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="2460a-108">Consider the following model:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="2460a-109">Se è stata usata la convalida del modello in ASP.NET MVC, si dovrebbe avere familiarità.</span><span class="sxs-lookup"><span data-stu-id="2460a-109">If you have used model validation in ASP.NET MVC, this should look familiar.</span></span> <span data-ttu-id="2460a-110">Il **necessari** attributo indica che il `Name` proprietà non può essere null.</span><span class="sxs-lookup"><span data-stu-id="2460a-110">The **Required** attribute says that the `Name` property must not be null.</span></span> <span data-ttu-id="2460a-111">Il **Range** attributo indica che `Weight` deve essere compreso tra 0 e 999.</span><span class="sxs-lookup"><span data-stu-id="2460a-111">The **Range** attribute says that `Weight` must be between zero and 999.</span></span>

<span data-ttu-id="2460a-112">Si supponga che un client invia una richiesta POST con la rappresentazione JSON seguente:</span><span class="sxs-lookup"><span data-stu-id="2460a-112">Suppose that a client sends a POST request with the following JSON representation:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

<span data-ttu-id="2460a-113">È possibile vedere che il client non comprendeva i `Name` proprietà, che è contrassegnato come obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="2460a-113">You can see that the client did not include the `Name` property, which is marked as required.</span></span> <span data-ttu-id="2460a-114">Quando Web API converte il codice JSON in una `Product` istanza, vengono convalidate le `Product` contro gli attributi di convalida.</span><span class="sxs-lookup"><span data-stu-id="2460a-114">When Web API converts the JSON into a `Product` instance, it validates the `Product` against the validation attributes.</span></span> <span data-ttu-id="2460a-115">Nell'ambito dell'azione del controller, è possibile controllare se il modello è valido:</span><span class="sxs-lookup"><span data-stu-id="2460a-115">In your controller action, you can check whether the model is valid:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="2460a-116">Convalida del modello non garantisce che i dati del client sono sicuri.</span><span class="sxs-lookup"><span data-stu-id="2460a-116">Model validation does not guarantee that client data is safe.</span></span> <span data-ttu-id="2460a-117">Potrebbe essere necessaria una convalida aggiuntiva in altri livelli dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2460a-117">Additional validation might be needed in other layers of the application.</span></span> <span data-ttu-id="2460a-118">(Ad esempio, il livello di dati potrebbe imporre vincoli di chiave esterna). L'esercitazione [uso di API Web con Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) esamina alcuni di questi problemi.</span><span class="sxs-lookup"><span data-stu-id="2460a-118">(For example, the data layer might enforce foreign key constraints.) The tutorial [Using Web API with Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explores some of these issues.</span></span>

<span data-ttu-id="2460a-119">**"Pubblicando correggerà"**: correggerà la registrazione si verifica quando il client esclude alcune proprietà.</span><span class="sxs-lookup"><span data-stu-id="2460a-119">**"Under-Posting"**: Under-posting happens when the client leaves out some properties.</span></span> <span data-ttu-id="2460a-120">Si supponga, ad esempio, che il client invia la seguente:</span><span class="sxs-lookup"><span data-stu-id="2460a-120">For example, suppose the client sends the following:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

<span data-ttu-id="2460a-121">In questo caso, il client non sono stati specificati valori per `Price` o `Weight`.</span><span class="sxs-lookup"><span data-stu-id="2460a-121">Here, the client did not specify values for `Price` or `Weight`.</span></span> <span data-ttu-id="2460a-122">Il formattatore JSON assegna un valore predefinito pari a zero per le proprietà mancante.</span><span class="sxs-lookup"><span data-stu-id="2460a-122">The JSON formatter assigns a default value of zero to the missing properties.</span></span>

![](model-validation-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="2460a-123">Lo stato del modello è valido, poiché lo zero è un valore valido per queste proprietà.</span><span class="sxs-lookup"><span data-stu-id="2460a-123">The model state is valid, because zero is a valid value for these properties.</span></span> <span data-ttu-id="2460a-124">Se si tratta di un problema dipende dallo scenario.</span><span class="sxs-lookup"><span data-stu-id="2460a-124">Whether this is a problem depends on your scenario.</span></span> <span data-ttu-id="2460a-125">Ad esempio, in un'operazione di aggiornamento, si potrebbe voler distinguere tra "0" e "non impostata."</span><span class="sxs-lookup"><span data-stu-id="2460a-125">For example, in an update operation, you might want to distinguish between "zero" and "not set."</span></span> <span data-ttu-id="2460a-126">Per forzare i client per impostare un valore, impostare la proprietà che ammette valori null e viene impostata la **necessari** attributo:</span><span class="sxs-lookup"><span data-stu-id="2460a-126">To force clients to set a value, make the property nullable and set the **Required** attribute:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

<span data-ttu-id="2460a-127">**"Overposting"**: un client può anche inviare *altre* dati quanto ci si aspetterebbe.</span><span class="sxs-lookup"><span data-stu-id="2460a-127">**"Over-Posting"**: A client can also send *more* data than you expected.</span></span> <span data-ttu-id="2460a-128">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2460a-128">For example:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

<span data-ttu-id="2460a-129">In questo caso, il codice JSON include una proprietà ("colore") che non esiste nel `Product` modello.</span><span class="sxs-lookup"><span data-stu-id="2460a-129">Here, the JSON includes a property ("Color") that does not exist in the `Product` model.</span></span> <span data-ttu-id="2460a-130">In questo caso, il formattatore JSON ignora semplicemente questo valore.</span><span class="sxs-lookup"><span data-stu-id="2460a-130">In this case, the JSON formatter simply ignores this value.</span></span> <span data-ttu-id="2460a-131">(Il formattatore XML esegue la stessa). Evitare l'overposting può causare problemi se il modello contiene le proprietà che si devono essere di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="2460a-131">(The XML formatter does the same.) Over-posting causes problems if your model has properties that you intended to be read-only.</span></span> <span data-ttu-id="2460a-132">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2460a-132">For example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="2460a-133">Preferibile che gli utenti per l'aggiornamento di `IsAdmin` proprietà ed elevare se stesso agli amministratori.</span><span class="sxs-lookup"><span data-stu-id="2460a-133">You don't want users to update the `IsAdmin` property and elevate themselves to administrators!</span></span> <span data-ttu-id="2460a-134">La strategia più sicura consiste nell'usare una classe di modello che corrisponde esattamente a ciò che il client è autorizzato a inviare:</span><span class="sxs-lookup"><span data-stu-id="2460a-134">The safest strategy is to use a model class that exactly matches what the client is allowed to send:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="2460a-135">Post di blog di Brad Wilson "[vs: convalida dell'Input. Convalida del modello in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"dispone per una trattazione correggerà la registrazione, evitare l'overposting.</span><span class="sxs-lookup"><span data-stu-id="2460a-135">Brad Wilson's blog post "[Input Validation vs. Model Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" has a good discussion of under-posting and over-posting.</span></span> <span data-ttu-id="2460a-136">Anche se il post è su ASP.NET MVC 2, i problemi sono ancora rilevanti per l'API Web.</span><span class="sxs-lookup"><span data-stu-id="2460a-136">Although the post is about ASP.NET MVC 2, the issues are still relevant to Web API.</span></span>


## <a name="handling-validation-errors"></a><span data-ttu-id="2460a-137">Gestione degli errori di convalida</span><span class="sxs-lookup"><span data-stu-id="2460a-137">Handling Validation Errors</span></span>

<span data-ttu-id="2460a-138">API Web non restituire un errore automaticamente al client quando si verifica un errore di convalida.</span><span class="sxs-lookup"><span data-stu-id="2460a-138">Web API does not automatically return an error to the client when validation fails.</span></span> <span data-ttu-id="2460a-139">È responsabilità dell'azione del controller per verificare lo stato del modello e rispondere in modo appropriato.</span><span class="sxs-lookup"><span data-stu-id="2460a-139">It is up to the controller action to check the model state and respond appropriately.</span></span>

<span data-ttu-id="2460a-140">È anche possibile creare un filtro azioni per controllare lo stato del modello prima che venga richiamato l'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="2460a-140">You can also create an action filter to check the model state before the controller action is invoked.</span></span> <span data-ttu-id="2460a-141">Il codice seguente viene illustrato un esempio:</span><span class="sxs-lookup"><span data-stu-id="2460a-141">The following code shows an example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="2460a-142">Se si verifica un errore di convalida del modello, questo filtro restituisce una risposta HTTP che contiene gli errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="2460a-142">If model validation fails, this filter returns an HTTP response that contains the validation errors.</span></span> <span data-ttu-id="2460a-143">In tal caso, non viene richiamata l'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="2460a-143">In that case, the controller action is not invoked.</span></span>

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

<span data-ttu-id="2460a-144">Per applicare il filtro selezionato a tutti i controller API Web, aggiungere un'istanza del filtro per il **HttpConfiguration.Filters** raccolta durante la configurazione:</span><span class="sxs-lookup"><span data-stu-id="2460a-144">To apply this filter to all Web API controllers, add an instance of the filter to the **HttpConfiguration.Filters** collection during configuration:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="2460a-145">Un'altra opzione consiste nell'impostare il filtro come attributo in singoli controller o azioni del controller:</span><span class="sxs-lookup"><span data-stu-id="2460a-145">Another option is to set the filter as an attribute on individual controllers or controller actions:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
