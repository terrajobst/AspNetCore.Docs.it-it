---
title: Introduzione a ASP.NET di base e di Entity Framework 6
author: tdykstra
description: In questo articolo viene illustrato come utilizzare Entity Framework 6 in un'applicazione ASP.NET Core.
keywords: Entity Framework di ASP.NET Core, Entity Framework 6
ms.author: tdykstra
manager: wpickett
ms.date: 02/24/2017
ms.topic: article
ms.assetid: 016cc836-4c43-45a4-b9a7-9efaf53350df
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/entity-framework-6
ms.openlocfilehash: 8abec95c591f20069e20eec55fd21503e74f8606
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a><span data-ttu-id="97c03-104">Introduzione a ASP.NET di base e di Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="97c03-104">Getting started with ASP.NET Core and Entity Framework 6</span></span>

<span data-ttu-id="97c03-105">Da [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="97c03-105">By [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="97c03-106">In questo articolo viene illustrato come utilizzare Entity Framework 6 in un'applicazione ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="97c03-106">This article shows how to use Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="overview"></a><span data-ttu-id="97c03-107">Panoramica</span><span class="sxs-lookup"><span data-stu-id="97c03-107">Overview</span></span>

<span data-ttu-id="97c03-108">Per l'utilizzo di Entity Framework 6, il progetto deve eseguire la compilazione con .NET Framework, come Entity Framework 6 non supporta .NET Core.</span><span class="sxs-lookup"><span data-stu-id="97c03-108">To use Entity Framework 6, your project has to compile against .NET Framework, as Entity Framework 6 does not support .NET Core.</span></span> <span data-ttu-id="97c03-109">Se sono necessarie funzionalità multipiattaforma, è necessario eseguire l'aggiornamento a [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="97c03-109">If you need cross-platform features you will need to upgrade to [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

<span data-ttu-id="97c03-110">Il metodo consigliato per l'utilizzo di Entity Framework 6 in un'applicazione ASP.NET di base consiste nell'inserire il contesto EF6 e classi del modello in una libreria di classi del progetto che fa riferimento il framework completo.</span><span class="sxs-lookup"><span data-stu-id="97c03-110">The recommended way to use Entity Framework 6 in an ASP.NET Core application is to put the EF6 context and model classes in a class library project that targets the full framework.</span></span> <span data-ttu-id="97c03-111">Aggiungere un riferimento alla libreria di classi dal progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="97c03-111">Add a reference to the class library from the ASP.NET Core project.</span></span> <span data-ttu-id="97c03-112">Vedere l'esempio [soluzione di Visual Studio con progetti EF6 e ASP.NET Core](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span><span class="sxs-lookup"><span data-stu-id="97c03-112">See the sample [Visual Studio solution with EF6 and ASP.NET Core projects](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span></span>

<span data-ttu-id="97c03-113">Impossibile inserire un contesto EF6 in un progetto ASP.NET Core perché i progetti .NET Core non supportano tutte le funzionalità, ad esempio i comandi di EF6 *Enable-Migrations* richiedono.</span><span class="sxs-lookup"><span data-stu-id="97c03-113">You can't put an EF6 context in an ASP.NET Core project because .NET Core projects don't support all of the functionality that EF6 commands such as *Enable-Migrations* require.</span></span>

<span data-ttu-id="97c03-114">Indipendentemente dal tipo di progetto in cui è individuare il contesto EF6, strumenti da riga di comando di EF6 utilizzano un contesto EF6.</span><span class="sxs-lookup"><span data-stu-id="97c03-114">Regardless of project type in which you locate your EF6 context, only EF6 command-line tools work with an EF6 context.</span></span> <span data-ttu-id="97c03-115">Ad esempio, `Scaffold-DbContext` è disponibile solo in Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="97c03-115">For example, `Scaffold-DbContext` is only available in Entity Framework Core.</span></span> <span data-ttu-id="97c03-116">Se è necessario il reverse engineering di un database in un modello EF6, vedere [Code First per un Database esistente](https://msdn.microsoft.com/jj200620).</span><span class="sxs-lookup"><span data-stu-id="97c03-116">If you need to do reverse engineering of a database into an EF6 model, see [Code First to an Existing Database](https://msdn.microsoft.com/jj200620).</span></span>

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a><span data-ttu-id="97c03-117">Framework completo di riferimento ed EF6 nel progetto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="97c03-117">Reference full framework and EF6 in the ASP.NET Core project</span></span>

<span data-ttu-id="97c03-118">Il progetto ASP.NET Core deve fare riferimento a .NET framework ed EF6.</span><span class="sxs-lookup"><span data-stu-id="97c03-118">Your ASP.NET Core project needs to reference .NET framework and EF6.</span></span> <span data-ttu-id="97c03-119">Ad esempio, il *csproj* file del progetto ASP.NET Core avrà un aspetto simile al seguente (sono visualizzate solo le parti pertinenti del file).</span><span class="sxs-lookup"><span data-stu-id="97c03-119">For example, the *.csproj* file of your ASP.NET Core project will look similar to the following example (only relevant parts of the file are shown).</span></span>

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

<span data-ttu-id="97c03-120">Se si crea un nuovo progetto, utilizzare il **applicazione Web di ASP.NET Core (.NET Framework)** modello.</span><span class="sxs-lookup"><span data-stu-id="97c03-120">If you’re creating a new project, use the **ASP.NET Core Web Application (.NET Framework)** template.</span></span>

## <a name="handle-connection-strings"></a><span data-ttu-id="97c03-121">Gestire le stringhe di connessione</span><span class="sxs-lookup"><span data-stu-id="97c03-121">Handle connection strings</span></span>

<span data-ttu-id="97c03-122">Gli strumenti da riga di comando EF6 che verranno utilizzati nel progetto di libreria di classi EF6 richiedono un costruttore predefinito, quindi è possibile creare un'istanza del contesto.</span><span class="sxs-lookup"><span data-stu-id="97c03-122">The EF6 command-line tools that you'll use in the EF6 class library project require a default constructor so they can instantiate the context.</span></span> <span data-ttu-id="97c03-123">Ma è opportuno specificare la stringa di connessione da utilizzare nel progetto ASP.NET Core, nel qual caso il costruttore di contesto deve avere un parametro che consente di passare la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="97c03-123">But you'll probably want to specify the connection string to use in the ASP.NET Core project, in which case your context constructor must have a parameter that lets you pass in the connection string.</span></span> <span data-ttu-id="97c03-124">Di seguito è riportato un esempio.</span><span class="sxs-lookup"><span data-stu-id="97c03-124">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

<span data-ttu-id="97c03-125">Poiché il contesto EF6 non dispone di un costruttore senza parametri, deve fornire un'implementazione di progetto EF6 [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span><span class="sxs-lookup"><span data-stu-id="97c03-125">Since your EF6 context doesn't have a parameterless constructor, your EF6 project has to provide an implementation of [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span></span> <span data-ttu-id="97c03-126">Gli strumenti da riga di comando EF6 verranno trovare e utilizzare tale implementazione, pertanto è possibile creare un'istanza del contesto.</span><span class="sxs-lookup"><span data-stu-id="97c03-126">The EF6 command-line tools will find and use that implementation so they can instantiate the context.</span></span> <span data-ttu-id="97c03-127">Di seguito è riportato un esempio.</span><span class="sxs-lookup"><span data-stu-id="97c03-127">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

<span data-ttu-id="97c03-128">In questo codice di esempio, il `IDbContextFactory` implementazione passa una stringa di connessione hardcoded.</span><span class="sxs-lookup"><span data-stu-id="97c03-128">In this sample code, the `IDbContextFactory` implementation passes in a hard-coded connection string.</span></span> <span data-ttu-id="97c03-129">Questa è la stringa di connessione che utilizzeranno gli strumenti da riga di comando.</span><span class="sxs-lookup"><span data-stu-id="97c03-129">This is the connection string that the command-line tools will use.</span></span> <span data-ttu-id="97c03-130">È opportuno implementare una strategia per garantire che la libreria di classi Usa la stessa stringa di connessione utilizzata dall'applicazione chiamante.</span><span class="sxs-lookup"><span data-stu-id="97c03-130">You'll want to implement a strategy to ensure that the class library uses the same connection string that the calling application uses.</span></span> <span data-ttu-id="97c03-131">Ad esempio, è possibile ottenere il valore dalla variabile di ambiente in entrambi i progetti.</span><span class="sxs-lookup"><span data-stu-id="97c03-131">For example, you could get the value from an environment variable in both projects.</span></span>

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a><span data-ttu-id="97c03-132">Impostare l'inserimento di dipendenze del progetto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="97c03-132">Set up dependency injection in the ASP.NET Core project</span></span>

<span data-ttu-id="97c03-133">Del progetto principale *Startup.cs* file, impostare il contesto di EF6 per l'inserimento di dipendenze (DI) in `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="97c03-133">In the Core project's *Startup.cs* file, set up the EF6 context for dependency injection (DI) in `ConfigureServices`.</span></span> <span data-ttu-id="97c03-134">Gli oggetti di contesto di Entity Framework devono essere limitati per una durata di ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="97c03-134">EF context objects should be scoped for a per-request lifetime.</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

<span data-ttu-id="97c03-135">È quindi possibile ottenere un'istanza del contesto nei controller di usando DI.</span><span class="sxs-lookup"><span data-stu-id="97c03-135">You can then get an instance of the context in your controllers by using DI.</span></span> <span data-ttu-id="97c03-136">Il codice è simile a si scriverebbe per un contesto di EF Core:</span><span class="sxs-lookup"><span data-stu-id="97c03-136">The code is similar to what you'd write for an EF Core context:</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a><span data-ttu-id="97c03-137">Applicazione di esempio</span><span class="sxs-lookup"><span data-stu-id="97c03-137">Sample application</span></span>

<span data-ttu-id="97c03-138">Per un'applicazione di esempio funzionante, vedere il [soluzione di Visual Studio di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) che accompagna questo articolo.</span><span class="sxs-lookup"><span data-stu-id="97c03-138">For a working sample application, see the [sample Visual Studio solution](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) that accompanies this article.</span></span>

<span data-ttu-id="97c03-139">In questo esempio può essere creato da zero per i passaggi seguenti in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="97c03-139">This sample can be created from scratch by the following steps in Visual Studio:</span></span>

* <span data-ttu-id="97c03-140">Creare una soluzione.</span><span class="sxs-lookup"><span data-stu-id="97c03-140">Create a solution.</span></span>

* <span data-ttu-id="97c03-141">**Aggiungi nuovo progetto > Web > applicazione Web ASP.NET Core (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="97c03-141">**Add New Project > Web > ASP.NET Core Web Application (.NET Framework)**</span></span>

* <span data-ttu-id="97c03-142">**Aggiungi nuovo progetto > Windows Desktop classico > (.NET Framework) libreria di classi**</span><span class="sxs-lookup"><span data-stu-id="97c03-142">**Add New Project > Windows Classic Desktop > Class Library (.NET Framework)**</span></span>

* <span data-ttu-id="97c03-143">In **Package Manager Console** (PMC) per entrambi i progetti, eseguire il comando `Install-Package Entityframework`.</span><span class="sxs-lookup"><span data-stu-id="97c03-143">In **Package Manager Console** (PMC) for both projects, run the command `Install-Package Entityframework`.</span></span>

* <span data-ttu-id="97c03-144">Nel progetto libreria di classi, creare le classi di modello di dati e una classe del contesto e un'implementazione di `IDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="97c03-144">In the class library project, create data model classes and a context class, and an implementation of `IDbContextFactory`.</span></span>

* <span data-ttu-id="97c03-145">In PMC per il progetto libreria di classi, eseguire i comandi `Enable-Migrations` e `Add-Migration Initial`.</span><span class="sxs-lookup"><span data-stu-id="97c03-145">In PMC for the class library project, run the commands `Enable-Migrations` and `Add-Migration Initial`.</span></span> <span data-ttu-id="97c03-146">Se il progetto ASP.NET Core è stata impostata come progetto di avvio, aggiungere `-StartupProjectName EF6` a questi comandi.</span><span class="sxs-lookup"><span data-stu-id="97c03-146">If you have set the ASP.NET Core project as the startup project, add `-StartupProjectName EF6` to these commands.</span></span>

* <span data-ttu-id="97c03-147">Nel progetto di base, aggiungere un riferimento progetto a progetto libreria di classi.</span><span class="sxs-lookup"><span data-stu-id="97c03-147">In the Core project, add a project reference to the class library project.</span></span>

* <span data-ttu-id="97c03-148">Nel progetto di base, in *Startup.cs*, registrare il contesto per DI.</span><span class="sxs-lookup"><span data-stu-id="97c03-148">In the Core project, in *Startup.cs*, register the context for DI.</span></span>

* <span data-ttu-id="97c03-149">Nel progetto di base, in *appSettings. JSON*, aggiungere la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="97c03-149">In the Core project, in *appsettings.json*, add the connection string.</span></span>

* <span data-ttu-id="97c03-150">Nel progetto di base, aggiungere un controller e visualizzazioni per verificare che è possibile leggere e scrivere i dati.</span><span class="sxs-lookup"><span data-stu-id="97c03-150">In the Core project, add a controller and view(s) to verify that you can read and write data.</span></span> <span data-ttu-id="97c03-151">Si noti che lo scaffolding di ASP.NET MVC di base non funzionerà con il contesto EF6 a cui fa riferimento della libreria di classi.</span><span class="sxs-lookup"><span data-stu-id="97c03-151">(Note that ASP.NET Core MVC scaffolding won't work with the EF6 context referenced from the class library.)</span></span>

## <a name="summary"></a><span data-ttu-id="97c03-152">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="97c03-152">Summary</span></span>

<span data-ttu-id="97c03-153">Questo articolo sono presenti linee guida di base per l'utilizzo di Entity Framework 6 in un'applicazione ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="97c03-153">This article has provided basic guidance for using Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="97c03-154">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="97c03-154">Additional Resources</span></span>

* [<span data-ttu-id="97c03-155">Entity Framework - configurazione basata su codice</span><span class="sxs-lookup"><span data-stu-id="97c03-155">Entity Framework - Code-Based Configuration</span></span>](https://msdn.microsoft.com/data/jj680699.aspx)
