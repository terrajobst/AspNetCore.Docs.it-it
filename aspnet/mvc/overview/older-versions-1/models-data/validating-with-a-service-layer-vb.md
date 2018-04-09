---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
title: Convalida con un livello di servizio (VB) | Documenti Microsoft
author: StephenWalther
description: Informazioni su come spostare la logica di convalida le azioni del controller e in un livello di servizio separato. In questa esercitazione, Stephen Walther illustra come è...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 344bb38e-4965-4c47-bda1-f6d29ae5b83a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: bb1191b663f863bf881def620efab4f2f03edc56
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="validating-with-a-service-layer-vb"></a><span data-ttu-id="708c8-104">Convalida con un livello di servizio (VB)</span><span class="sxs-lookup"><span data-stu-id="708c8-104">Validating with a Service Layer (VB)</span></span>
====================
<span data-ttu-id="708c8-105">da [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="708c8-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="708c8-106">Informazioni su come spostare la logica di convalida le azioni del controller e in un livello di servizio separato.</span><span class="sxs-lookup"><span data-stu-id="708c8-106">Learn how to move your validation logic out of your controller actions and into a separate service layer.</span></span> <span data-ttu-id="708c8-107">In questa esercitazione, Stephen Walther viene illustrato come è possibile mantenere una separazione delle problematiche acuta isolando il livello di servizio dal livello del controller.</span><span class="sxs-lookup"><span data-stu-id="708c8-107">In this tutorial, Stephen Walther explains how you can maintain a sharp separation of concerns by isolating your service layer from your controller layer.</span></span>


<span data-ttu-id="708c8-108">L'obiettivo di questa esercitazione viene descritto un metodo di eseguire la convalida in un'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="708c8-108">The goal of this tutorial is to describe one method of performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="708c8-109">In questa esercitazione informazioni su come spostare la logica di convalida, i controller e in un livello di servizio separato.</span><span class="sxs-lookup"><span data-stu-id="708c8-109">In this tutorial, you learn how to move your validation logic out of your controllers and into a separate service layer.</span></span>

## <a name="separating-concerns"></a><span data-ttu-id="708c8-110">Separazione dei problemi</span><span class="sxs-lookup"><span data-stu-id="708c8-110">Separating Concerns</span></span>

<span data-ttu-id="708c8-111">Quando si compila un'applicazione MVC ASP.NET, non inserire la logica del database all'interno di azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="708c8-111">When you build an ASP.NET MVC application, you should not place your database logic inside your controller actions.</span></span> <span data-ttu-id="708c8-112">Combinazione logica del database e controller rende più difficile da gestire nel tempo l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="708c8-112">Mixing your database and controller logic makes your application more difficult to maintain over time.</span></span> <span data-ttu-id="708c8-113">Si consiglia di inserire tutta la logica di database in un livello di repository separato.</span><span class="sxs-lookup"><span data-stu-id="708c8-113">The recommendation is that you place all of your database logic in a separate repository layer.</span></span>

<span data-ttu-id="708c8-114">Ad esempio, listato 1 contiene un repository semplice denominato il ProductRepository.</span><span class="sxs-lookup"><span data-stu-id="708c8-114">For example, Listing 1 contains a simple repository named the ProductRepository.</span></span> <span data-ttu-id="708c8-115">Il repository di prodotto contiene tutto il codice di accesso ai dati per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="708c8-115">The product repository contains all of the data access code for the application.</span></span> <span data-ttu-id="708c8-116">L'elenco include anche l'interfaccia IProductRepository che implementa il repository di prodotto.</span><span class="sxs-lookup"><span data-stu-id="708c8-116">The listing also includes the IProductRepository interface that the product repository implements.</span></span>

<span data-ttu-id="708c8-117">**Elenco 1 - Models\ProductRepository.vb**</span><span class="sxs-lookup"><span data-stu-id="708c8-117">**Listing 1 - Models\ProductRepository.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample1.vb)]

