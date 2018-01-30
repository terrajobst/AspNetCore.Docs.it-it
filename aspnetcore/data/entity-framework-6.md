---
title: Introduzione a ASP.NET di base e di Entity Framework 6
author: tdykstra
description: In questo articolo viene illustrato come utilizzare Entity Framework 6 in un'applicazione ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.date: 02/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: data/entity-framework-6
ms.openlocfilehash: 7407fe8a976978d7d5077d5e5ac6cc264565621d
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a><span data-ttu-id="578d6-103">Introduzione a ASP.NET di base e di Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="578d6-103">Getting started with ASP.NET Core and Entity Framework 6</span></span>

<span data-ttu-id="578d6-104">Da [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="578d6-104">By [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="578d6-105">In questo articolo viene illustrato come utilizzare Entity Framework 6 in un'applicazione ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="578d6-105">This article shows how to use Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="overview"></a><span data-ttu-id="578d6-106">Panoramica</span><span class="sxs-lookup"><span data-stu-id="578d6-106">Overview</span></span>

<span data-ttu-id="578d6-107">Per l'utilizzo di Entity Framework 6, il progetto deve eseguire la compilazione con .NET Framework, come Entity Framework 6 non supporta .NET Core.</span><span class="sxs-lookup"><span data-stu-id="578d6-107">To use Entity Framework 6, your project has to compile against .NET Framework, as Entity Framework 6 doesn't support .NET Core.</span></span> <span data-ttu-id="578d6-108">Se sono necessarie funzionalità multipiattaforma, è necessario eseguire l'aggiornamento a [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="578d6-108">If you need cross-platform features you will need to upgrade to [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

<span data-ttu-id="578d6-109">Il metodo consigliato per l'utilizzo di Entity Framework 6 in un'applicazione ASP.NET di base consiste nell'inserire il contesto EF6 e classi del modello in una libreria di classi del progetto che fa riferimento il framework completo.</span><span class="sxs-lookup"><span data-stu-id="578d6-109">The recommended way to use Entity Framework 6 in an ASP.NET Core application is to put the EF6 context and model classes in a class library project that targets the full framework.</span></span> <span data-ttu-id="578d6-110">Aggiungere un riferimento alla libreria di classi dal progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="578d6-110">Add a reference to the class library from the ASP.NET Core project.</span></span> <span data-ttu-id="578d6-111">Vedere l'esempio [soluzione di Visual Studio con progetti EF6 e ASP.NET Core](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span><span class="sxs-lookup"><span data-stu-id="578d6-111">See the sample [Visual Studio solution with EF6 and ASP.NET Core projects](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span></span>

<span data-ttu-id="578d6-112">Impossibile inserire un contesto EF6 in un progetto ASP.NET Core perché i progetti .NET Core non supportano tutte le funzionalità, ad esempio i comandi di EF6 *Enable-Migrations* richiedono.</span><span class="sxs-lookup"><span data-stu-id="578d6-112">You can't put an EF6 context in an ASP.NET Core project because .NET Core projects don't support all of the functionality that EF6 commands such as *Enable-Migrations* require.</span></span>

<span data-ttu-id="578d6-113">Indipendentemente dal tipo di progetto in cui è individuare il contesto EF6, strumenti da riga di comando di EF6 utilizzano un contesto EF6.</span><span class="sxs-lookup"><span data-stu-id="578d6-113">Regardless of project type in which you locate your EF6 context, only EF6 command-line tools work with an EF6 context.</span></span> <span data-ttu-id="578d6-114">Ad esempio, `Scaffold-DbContext` è disponibile solo in Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="578d6-114">For example, `Scaffold-DbContext` is only available in Entity Framework Core.</span></span> <span data-ttu-id="578d6-115">Se è necessario il reverse engineering di un database in un modello EF6, vedere [Code First per un Database esistente](https://msdn.microsoft.com/jj200620).</span><span class="sxs-lookup"><span data-stu-id="578d6-115">If you need to do reverse engineering of a database into an EF6 model, see [Code First to an Existing Database](https://msdn.microsoft.com/jj200620).</span></span>

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a><span data-ttu-id="578d6-116">Framework completo di riferimento ed EF6 nel progetto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="578d6-116">Reference full framework and EF6 in the ASP.NET Core project</span></span>

<span data-ttu-id="578d6-117">Il progetto ASP.NET Core deve fare riferimento a .NET framework ed EF6.</span><span class="sxs-lookup"><span data-stu-id="578d6-117">Your ASP.NET Core project needs to reference .NET framework and EF6.</span></span> <span data-ttu-id="578d6-118">Ad esempio, il *csproj* file del progetto ASP.NET Core avrà un aspetto simile al seguente (sono visualizzate solo le parti pertinenti del file).</span><span class="sxs-lookup"><span data-stu-id="578d6-118">For example, the *.csproj* file of your ASP.NET Core project will look similar to the following example (only relevant parts of the file are shown).</span></span>

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

<span data-ttu-id="578d6-119">Quando si crea un nuovo progetto, utilizzare il **applicazione Web di ASP.NET Core (.NET Framework)** modello.</span><span class="sxs-lookup"><span data-stu-id="578d6-119">When creating a new project, use the **ASP.NET Core Web Application (.NET Framework)** template.</span></span>

## <a name="handle-connection-strings"></a><span data-ttu-id="578d6-120">Gestire le stringhe di connessione</span><span class="sxs-lookup"><span data-stu-id="578d6-120">Handle connection strings</span></span>

<span data-ttu-id="578d6-121">Gli strumenti da riga di comando EF6 che verranno utilizzati nel progetto di libreria di classi EF6 richiedono un costruttore predefinito, quindi è possibile creare un'istanza del contesto.</span><span class="sxs-lookup"><span data-stu-id="578d6-121">The EF6 command-line tools that you'll use in the EF6 class library project require a default constructor so they can instantiate the context.</span></span> <span data-ttu-id="578d6-122">Ma è opportuno specificare la stringa di connessione da utilizzare nel progetto ASP.NET Core, nel qual caso il costruttore di contesto deve avere un parametro che consente di passare la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="578d6-122">But you'll probably want to specify the connection string to use in the ASP.NET Core project, in which case your context constructor must have a parameter that lets you pass in the connection string.</span></span> <span data-ttu-id="578d6-123">Di seguito è riportato un esempio.</span><span class="sxs-lookup"><span data-stu-id="578d6-123">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

<span data-ttu-id="578d6-124">Poiché il contesto EF6 non dispone di un costruttore senza parametri, deve fornire un'implementazione di progetto EF6 [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span><span class="sxs-lookup"><span data-stu-id="578d6-124">Since your EF6 context doesn't have a parameterless constructor, your EF6 project has to provide an implementation of [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span></span> <span data-ttu-id="578d6-125">Gli strumenti da riga di comando EF6 verranno trovare e utilizzare tale implementazione, pertanto è possibile creare un'istanza del contesto.</span><span class="sxs-lookup"><span data-stu-id="578d6-125">The EF6 command-line tools will find and use that implementation so they can instantiate the context.</span></span> <span data-ttu-id="578d6-126">Di seguito è riportato un esempio.</span><span class="sxs-lookup"><span data-stu-id="578d6-126">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

<span data-ttu-id="578d6-127">In questo codice di esempio, il `IDbContextFactory` implementazione passa una stringa di connessione hardcoded.</span><span class="sxs-lookup"><span data-stu-id="578d6-127">In this sample code, the `IDbContextFactory` implementation passes in a hard-coded connection string.</span></span> <span data-ttu-id="578d6-128">Questa è la stringa di connessione che utilizzeranno gli strumenti da riga di comando.</span><span class="sxs-lookup"><span data-stu-id="578d6-128">This is the connection string that the command-line tools will use.</span></span> <span data-ttu-id="578d6-129">È opportuno implementare una strategia per garantire che la libreria di classi Usa la stessa stringa di connessione utilizzata dall'applicazione chiamante.</span><span class="sxs-lookup"><span data-stu-id="578d6-129">You'll want to implement a strategy to ensure that the class library uses the same connection string that the calling application uses.</span></span> <span data-ttu-id="578d6-130">Ad esempio, è possibile ottenere il valore dalla variabile di ambiente in entrambi i progetti.</span><span class="sxs-lookup"><span data-stu-id="578d6-130">For example, you could get the value from an environment variable in both projects.</span></span>

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a><span data-ttu-id="578d6-131">Impostare l'inserimento di dipendenze del progetto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="578d6-131">Set up dependency injection in the ASP.NET Core project</span></span>

<span data-ttu-id="578d6-132">Del progetto principale *Startup.cs* file, impostare il contesto di EF6 per l'inserimento di dipendenze (DI) in `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="578d6-132">In the Core project's *Startup.cs* file, set up the EF6 context for dependency injection (DI) in `ConfigureServices`.</span></span> <span data-ttu-id="578d6-133">Gli oggetti di contesto di Entity Framework devono essere limitati per una durata di ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="578d6-133">EF context objects should be scoped for a per-request lifetime.</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

<span data-ttu-id="578d6-134">È quindi possibile ottenere un'istanza del contesto nei controller di usando DI.</span><span class="sxs-lookup"><span data-stu-id="578d6-134">You can then get an instance of the context in your controllers by using DI.</span></span> <span data-ttu-id="578d6-135">Il codice è simile a si scriverebbe per un contesto di EF Core:</span><span class="sxs-lookup"><span data-stu-id="578d6-135">The code is similar to what you'd write for an EF Core context:</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a><span data-ttu-id="578d6-136">Applicazione di esempio</span><span class="sxs-lookup"><span data-stu-id="578d6-136">Sample application</span></span>

<span data-ttu-id="578d6-137">Per un'applicazione di esempio funzionante, vedere il [soluzione di Visual Studio di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) che accompagna questo articolo.</span><span class="sxs-lookup"><span data-stu-id="578d6-137">For a working sample application, see the [sample Visual Studio solution](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) that accompanies this article.</span></span>

<span data-ttu-id="578d6-138">In questo esempio può essere creato da zero per i passaggi seguenti in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="578d6-138">This sample can be created from scratch by the following steps in Visual Studio:</span></span>

* <span data-ttu-id="578d6-139">Creare una soluzione.</span><span class="sxs-lookup"><span data-stu-id="578d6-139">Create a solution.</span></span>

* <span data-ttu-id="578d6-140">**Aggiungi nuovo progetto > Web > applicazione Web ASP.NET Core (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="578d6-140">**Add New Project > Web > ASP.NET Core Web Application (.NET Framework)**</span></span>

* <span data-ttu-id="578d6-141">**Aggiungi nuovo progetto > Windows Desktop classico > (.NET Framework) libreria di classi**</span><span class="sxs-lookup"><span data-stu-id="578d6-141">**Add New Project > Windows Classic Desktop > Class Library (.NET Framework)**</span></span>

* <span data-ttu-id="578d6-142">In **Package Manager Console** (PMC) per entrambi i progetti, eseguire il comando `Install-Package Entityframework`.</span><span class="sxs-lookup"><span data-stu-id="578d6-142">In **Package Manager Console** (PMC) for both projects, run the command `Install-Package Entityframework`.</span></span>

* <span data-ttu-id="578d6-143">Nel progetto libreria di classi, creare le classi di modello di dati e una classe del contesto e un'implementazione di `IDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="578d6-143">In the class library project, create data model classes and a context class, and an implementation of `IDbContextFactory`.</span></span>

* <span data-ttu-id="578d6-144">In PMC per il progetto libreria di classi, eseguire i comandi `Enable-Migrations` e `Add-Migration Initial`.</span><span class="sxs-lookup"><span data-stu-id="578d6-144">In PMC for the class library project, run the commands `Enable-Migrations` and `Add-Migration Initial`.</span></span> <span data-ttu-id="578d6-145">Se il progetto ASP.NET Core è stata impostata come progetto di avvio, aggiungere `-StartupProjectName EF6` a questi comandi.</span><span class="sxs-lookup"><span data-stu-id="578d6-145">If you have set the ASP.NET Core project as the startup project, add `-StartupProjectName EF6` to these commands.</span></span>

* <span data-ttu-id="578d6-146">Nel progetto di base, aggiungere un riferimento progetto a progetto libreria di classi.</span><span class="sxs-lookup"><span data-stu-id="578d6-146">In the Core project, add a project reference to the class library project.</span></span>

* <span data-ttu-id="578d6-147">Nel progetto di base, in *Startup.cs*, registrare il contesto per DI.</span><span class="sxs-lookup"><span data-stu-id="578d6-147">In the Core project, in *Startup.cs*, register the context for DI.</span></span>

* <span data-ttu-id="578d6-148">Nel progetto di base, in *appSettings. JSON*, aggiungere la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="578d6-148">In the Core project, in *appsettings.json*, add the connection string.</span></span>

* <span data-ttu-id="578d6-149">Nel progetto di base, aggiungere un controller e visualizzazioni per verificare che è possibile leggere e scrivere i dati.</span><span class="sxs-lookup"><span data-stu-id="578d6-149">In the Core project, add a controller and view(s) to verify that you can read and write data.</span></span> <span data-ttu-id="578d6-150">Si noti che lo scaffolding di ASP.NET MVC di base non funzionerà con il contesto EF6 a cui fa riferimento della libreria di classi.</span><span class="sxs-lookup"><span data-stu-id="578d6-150">(Note that ASP.NET Core MVC scaffolding won't work with the EF6 context referenced from the class library.)</span></span>

## <a name="summary"></a><span data-ttu-id="578d6-151">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="578d6-151">Summary</span></span>

<span data-ttu-id="578d6-152">Questo articolo sono presenti linee guida di base per l'utilizzo di Entity Framework 6 in un'applicazione ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="578d6-152">This article has provided basic guidance for using Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="578d6-153">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="578d6-153">Additional resources</span></span>

* [<span data-ttu-id="578d6-154">Entity Framework - configurazione basata su codice</span><span class="sxs-lookup"><span data-stu-id="578d6-154">Entity Framework - Code-Based Configuration</span></span>](https://msdn.microsoft.com/data/jj680699.aspx)
