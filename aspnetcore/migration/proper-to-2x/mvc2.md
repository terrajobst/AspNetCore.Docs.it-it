---
title: La migrazione da ASP.NET per ASP.NET 2.0 Core
author: isaac2004
description: In questo documento fornisce materiale sussidiario per migrazione ASP.NET MVC o Web API delle applicazioni esistenti per ASP.NET 2.0 Core.
keywords: La migrazione di componenti di base, MVC ASP.NET
ms.author: scaddie
manager: wpickett
ms.date: 08/27/2017
ms.topic: article
ms.assetid: 3155cc9e-d0c9-424b-886c-35c0ec6f9f4e
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc2
ms.openlocfilehash: e0691b276b63ee12d3163ac48d1392696fb97aa6
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="migrating-from-aspnet-to-aspnet-core-20"></a><span data-ttu-id="28d78-104">La migrazione da ASP.NET per ASP.NET 2.0 Core</span><span class="sxs-lookup"><span data-stu-id="28d78-104">Migrating From ASP.NET to ASP.NET Core 2.0</span></span>

<span data-ttu-id="28d78-105">Da [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="28d78-105">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="28d78-106">In questo articolo viene utilizzato come una Guida di riferimento per la migrazione delle applicazioni ASP.NET ad ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="28d78-106">This article serves as a reference guide for migrating ASP.NET applications to ASP.NET Core 2.0.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="28d78-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="28d78-107">Prerequisites</span></span>

* <span data-ttu-id="28d78-108">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="28d78-108">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

## <a name="target-frameworks"></a><span data-ttu-id="28d78-109">Framework di destinazione</span><span class="sxs-lookup"><span data-stu-id="28d78-109">Target Frameworks</span></span>
<span data-ttu-id="28d78-110">Progetti di componenti di base di ASP.NET 2.0 offrono agli sviluppatori la flessibilità di destinazione di .NET Core, .NET Framework o entrambi.</span><span class="sxs-lookup"><span data-stu-id="28d78-110">ASP.NET Core 2.0 projects offer developers the flexibility of targeting .NET Core, .NET Framework, or both.</span></span> <span data-ttu-id="28d78-111">Vedere [scelta tra .NET Core e .NET Framework per applicazioni server](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) per determinare quali framework di destinazione è più appropriato.</span><span class="sxs-lookup"><span data-stu-id="28d78-111">See [Choosing between .NET Core and .NET Framework for server apps](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) to determine which target framework is most appropriate.</span></span>

<span data-ttu-id="28d78-112">Quando la destinazione è .NET Framework, è necessario fare riferimento a singoli pacchetti NuGet progetti.</span><span class="sxs-lookup"><span data-stu-id="28d78-112">When targeting .NET Framework, projects need to reference individual NuGet packages.</span></span>

<span data-ttu-id="28d78-113">Destinato a .NET Core consente di eliminare numerosi riferimenti pacchetto esplicita, grazie ai componenti di base di ASP.NET 2.0 [metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="28d78-113">Targeting .NET Core allows you to eliminate numerous explicit package references, thanks to the ASP.NET Core 2.0 [metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="28d78-114">Installare il `Microsoft.AspNetCore.All` metapackage nel progetto:</span><span class="sxs-lookup"><span data-stu-id="28d78-114">Install the `Microsoft.AspNetCore.All` metapackage in your project:</span></span>

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

<span data-ttu-id="28d78-115">Quando viene utilizzato il metapackage, nessun pacchetto a cui fa riferimento il metapackage viene distribuito con l'app.</span><span class="sxs-lookup"><span data-stu-id="28d78-115">When the metapackage is used, no packages referenced in the metapackage are deployed with the app.</span></span> <span data-ttu-id="28d78-116">L'archivio di Runtime .NET Core include queste risorse, e sono precompilati per migliorare le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="28d78-116">The .NET Core Runtime Store includes these assets, and they are precompiled to improve performance.</span></span> <span data-ttu-id="28d78-117">Vedere [metapackage Microsoft.AspNetCore.All per ASP.NET Core 2. x](xref:fundamentals/metapackage) per ulteriori dettagli.</span><span class="sxs-lookup"><span data-stu-id="28d78-117">See [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x](xref:fundamentals/metapackage) for more detail.</span></span>

## <a name="project-structure-differences"></a><span data-ttu-id="28d78-118">Differenze di struttura di progetto</span><span class="sxs-lookup"><span data-stu-id="28d78-118">Project structure differences</span></span>
<span data-ttu-id="28d78-119">Il *csproj* formato di file è stato semplificato in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="28d78-119">The *.csproj* file format has been simplified in ASP.NET Core.</span></span> <span data-ttu-id="28d78-120">Alcune modifiche importanti includono:</span><span class="sxs-lookup"><span data-stu-id="28d78-120">Some notable changes include:</span></span>
- <span data-ttu-id="28d78-121">Esplicita inclusione di file non è necessaria affinché possano essere considerate parte del progetto.</span><span class="sxs-lookup"><span data-stu-id="28d78-121">Explicit inclusion of files is not necessary for them to be considered part of the project.</span></span> <span data-ttu-id="28d78-122">Questo riduce il rischio di conflitti di unione XML quando si lavora in team di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="28d78-122">This reduces the risk of XML merge conflicts when working on large teams.</span></span>
- <span data-ttu-id="28d78-123">Non sono presenti riferimenti basato su GUID per altri progetti, che migliora la leggibilità di file.</span><span class="sxs-lookup"><span data-stu-id="28d78-123">There are no GUID-based references to other projects, which improves file readability.</span></span>
- <span data-ttu-id="28d78-124">Il file può essere modificato senza scaricarlo in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="28d78-124">The file can be edited without unloading it in Visual Studio:</span></span>

    ![Modificare l'opzione del menu contesto CSPROJ in Visual Studio 2017](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a><span data-ttu-id="28d78-126">Sostituzione di file Global. asax</span><span class="sxs-lookup"><span data-stu-id="28d78-126">Global.asax file replacement</span></span>
<span data-ttu-id="28d78-127">È stato introdotto un nuovo meccanismo per l'avvio di un'applicazione ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="28d78-127">ASP.NET Core introduced a new mechanism for bootstrapping an app.</span></span> <span data-ttu-id="28d78-128">Il punto di ingresso per le applicazioni ASP.NET è il *Global. asax* file.</span><span class="sxs-lookup"><span data-stu-id="28d78-128">The entry point for ASP.NET applications is the *Global.asax* file.</span></span> <span data-ttu-id="28d78-129">Attività quali la configurazione delle route e le registrazioni di area e di filtro vengono gestite nel *Global. asax* file.</span><span class="sxs-lookup"><span data-stu-id="28d78-129">Tasks such as route configuration and filter and area registrations are handled in the *Global.asax* file.</span></span>

<span data-ttu-id="28d78-130">[!code-csharp[Principale](samples/globalasax-sample.cs)]</span><span class="sxs-lookup"><span data-stu-id="28d78-130">[!code-csharp[Main](samples/globalasax-sample.cs)]</span></span>

<span data-ttu-id="28d78-131">Collegato a questo approccio l'applicazione e il server a cui è stato distribuito in modo che interferisce con l'implementazione.</span><span class="sxs-lookup"><span data-stu-id="28d78-131">This approach couples the application and the server to which it's deployed in a way that interferes with the implementation.</span></span> <span data-ttu-id="28d78-132">Nel tentativo di separare, [OWIN](http://owin.org/) è stata introdotta per fornire un modo più semplice da utilizzare insieme più Framework.</span><span class="sxs-lookup"><span data-stu-id="28d78-132">In an effort to decouple, [OWIN](http://owin.org/) was introduced to provide a cleaner way to use multiple frameworks together.</span></span> <span data-ttu-id="28d78-133">OWIN fornisce una pipeline per aggiungere solo i moduli necessari.</span><span class="sxs-lookup"><span data-stu-id="28d78-133">OWIN provides a pipeline to add only the modules needed.</span></span> <span data-ttu-id="28d78-134">L'ambiente host ha un [avvio](xref:fundamentals/startup) funzione per configurare servizi e delle pipeline delle richieste dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="28d78-134">The hosting environment takes a [Startup](xref:fundamentals/startup) function to configure services and the app's request pipeline.</span></span> <span data-ttu-id="28d78-135">`Startup`Registra un set di middleware con l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="28d78-135">`Startup` registers a set of middleware with the application.</span></span> <span data-ttu-id="28d78-136">Per ogni richiesta, l'applicazione chiama ognuno dei componenti middleware con il puntatore head di un elenco collegato a un set esistente di gestori.</span><span class="sxs-lookup"><span data-stu-id="28d78-136">For each request, the application calls each of the the middleware components with the head pointer of a linked list to an existing set of handlers.</span></span> <span data-ttu-id="28d78-137">Ogni componente del middleware è possibile aggiungere uno o più gestori per la gestione della pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="28d78-137">Each middleware component can add one or more handlers to the request handling pipeline.</span></span> <span data-ttu-id="28d78-138">Questa operazione viene eseguita tramite la restituzione di un riferimento al gestore che è il nuovo inizio dell'elenco.</span><span class="sxs-lookup"><span data-stu-id="28d78-138">This is accomplished by returning a reference to the handler that is the new head of the list.</span></span> <span data-ttu-id="28d78-139">Ogni gestore è responsabile per la memorizzazione e richiamare il gestore successivo nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="28d78-139">Each handler is responsible for remembering and invoking the next handler in the list.</span></span> <span data-ttu-id="28d78-140">Il punto di ingresso a un'applicazione ASP.NET di base, è `Startup`, e non è una dipendenza *Global. asax*.</span><span class="sxs-lookup"><span data-stu-id="28d78-140">With ASP.NET Core, the entry point to an application is `Startup`, and you no longer have a dependency on *Global.asax*.</span></span> <span data-ttu-id="28d78-141">Quando si utilizza OWIN con .NET Framework, è possibile utilizzare il seguente come una pipeline:</span><span class="sxs-lookup"><span data-stu-id="28d78-141">When using OWIN with .NET Framework, use something like the following as a pipeline:</span></span>

<span data-ttu-id="28d78-142">[!code-csharp[Principale](samples/webapi-owin.cs)]</span><span class="sxs-lookup"><span data-stu-id="28d78-142">[!code-csharp[Main](samples/webapi-owin.cs)]</span></span>

<span data-ttu-id="28d78-143">Ciò consente di configurare le route predefinite e il valore predefinito XmlSerialization su Json.</span><span class="sxs-lookup"><span data-stu-id="28d78-143">This configures your default routes, and defaults to XmlSerialization over Json.</span></span> <span data-ttu-id="28d78-144">Aggiungere altro Middleware per questa pipeline in base alle esigenze (il caricamento di servizi, le impostazioni di configurazione, file statici, ecc.).</span><span class="sxs-lookup"><span data-stu-id="28d78-144">Add other Middleware to this pipeline as needed (loading services, configuration settings, static files, etc.).</span></span>

<span data-ttu-id="28d78-145">ASP.NET Core viene utilizzato un approccio simile, ma non si basi su OWIN per gestire la voce.</span><span class="sxs-lookup"><span data-stu-id="28d78-145">ASP.NET Core uses a similar approach, but doesn't rely on OWIN to handle the entry.</span></span> <span data-ttu-id="28d78-146">Invece, che viene eseguita tramite il *Program.cs* `Main` metodo (simile alle applicazioni console) e `Startup` viene caricato tramite tale posizione.</span><span class="sxs-lookup"><span data-stu-id="28d78-146">Instead, that is done through the *Program.cs* `Main` method (similar to console applications) and `Startup` is loaded through there.</span></span>

<span data-ttu-id="28d78-147">[!code-csharp[Principale](samples/program.cs)]</span><span class="sxs-lookup"><span data-stu-id="28d78-147">[!code-csharp[Main](samples/program.cs)]</span></span>

<span data-ttu-id="28d78-148">`Startup`deve includere un `Configure` metodo.</span><span class="sxs-lookup"><span data-stu-id="28d78-148">`Startup` must include a `Configure` method.</span></span> <span data-ttu-id="28d78-149">In `Configure`, aggiungere il middleware necessario per la pipeline.</span><span class="sxs-lookup"><span data-stu-id="28d78-149">In `Configure`, add the necessary middleware to the pipeline.</span></span> <span data-ttu-id="28d78-150">Nell'esempio seguente (dal modello sito web predefinito), i diversi metodi di estensione vengono utilizzati per configurare la pipeline con il supporto per:</span><span class="sxs-lookup"><span data-stu-id="28d78-150">In the following example (from the default web site template), several extension methods are used to configure the pipeline with support for:</span></span>

* [<span data-ttu-id="28d78-151">BrowserLink</span><span class="sxs-lookup"><span data-stu-id="28d78-151">BrowserLink</span></span>](http://vswebessentials.com/features/browserlink)
* <span data-ttu-id="28d78-152">Pagine di errore</span><span class="sxs-lookup"><span data-stu-id="28d78-152">Error pages</span></span>
* <span data-ttu-id="28d78-153">File statici</span><span class="sxs-lookup"><span data-stu-id="28d78-153">Static files</span></span>
* <span data-ttu-id="28d78-154">Componenti di base di ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="28d78-154">ASP.NET Core MVC</span></span>
* <span data-ttu-id="28d78-155">Identità</span><span class="sxs-lookup"><span data-stu-id="28d78-155">Identity</span></span>

<span data-ttu-id="28d78-156">[!code-csharp[Principale](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]</span><span class="sxs-lookup"><span data-stu-id="28d78-156">[!code-csharp[Main](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]</span></span>

<span data-ttu-id="28d78-157">L'host e applicazione sono state separate, che fornisce la flessibilità di transizione a una piattaforma diversa in futuro.</span><span class="sxs-lookup"><span data-stu-id="28d78-157">The host and application have been decoupled, which provides the flexibility of moving to a different platform in the future.</span></span>

<span data-ttu-id="28d78-158">**Nota:** per un riferimento più dettaglio di avvio ASP.NET di base e Middleware, vedere [avvio ASP.NET Core](xref:fundamentals/startup)</span><span class="sxs-lookup"><span data-stu-id="28d78-158">**Note:** For a more in-depth reference to ASP.NET Core Startup and Middleware, see [Startup in ASP.NET Core](xref:fundamentals/startup)</span></span>

## <a name="storing-configurations"></a><span data-ttu-id="28d78-159">Archiviazione delle configurazioni</span><span class="sxs-lookup"><span data-stu-id="28d78-159">Storing Configurations</span></span>
<span data-ttu-id="28d78-160">ASP.NET supporta le impostazioni di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="28d78-160">ASP.NET supports storing settings.</span></span> <span data-ttu-id="28d78-161">Queste impostazioni vengono utilizzate, ad esempio, per supportare l'ambiente in cui sono state distribuite le applicazioni.</span><span class="sxs-lookup"><span data-stu-id="28d78-161">These setting are used, for example, to support the environment to which the applications were deployed.</span></span> <span data-ttu-id="28d78-162">È pratica comune per archiviare tutte le coppie chiave-valore personalizzate nel `<appSettings>` sezione la *Web. config* file:</span><span class="sxs-lookup"><span data-stu-id="28d78-162">A common practice was to store all custom key-value pairs in the `<appSettings>` section of the *Web.config* file:</span></span>

<span data-ttu-id="28d78-163">[!code-xml[Principale](samples/webconfig-sample.xml)]</span><span class="sxs-lookup"><span data-stu-id="28d78-163">[!code-xml[Main](samples/webconfig-sample.xml)]</span></span>

<span data-ttu-id="28d78-164">Le applicazioni leggono queste impostazioni utilizzando il `ConfigurationManager.AppSettings` insieme il `System.Configuration` dello spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="28d78-164">Applications read these settings using the `ConfigurationManager.AppSettings` collection in the `System.Configuration` namespace:</span></span>

<span data-ttu-id="28d78-165">[!code-csharp[Principale](samples/read-webconfig.cs)]</span><span class="sxs-lookup"><span data-stu-id="28d78-165">[!code-csharp[Main](samples/read-webconfig.cs)]</span></span>

<span data-ttu-id="28d78-166">ASP.NET Core è possibile archiviare i dati di configurazione per l'applicazione in tutti i file e caricarli come parte di avvio automatico di middleware.</span><span class="sxs-lookup"><span data-stu-id="28d78-166">ASP.NET Core can store configuration data for the application in any file and load them as part of middleware bootstrapping.</span></span> <span data-ttu-id="28d78-167">Il file predefinito utilizzato nei modelli di progetto è *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="28d78-167">The default file used in the project templates is *appsettings.json*:</span></span>

<span data-ttu-id="28d78-168">[!code-json[Principale](samples/appsettings-sample.json)]</span><span class="sxs-lookup"><span data-stu-id="28d78-168">[!code-json[Main](samples/appsettings-sample.json)]</span></span>

<span data-ttu-id="28d78-169">Caricare il file in un'istanza di `IConfiguration` all'interno dell'applicazione viene eseguita in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="28d78-169">Loading this file into an instance of `IConfiguration` inside your application is done in *Startup.cs*:</span></span>

<span data-ttu-id="28d78-170">[!code-csharp[Principale](samples/startup-builder.cs)]</span><span class="sxs-lookup"><span data-stu-id="28d78-170">[!code-csharp[Main](samples/startup-builder.cs)]</span></span>

<span data-ttu-id="28d78-171">L'app legge da `Configuration` per ottenere le impostazioni:</span><span class="sxs-lookup"><span data-stu-id="28d78-171">The app reads from `Configuration` to get the settings:</span></span>

<span data-ttu-id="28d78-172">[!code-csharp[Principale](samples/read-appsettings.cs)]</span><span class="sxs-lookup"><span data-stu-id="28d78-172">[!code-csharp[Main](samples/read-appsettings.cs)]</span></span>

<span data-ttu-id="28d78-173">Sono disponibili estensioni di questo approccio per rendere più affidabile, ad esempio utilizzando il processo [Dependency Injection](xref:fundamentals/dependency-injection) (DI) per caricare un servizio con questi valori.</span><span class="sxs-lookup"><span data-stu-id="28d78-173">There are extensions to this approach to make the process more robust, such as using [Dependency Injection](xref:fundamentals/dependency-injection) (DI) to load a service with these values.</span></span> <span data-ttu-id="28d78-174">L'approccio DI fornisce un set di fortemente tipizzata di oggetti di configurazione.</span><span class="sxs-lookup"><span data-stu-id="28d78-174">The DI approach provides a strongly-typed set of configuration objects.</span></span>

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

<span data-ttu-id="28d78-175">**Nota:** per la documentazione più dettagliata alla configurazione di ASP.NET Core, vedere [configurazione in ASP.NET Core](xref:fundamentals/configuration).</span><span class="sxs-lookup"><span data-stu-id="28d78-175">**Note:** For a more in-depth reference to ASP.NET Core configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration).</span></span>

## <a name="native-dependency-injection"></a><span data-ttu-id="28d78-176">Inserimento di dipendenze nativo</span><span class="sxs-lookup"><span data-stu-id="28d78-176">Native Dependency Injection</span></span>
<span data-ttu-id="28d78-177">Un importante obiettivo durante la compilazione di applicazioni di grandi dimensioni, scalabile è dell'accoppiamento debole di componenti e servizi.</span><span class="sxs-lookup"><span data-stu-id="28d78-177">An important goal when building large, scalable applications is the loose coupling of components and services.</span></span> <span data-ttu-id="28d78-178">[Inserimento di dipendenze](xref:fundamentals/dependency-injection) è una tecnica comune per raggiungere questo obiettivo, ed è un componente nativo di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="28d78-178">[Dependency Injection](xref:fundamentals/dependency-injection) is a popular technique for achieving this, and it is a native component of ASP.NET Core.</span></span>

<span data-ttu-id="28d78-179">Nelle applicazioni ASP.NET, gli sviluppatori si basano su una libreria di terze parti per implementare l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="28d78-179">In ASP.NET applications, developers rely on a third-party library to implement Dependency Injection.</span></span> <span data-ttu-id="28d78-180">È una di queste librerie [Unity](https://github.com/unitycontainer/unity), fornito da Microsoft Patterns & Practices.</span><span class="sxs-lookup"><span data-stu-id="28d78-180">One such library is [Unity](https://github.com/unitycontainer/unity), provided by Microsoft Patterns & Practices.</span></span> 

<span data-ttu-id="28d78-181">Implementazione di un esempio di configurazione di inserimento di dipendenze con Unity `IDependencyResolver` che esegue il wrapping di un `UnityContainer`:</span><span class="sxs-lookup"><span data-stu-id="28d78-181">An example of setting up Dependency Injection with Unity is implementing `IDependencyResolver` that wraps a `UnityContainer`:</span></span>

<span data-ttu-id="28d78-182">[!code-csharp[Principale](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]</span><span class="sxs-lookup"><span data-stu-id="28d78-182">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]</span></span>

<span data-ttu-id="28d78-183">Creare un'istanza del `UnityContainer`, registrare il servizio e impostare il resolver di dipendenza di `HttpConfiguration` nella nuova istanza di `UnityResolver` per il contenitore:</span><span class="sxs-lookup"><span data-stu-id="28d78-183">Create an instance of your `UnityContainer`, register your service, and set the dependency resolver of `HttpConfiguration` to the new instance of `UnityResolver` for your container:</span></span>

<span data-ttu-id="28d78-184">[!code-csharp[Principale](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]</span><span class="sxs-lookup"><span data-stu-id="28d78-184">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]</span></span>

<span data-ttu-id="28d78-185">Inserire `IProductRepository` dove necessario:</span><span class="sxs-lookup"><span data-stu-id="28d78-185">Inject `IProductRepository` where needed:</span></span>

<span data-ttu-id="28d78-186">[!code-csharp[Principale](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]</span><span class="sxs-lookup"><span data-stu-id="28d78-186">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]</span></span>

<span data-ttu-id="28d78-187">Poiché l'inserimento di dipendenze fa parte di ASP.NET Core, è possibile aggiungere il servizio nel `ConfigureServices` metodo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="28d78-187">Because Dependency Injection is part of ASP.NET Core, you can add your service in the `ConfigureServices` method of *Startup.cs*:</span></span>

<span data-ttu-id="28d78-188">[!code-csharp[Principale](samples/configure-services.cs)]</span><span class="sxs-lookup"><span data-stu-id="28d78-188">[!code-csharp[Main](samples/configure-services.cs)]</span></span>

<span data-ttu-id="28d78-189">Il repository può essere inserito in qualsiasi posizione, analogamente a Unity.</span><span class="sxs-lookup"><span data-stu-id="28d78-189">The repository can be injected anywhere, as was true with Unity.</span></span>

<span data-ttu-id="28d78-190">**Nota:** per un riferimento dettagliato per l'inserimento di dipendenze in ASP.NET Core, vedere [Dependency Injection in ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)</span><span class="sxs-lookup"><span data-stu-id="28d78-190">**Note:** For an in-depth reference to dependency injection in ASP.NET Core, see [Dependency Injection in ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)</span></span>

## <a name="serving-static-files"></a><span data-ttu-id="28d78-191">Gestione dei file statici</span><span class="sxs-lookup"><span data-stu-id="28d78-191">Serving Static Files</span></span>
<span data-ttu-id="28d78-192">Una parte importante dello sviluppo web è la possibilità di distribuire asset statico, sul lato client.</span><span class="sxs-lookup"><span data-stu-id="28d78-192">An important part of web development is the ability to serve static, client-side assets.</span></span> <span data-ttu-id="28d78-193">Gli esempi più comuni dei file statici sono HTML, CSS, Javascript e immagini.</span><span class="sxs-lookup"><span data-stu-id="28d78-193">The most common examples of static files are HTML, CSS, Javascript, and images.</span></span> <span data-ttu-id="28d78-194">Questi file devono essere salvati nella posizione di pubblicazione dell'app (o della rete CDN) e a cui fa riferimento in modo che può essere caricati da una richiesta.</span><span class="sxs-lookup"><span data-stu-id="28d78-194">These files need to be saved in the published location of the app (or CDN) and referenced so they can be loaded by a request.</span></span> <span data-ttu-id="28d78-195">Questo processo è stato modificato in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="28d78-195">This process has changed in ASP.NET Core.</span></span>

<span data-ttu-id="28d78-196">In ASP.NET, file statici sono archiviati in directory diverse e a cui fa riferimento nelle viste.</span><span class="sxs-lookup"><span data-stu-id="28d78-196">In ASP.NET, static files are stored in various directories and referenced in the views.</span></span>

<span data-ttu-id="28d78-197">In ASP.NET Core, file statici vengono archiviati nella radice web"" (*&lt;contenuto radice&gt;/wwwroot*), salvo configurazione diversa.</span><span class="sxs-lookup"><span data-stu-id="28d78-197">In ASP.NET Core, static files are stored in the "web root" (*&lt;content root&gt;/wwwroot*), unless configured otherwise.</span></span> <span data-ttu-id="28d78-198">I file vengono caricati nella pipeline delle richieste richiamando il `UseStaticFiles` il metodo di estensione da `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="28d78-198">The files are loaded into the request pipeline by invoking the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

<span data-ttu-id="28d78-199">[!code-csharp[Principale](../../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="28d78-199">[!code-csharp[Main](../../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]</span></span>

<span data-ttu-id="28d78-200">**Nota:** se la destinazione di .NET Framework, installare il pacchetto NuGet `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="28d78-200">**Note:** If targeting .NET Framework, install the NuGet package `Microsoft.AspNetCore.StaticFiles`.</span></span>

<span data-ttu-id="28d78-201">Ad esempio, una risorsa immagine nel *wwwroot/immagini* cartella sia accessibile al browser in corrispondenza della posizione, ad esempio `http://<app>/images/<imageFileName>`.</span><span class="sxs-lookup"><span data-stu-id="28d78-201">For example, an image asset in the *wwwroot/images* folder is accessible to the browser at a location such as `http://<app>/images/<imageFileName>`.</span></span>

<span data-ttu-id="28d78-202">**Nota:** per la documentazione più dettagliata di gestione dei file statici in ASP.NET Core, vedere [Introduzione all'utilizzo dei file statici di ASP.NET Core](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="28d78-202">**Note:** For a more in-depth reference to serving static files in ASP.NET Core, see [Introduction to working with static files in ASP.NET Core](xref:fundamentals/static-files).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="28d78-203">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="28d78-203">Additional Resources</span></span>
* [<span data-ttu-id="28d78-204">Importazione di librerie per .NET Core</span><span class="sxs-lookup"><span data-stu-id="28d78-204">Porting Libraries to .NET Core</span></span>](https://docs.microsoft.com/dotnet/core/porting/libraries)