<span data-ttu-id="708c8-118">Il controller nel listato 2 utilizza il livello di repository in entrambe le proprietà Index () e il metodo di creazione di azioni.</span><span class="sxs-lookup"><span data-stu-id="708c8-118">The controller in Listing 2 uses the repository layer in both its Index() and Create() actions.</span></span> <span data-ttu-id="708c8-119">Si noti che il controller non contiene alcuna logica di database.</span><span class="sxs-lookup"><span data-stu-id="708c8-119">Notice that this controller does not contain any database logic.</span></span> <span data-ttu-id="708c8-120">Creazione di un livello di repository consente di mantenere una netta separazione delle problematiche.</span><span class="sxs-lookup"><span data-stu-id="708c8-120">Creating a repository layer enables you to maintain a clean separation of concerns.</span></span> <span data-ttu-id="708c8-121">I controller sono responsabili della logica di controllo di flusso dell'applicazione e il repository è responsabile della logica di accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="708c8-121">Controllers are responsible for application flow control logic and the repository is responsible for data access logic.</span></span>

<span data-ttu-id="708c8-122">**Listing 2 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="708c8-122">**Listing 2 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample2.vb)]

## <a name="creating-a-service-layer"></a><span data-ttu-id="708c8-123">Creazione di un livello di servizio</span><span class="sxs-lookup"><span data-stu-id="708c8-123">Creating a Service Layer</span></span>

<span data-ttu-id="708c8-124">In tal caso, logica di controllo di flusso dell'applicazione fa parte di un controller e logica di accesso ai dati appartiene in un repository.</span><span class="sxs-lookup"><span data-stu-id="708c8-124">So, application flow control logic belongs in a controller and data access logic belongs in a repository.</span></span> <span data-ttu-id="708c8-125">In tal caso, in cui si gestiscono la logica di convalida?</span><span class="sxs-lookup"><span data-stu-id="708c8-125">In that case, where do you put your validation logic?</span></span> <span data-ttu-id="708c8-126">È possibile inserire la logica di convalida in un *livello di servizio*.</span><span class="sxs-lookup"><span data-stu-id="708c8-126">One option is to place your validation logic in a *service layer*.</span></span>

<span data-ttu-id="708c8-127">Un livello di servizio è un ulteriore livello di un'applicazione MVC ASP.NET che consente di eseguire la comunicazione tra un controller e il livello del repository.</span><span class="sxs-lookup"><span data-stu-id="708c8-127">A service layer is an additional layer in an ASP.NET MVC application that mediates communication between a controller and repository layer.</span></span> <span data-ttu-id="708c8-128">Il livello di servizio contiene la logica di business.</span><span class="sxs-lookup"><span data-stu-id="708c8-128">The service layer contains business logic.</span></span> <span data-ttu-id="708c8-129">In particolare, include la logica di convalida.</span><span class="sxs-lookup"><span data-stu-id="708c8-129">In particular, it contains validation logic.</span></span>

<span data-ttu-id="708c8-130">Ad esempio, il livello di servizio prodotto listato 3 ha un metodo CreateProduct().</span><span class="sxs-lookup"><span data-stu-id="708c8-130">For example, the product service layer in Listing 3 has a CreateProduct() method.</span></span> <span data-ttu-id="708c8-131">Il metodo CreateProduct() chiama il metodo ValidateProduct() per convalidare un nuovo prodotto prima di passare il prodotto nel repository di prodotto.</span><span class="sxs-lookup"><span data-stu-id="708c8-131">The CreateProduct() method calls the ValidateProduct() method to validate a new product before passing the product to the product repository.</span></span>

<span data-ttu-id="708c8-132">**Listing 3 - Models\ProductService.vb**</span><span class="sxs-lookup"><span data-stu-id="708c8-132">**Listing 3 - Models\ProductService.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample3.vb)]

<span data-ttu-id="708c8-133">Il controller di prodotto è stato aggiornato nel listato 4 per utilizzare il livello di servizio anziché al livello del repository.</span><span class="sxs-lookup"><span data-stu-id="708c8-133">The Product controller has been updated in Listing 4 to use the service layer instead of the repository layer.</span></span> <span data-ttu-id="708c8-134">Il livello di controller comunica con il livello di servizio.</span><span class="sxs-lookup"><span data-stu-id="708c8-134">The controller layer talks to the service layer.</span></span> <span data-ttu-id="708c8-135">Il livello di servizio comunica con il livello di repository.</span><span class="sxs-lookup"><span data-stu-id="708c8-135">The service layer talks to the repository layer.</span></span> <span data-ttu-id="708c8-136">Ogni livello è una responsabilità separata.</span><span class="sxs-lookup"><span data-stu-id="708c8-136">Each layer has a separate responsibility.</span></span>

<span data-ttu-id="708c8-137">**Listing 4 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="708c8-137">**Listing 4 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample4.vb)]

<span data-ttu-id="708c8-138">Si noti che il servizio di prodotto viene creato nel costruttore controller prodotto.</span><span class="sxs-lookup"><span data-stu-id="708c8-138">Notice that the product service is created in the product controller constructor.</span></span> <span data-ttu-id="708c8-139">Quando viene creato il servizio di prodotto, dizionario di stato del modello viene passato al servizio.</span><span class="sxs-lookup"><span data-stu-id="708c8-139">When the product service is created, the model state dictionary is passed to the service.</span></span> <span data-ttu-id="708c8-140">Il servizio prodotto Usa lo stato del modello per passare i messaggi di errore di convalida al controller.</span><span class="sxs-lookup"><span data-stu-id="708c8-140">The product service uses model state to pass validation error messages back to the controller.</span></span>

## <a name="decoupling-the-service-layer"></a><span data-ttu-id="708c8-141">Separazione il livello di servizio</span><span class="sxs-lookup"><span data-stu-id="708c8-141">Decoupling the Service Layer</span></span>

<span data-ttu-id="708c8-142">Stato non è stato possibile isolare i livelli di servizio per un aspetto e il controller.</span><span class="sxs-lookup"><span data-stu-id="708c8-142">We have failed to isolate the controller and service layers in one respect.</span></span> <span data-ttu-id="708c8-143">Il controller e i livelli di servizio comunicano tramite lo stato del modello.</span><span class="sxs-lookup"><span data-stu-id="708c8-143">The controller and service layers communicate through model state.</span></span> <span data-ttu-id="708c8-144">In altre parole, il livello di servizio presenta una dipendenza su una determinata funzionalità del framework di MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="708c8-144">In other words, the service layer has a dependency on a particular feature of the ASP.NET MVC framework.</span></span>

<span data-ttu-id="708c8-145">Si desidera isolare il livello di servizio di livello il controller quanto possibile.</span><span class="sxs-lookup"><span data-stu-id="708c8-145">We want to isolate the service layer from our controller layer as much as possible.</span></span> <span data-ttu-id="708c8-146">In teoria, è necessario essere in grado di utilizzare il livello di servizio con qualsiasi tipo di applicazione e non solo di un'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="708c8-146">In theory, we should be able to use the service layer with any type of application and not only an ASP.NET MVC application.</span></span> <span data-ttu-id="708c8-147">Ad esempio, in futuro, potrebbe essere opportuno compilare un front-end per l'applicazione WPF.</span><span class="sxs-lookup"><span data-stu-id="708c8-147">For example, in the future, we might want to build a WPF front-end for our application.</span></span> <span data-ttu-id="708c8-148">Occorre individuare un modo per rimuovere la dipendenza su ASP.NET MVC lo stato del modello da questo livello di servizio.</span><span class="sxs-lookup"><span data-stu-id="708c8-148">We should find a way to remove the dependency on ASP.NET MVC model state from our service layer.</span></span>

<span data-ttu-id="708c8-149">Nel listato 5, il livello di servizio è stato aggiornato in modo che non utilizzi più lo stato del modello.</span><span class="sxs-lookup"><span data-stu-id="708c8-149">In Listing 5, the service layer has been updated so that it no longer uses model state.</span></span> <span data-ttu-id="708c8-150">Utilizza invece qualsiasi classe che implementa l'interfaccia IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="708c8-150">Instead, it uses any class that implements the IValidationDictionary interface.</span></span>

<span data-ttu-id="708c8-151">**Listing 5 - Models\ProductService.vb (decoupled)**</span><span class="sxs-lookup"><span data-stu-id="708c8-151">**Listing 5 - Models\ProductService.vb (decoupled)**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample5.vb)]

<span data-ttu-id="708c8-152">L'interfaccia IValidationDictionary è definito nel listato 6.</span><span class="sxs-lookup"><span data-stu-id="708c8-152">The IValidationDictionary interface is defined in Listing 6.</span></span> <span data-ttu-id="708c8-153">Questa semplice interfaccia dispone di un singolo metodo e una singola proprietà.</span><span class="sxs-lookup"><span data-stu-id="708c8-153">This simple interface has a single method and a single property.</span></span>

<span data-ttu-id="708c8-154">**Elenco 6 - Models\IValidationDictionary.cs**</span><span class="sxs-lookup"><span data-stu-id="708c8-154">**Listing 6 - Models\IValidationDictionary.cs**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample6.vb)]

<span data-ttu-id="708c8-155">La classe nel listato 7, la classe ModelStateWrapper, denominato implementa l'interfaccia IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="708c8-155">The class in Listing 7, named the ModelStateWrapper class, implements the IValidationDictionary interface.</span></span> <span data-ttu-id="708c8-156">È possibile creare un'istanza della classe ModelStateWrapper passando un dizionario di stato del modello al costruttore.</span><span class="sxs-lookup"><span data-stu-id="708c8-156">You can instantiate the ModelStateWrapper class by passing a model state dictionary to the constructor.</span></span>

<span data-ttu-id="708c8-157">**Elenco 7 - Models\ModelStateWrapper.vb**</span><span class="sxs-lookup"><span data-stu-id="708c8-157">**Listing 7 - Models\ModelStateWrapper.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample7.vb)]

<span data-ttu-id="708c8-158">Infine, il controller aggiornato nel listato 8 Usa il ModelStateWrapper quando si crea il livello di servizio nel relativo costruttore.</span><span class="sxs-lookup"><span data-stu-id="708c8-158">Finally, the updated controller in Listing 8 uses the ModelStateWrapper when creating the service layer in its constructor.</span></span>

<span data-ttu-id="708c8-159">**Elenco 8 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="708c8-159">**Listing 8 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample8.vb)]

<span data-ttu-id="708c8-160">Utilizzando il IValidationDictionary interfaccia e la classe ModelStateWrapper consente di isolare completamente il livello di servizio di livello il controller.</span><span class="sxs-lookup"><span data-stu-id="708c8-160">Using the IValidationDictionary interface and the ModelStateWrapper class enables us to completely isolate our service layer from our controller layer.</span></span> <span data-ttu-id="708c8-161">Il livello di servizio non è più dipendente sullo stato del modello.</span><span class="sxs-lookup"><span data-stu-id="708c8-161">The service layer is no longer dependent on model state.</span></span> <span data-ttu-id="708c8-162">È possibile passare qualsiasi classe che implementa l'interfaccia IValidationDictionary a livello di servizio.</span><span class="sxs-lookup"><span data-stu-id="708c8-162">You can pass any class that implements the IValidationDictionary interface to the service layer.</span></span> <span data-ttu-id="708c8-163">Ad esempio, un'applicazione WPF potrebbe implementare l'interfaccia IValidationDictionary con una classe collection semplice.</span><span class="sxs-lookup"><span data-stu-id="708c8-163">For example, a WPF application might implement the IValidationDictionary interface with a simple collection class.</span></span>

## <a name="summary"></a><span data-ttu-id="708c8-164">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="708c8-164">Summary</span></span>

<span data-ttu-id="708c8-165">L'obiettivo di questa esercitazione è stata per illustrare un approccio per eseguire la convalida in un'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="708c8-165">The goal of this tutorial was to discuss one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="708c8-166">In questa esercitazione è stato descritto come spostare tutta la logica di convalida, i controller e in un livello di servizio separato.</span><span class="sxs-lookup"><span data-stu-id="708c8-166">In this tutorial, you learned how to move all of your validation logic out of your controllers and into a separate service layer.</span></span> <span data-ttu-id="708c8-167">È stato inoltre descritto come isolare il livello di servizio dal livello del controller tramite la creazione di una classe ModelStateWrapper.</span><span class="sxs-lookup"><span data-stu-id="708c8-167">You also learned how to isolate your service layer from your controller layer by creating a ModelStateWrapper class.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="708c8-168">[Precedente](validating-with-the-idataerrorinfo-interface-vb.md)
> [Successivo](validation-with-the-data-annotation-validators-vb.md)</span><span class="sxs-lookup"><span data-stu-id="708c8-168">[Previous](validating-with-the-idataerrorinfo-interface-vb.md)
[Next](validation-with-the-data-annotation-validators-vb.md)</span></span>
