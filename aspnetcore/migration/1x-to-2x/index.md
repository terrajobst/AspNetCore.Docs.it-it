---
title: Eseguire la migrazione da ASP.NET Core 1.x alla versione 2.0
author: scottaddie
description: In questo articolo vengono illustrati i prerequisiti e i passaggi più comuni per la migrazione di un progetto di ASP.NET Core 1.x su ASP.NET 2.0 Core.
manager: wpickett
ms.author: scaddie
ms.date: 10/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/1x-to-2x/index
ms.openlocfilehash: f96b38e9f0368e04198c849bb3585674183a92b1
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/27/2018
ms.locfileid: "34555235"
---
# <a name="migrate-from-aspnet-core-1x-to-20"></a><span data-ttu-id="2b9b6-103">Eseguire la migrazione da ASP.NET Core 1.x alla versione 2.0</span><span class="sxs-lookup"><span data-stu-id="2b9b6-103">Migrate from ASP.NET Core 1.x to 2.0</span></span>

<span data-ttu-id="2b9b6-104">Di [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="2b9b6-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="2b9b6-105">Questo articolo illustra come aggiornare un progetto ASP.NET Core 1.x esistente ad ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-105">In this article, we walk you through updating an existing ASP.NET Core 1.x project to ASP.NET Core 2.0.</span></span> <span data-ttu-id="2b9b6-106">La migrazione dell'applicazione ad ASP.NET 2.0 Core consente di sfruttare le [molte nuove caratteristiche e i miglioramenti delle prestazioni](xref:aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="2b9b6-106">Migrating your application to ASP.NET Core 2.0 enables you to take advantage of [many new features and performance improvements](xref:aspnetcore-2.0).</span></span> 

<span data-ttu-id="2b9b6-107">Le applicazioni esistenti di ASP.NET Core 1.x si basano su modelli di progetto specifico della versione.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-107">Existing ASP.NET Core 1.x applications are based off of version-specific project templates.</span></span> <span data-ttu-id="2b9b6-108">Quando il framework di ASP.NET Core si evolve, i modelli del progetto fanno la stessa cosa, così come il codice di avvio in essi contenuti.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-108">As the ASP.NET Core framework evolves, so do the project templates and the starter code contained within them.</span></span> <span data-ttu-id="2b9b6-109">Oltre ad aggiornare il framework di ASP.NET Core, è necessario aggiornare il codice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-109">In addition to updating the ASP.NET Core framework, you need to update the code for your application.</span></span>

<a name="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="2b9b6-110">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="2b9b6-110">Prerequisites</span></span>
<span data-ttu-id="2b9b6-111">Vedere [Introduzione ad ASP.NET Core](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="2b9b6-111">See [Get Started with ASP.NET Core](xref:getting-started).</span></span>

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a><span data-ttu-id="2b9b6-112">Aggiornare Moniker della versione di .NET Framework di destinazione (TFM, Target Framework Moniker)</span><span class="sxs-lookup"><span data-stu-id="2b9b6-112">Update Target Framework Moniker (TFM)</span></span>
<span data-ttu-id="2b9b6-113">I progetti destinati a .NET Core devono usare il [TFM](/dotnet/standard/frameworks#referring-to-frameworks) di una versione successiva o uguale a .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-113">Projects targeting .NET Core should use the [TFM](/dotnet/standard/frameworks#referring-to-frameworks) of a version greater than or equal to .NET Core 2.0.</span></span> <span data-ttu-id="2b9b6-114">Cercare il nodo `<TargetFramework>` nel file *csproj*, e sostituire il testo interno con `netcoreapp2.0`:</span><span class="sxs-lookup"><span data-stu-id="2b9b6-114">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `netcoreapp2.0`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=3)]

<span data-ttu-id="2b9b6-115">I progetti destinati a .NET Framework devono usare il TFM di una versione successiva o uguale a .NET Framework 4.6.1.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-115">Projects targeting .NET Framework should use the TFM of a version greater than or equal to .NET Framework 4.6.1.</span></span> <span data-ttu-id="2b9b6-116">Cercare il nodo `<TargetFramework>` nel file *csproj*, e sostituire il testo interno con `net461`:</span><span class="sxs-lookup"><span data-stu-id="2b9b6-116">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `net461`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> <span data-ttu-id="2b9b6-117">.NET core 2.0 offre una quantità di superficie maggiore rispetto a .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-117">.NET Core 2.0 offers a much larger surface area than .NET Core 1.x.</span></span> <span data-ttu-id="2b9b6-118">Se il destinatario è .NET Framework esclusivamente a causa della mancanza di API in .NET Core 1.x, avere .NET Core 2.0 come destinatario potrebbe funzionare.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-118">If you're targeting .NET Framework solely because of missing APIs in .NET Core 1.x, targeting .NET Core 2.0 is likely to work.</span></span>

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a><span data-ttu-id="2b9b6-119">Aggiornare la versione di .NET Core SDK in global.json</span><span class="sxs-lookup"><span data-stu-id="2b9b6-119">Update .NET Core SDK version in global.json</span></span>
<span data-ttu-id="2b9b6-120">Se la soluzione si basa su un file [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) per avere come destinatario una specifica versione di .NET Core SDK, aggiornare la relativa proprietà `version` per usare la versione 2.0 installata nel computer:</span><span class="sxs-lookup"><span data-stu-id="2b9b6-120">If your solution relies upon a [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) file to target a specific .NET Core SDK version, update its `version` property to use the 2.0 version installed on your machine:</span></span>

[!code-json[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a><span data-ttu-id="2b9b6-121">Aggiornare i riferimenti del pacchetto</span><span class="sxs-lookup"><span data-stu-id="2b9b6-121">Update package references</span></span>
<span data-ttu-id="2b9b6-122">Il file *csproj* in un progetto di 1.x elenca ogni pacchetto NuGet usato dal progetto.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-122">The *.csproj* file in a 1.x project lists each NuGet package used by the project.</span></span>

<span data-ttu-id="2b9b6-123">In un progetto ASP.NET Core 2.0 destinato a .NET Core 2.0, un singolo riferimento del [metapacchetto](xref:fundamentals/metapackage) nel file *csproj* sostituisce la raccolta di pacchetti:</span><span class="sxs-lookup"><span data-stu-id="2b9b6-123">In an ASP.NET Core 2.0 project targeting .NET Core 2.0, a single [metapackage](xref:fundamentals/metapackage) reference in the *.csproj* file replaces the collection of packages:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=8-10)]

<span data-ttu-id="2b9b6-124">Tutte le funzionalità di ASP.NET Core 2.0 e di Entity Framework Core 2.0 sono incluse nel metapacchetto.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-124">All the features of ASP.NET Core 2.0 and Entity Framework Core 2.0 are included in the metapackage.</span></span>

<span data-ttu-id="2b9b6-125">I progetti di ASP.NET Core 2.0 con destinazione .NET Framework devono fare riferimento a singoli pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-125">ASP.NET Core 2.0 projects targeting .NET Framework should continue to reference individual NuGet packages.</span></span> <span data-ttu-id="2b9b6-126">Aggiornamento dell'attributo `Version` di ogni `<PackageReference />` nodo a 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-126">Update the `Version` attribute of each `<PackageReference />` node to 2.0.0.</span></span>

<span data-ttu-id="2b9b6-127">Ad esempio, ecco l'elenco dei `<PackageReference />` nodi usati in un progetto ASP.NET Core 2.0 tipico destinato a .NET Framework:</span><span class="sxs-lookup"><span data-stu-id="2b9b6-127">For example, here's the list of `<PackageReference />` nodes used in a typical ASP.NET Core 2.0 project targeting .NET Framework:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a><span data-ttu-id="2b9b6-128">Strumenti dell'interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="2b9b6-128">Update .NET Core CLI tools</span></span>
<span data-ttu-id="2b9b6-129">Nel file *.csproj* aggiornare l'attributo `Version` di ogni nodo `<DotNetCliToolReference />` a 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-129">In the *.csproj* file, update the `Version` attribute of each `<DotNetCliToolReference />` node to 2.0.0.</span></span>

<span data-ttu-id="2b9b6-130">Ad esempio, ecco l'elenco degli strumenti dell'interfaccia della riga di comando usati in un progetto ASP.NET Core 2.0 tipico destinato a .NET Core 2.0:</span><span class="sxs-lookup"><span data-stu-id="2b9b6-130">For example, here's the list of CLI tools used in a typical ASP.NET Core 2.0 project targeting .NET Core 2.0:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=12-16)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a><span data-ttu-id="2b9b6-131">Rinominare la proprietà di Fallback di destinazione del pacchetto</span><span class="sxs-lookup"><span data-stu-id="2b9b6-131">Rename Package Target Fallback property</span></span>
<span data-ttu-id="2b9b6-132">Il file *csproj* di un progetto 1.x ha usato un nodo `PackageTargetFallback` e una variabile:</span><span class="sxs-lookup"><span data-stu-id="2b9b6-132">The *.csproj* file of a 1.x project used a `PackageTargetFallback` node and variable:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=5)]

<span data-ttu-id="2b9b6-133">Rinominare il nodo e la variabile a `AssetTargetFallback`:</span><span class="sxs-lookup"><span data-stu-id="2b9b6-133">Rename both the node and variable to `AssetTargetFallback`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=4)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a><span data-ttu-id="2b9b6-134">Aggiornare il metodo Main in Program.cs</span><span class="sxs-lookup"><span data-stu-id="2b9b6-134">Update Main method in Program.cs</span></span>
<span data-ttu-id="2b9b6-135">Nei progetti di 1.x, il metodo `Main` di *Program.cs* era simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="2b9b6-135">In 1.x projects, the `Main` method of *Program.cs* looked like this:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

<span data-ttu-id="2b9b6-136">Nei progetti 2.0, il metodo `Main` di *Program.cs* è stato semplificato:</span><span class="sxs-lookup"><span data-stu-id="2b9b6-136">In 2.0 projects, the `Main` method of *Program.cs* has been simplified:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program.cs?highlight=8-11)]

<span data-ttu-id="2b9b6-137">L'adozione di questo nuovo modello 2.0 è altamente consigliabile ed è necessaria affinché le funzionalità di prodotto come le [Migrazioni Entity Framework (EF) Core](xref:data/ef-mvc/migrations) funzionino.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-137">The adoption of this new 2.0 pattern is highly recommended and is required for product features like [Entity Framework (EF) Core Migrations](xref:data/ef-mvc/migrations) to work.</span></span> <span data-ttu-id="2b9b6-138">Ad esempio, l'esecuzione di `Update-Database` dalla finestra della Console di Gestione pacchetti o di `dotnet ef database update` dalla riga di comando (nei progetti convertiti in ASP.NET Core 2.0) genera l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="2b9b6-138">For example, running `Update-Database` from the Package Manager Console window or `dotnet ef database update` from the command line (on projects converted to ASP.NET Core 2.0) generates the following error:</span></span>

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="add-modify-configuration"></a>

## <a name="add-configuration-providers"></a><span data-ttu-id="2b9b6-139">Aggiungere provider di configurazione</span><span class="sxs-lookup"><span data-stu-id="2b9b6-139">Add configuration providers</span></span>
<span data-ttu-id="2b9b6-140">Nei progetti di 1. x viene usato il costruttore `Startup` per aggiungere i provider di configurazione a un'app.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-140">In 1.x projects, adding configuration providers to an app was accomplished via the `Startup` constructor.</span></span> <span data-ttu-id="2b9b6-141">La procedura implica la creazione di un'istanza di `ConfigurationBuilder`, il caricamento di provider applicabili (variabili di ambiente, impostazioni dell'app e così via) e l'inizializzazione di un membro di `IConfigurationRoot`.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-141">The steps involved creating an instance of `ConfigurationBuilder`, loading applicable providers (environment variables, app settings, etc.), and initializing a member of `IConfigurationRoot`.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_1xStartup)]

<span data-ttu-id="2b9b6-142">Nell'esempio precedente il membro `Configuration` viene caricato con le impostazioni di configurazione da *appsettings.json* e da un qualsiasi file *appsettings.\<NomeAmbiente\>.json* corrispondente alla proprietà `IHostingEnvironment.EnvironmentName`.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-142">The preceding example loads the `Configuration` member with configuration settings from *appsettings.json* as well as any *appsettings.\<EnvironmentName\>.json* file matching the `IHostingEnvironment.EnvironmentName` property.</span></span> <span data-ttu-id="2b9b6-143">Questi file si trovano nello stesso percorso di *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-143">The location of these files is at the same path as *Startup.cs*.</span></span>

<span data-ttu-id="2b9b6-144">Nei progetti 2.0 il codice di configurazione boilerplate relativo ai progetti 1. x viene eseguito in background.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-144">In 2.0 projects, the boilerplate configuration code inherent to 1.x projects runs behind-the-scenes.</span></span> <span data-ttu-id="2b9b6-145">Le variabili di ambiente e le impostazioni dell'app vengono ad esempio caricate all'avvio.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-145">For example, environment variables and app settings are loaded at startup.</span></span> <span data-ttu-id="2b9b6-146">Il codice *Startup.cs* equivalente viene ridotto all'inizializzazione `IConfiguration` con l'istanza inserita:</span><span class="sxs-lookup"><span data-stu-id="2b9b6-146">The equivalent *Startup.cs* code is reduced to `IConfiguration` initialization with the injected instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Startup.cs?name=snippet_2xStartup)]

<span data-ttu-id="2b9b6-147">Per rimuovere i provider predefiniti aggiunti da `WebHostBuilder.CreateDefaultBuilder`, richiamare il metodo `Clear` nella proprietà `IConfigurationBuilder.Sources` all'interno di `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-147">To remove the default providers added by `WebHostBuilder.CreateDefaultBuilder`, invoke the `Clear` method on the `IConfigurationBuilder.Sources` property inside of `ConfigureAppConfiguration`.</span></span> <span data-ttu-id="2b9b6-148">Per aggiungere di nuovo i provider, usare il metodo `ConfigureAppConfiguration`in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="2b9b6-148">To add providers back, utilize the `ConfigureAppConfiguration` method in *Program.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Program.cs?name=snippet_ProgramMainConfigProviders&highlight=9-14)]

<span data-ttu-id="2b9b6-149">La configurazione usata dal metodo `CreateDefaultBuilder` nel frammento di codice precedente può essere visualizzata [qui](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="2b9b6-149">The configuration used by the `CreateDefaultBuilder` method in the preceding code snippet can be seen [here](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).</span></span>

<span data-ttu-id="2b9b6-150">Per altre informazioni, vedere [Configurazione in ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="2b9b6-150">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

<a name="db-init-code"></a>

## <a name="move-database-initialization-code"></a><span data-ttu-id="2b9b6-151">Spostare il codice di inizializzazione del database</span><span class="sxs-lookup"><span data-stu-id="2b9b6-151">Move database initialization code</span></span>
<span data-ttu-id="2b9b6-152">Nei progetti 1.x che usano EF Core 1.x un comando come ad esempio `dotnet ef migrations add` esegue le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="2b9b6-152">In 1.x projects using EF Core 1.x, a command such as `dotnet ef migrations add` does the following:</span></span>
1. <span data-ttu-id="2b9b6-153">Crea un'istanza dell'istanza `Startup`</span><span class="sxs-lookup"><span data-stu-id="2b9b6-153">Instantiates a `Startup` instance</span></span>
2. <span data-ttu-id="2b9b6-154">Richiama il metodo `ConfigureServices` per registrare tutti i servizi con inserimento di dipendenze, inclusi i tipi `DbContext`</span><span class="sxs-lookup"><span data-stu-id="2b9b6-154">Invokes the `ConfigureServices` method to register all services with dependency injection (including `DbContext` types)</span></span>
3. <span data-ttu-id="2b9b6-155">Esegue le attività necessarie</span><span class="sxs-lookup"><span data-stu-id="2b9b6-155">Performs its requisite tasks</span></span>

<span data-ttu-id="2b9b6-156">Nei progetti 2.0 che usano EF Core 2.0 viene richiamato `Program.BuildWebHost` per ottenere i servizi delle applicazioni.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-156">In 2.0 projects using EF Core 2.0, `Program.BuildWebHost` is invoked to obtain the application services.</span></span> <span data-ttu-id="2b9b6-157">A differenza della versione 1.x, questo ha l'effetto collaterale di richiamare `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-157">Unlike 1.x, this has the additional side effect of invoking `Startup.Configure`.</span></span> <span data-ttu-id="2b9b6-158">Se l'app 1.x ha richiamato il codice di inizializzazione del database nel metodo `Configure`, possono verificarsi problemi imprevisti.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-158">If your 1.x app invoked database initialization code in its `Configure` method, unexpected problems can occur.</span></span> <span data-ttu-id="2b9b6-159">Ad esempio, se il database non esiste ancora, il codice di seeding viene eseguito prima dell'esecuzione del comando delle migrazioni di EF Core.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-159">For example, if the database doesn't yet exist, the seeding code runs before the EF Core Migrations command execution.</span></span> <span data-ttu-id="2b9b6-160">Questo problema causa l'esito negativo del comando `dotnet ef migrations list` se il database non esiste ancora.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-160">This problem causes a `dotnet ef migrations list` command to fail if the database doesn't yet exist.</span></span>

<span data-ttu-id="2b9b6-161">Si consideri il codice di inizializzazione del seed 1.x seguente nel metodo `Configure` di *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="2b9b6-161">Consider the following 1.x seed initialization code in the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_ConfigureSeedData&highlight=8)]

<span data-ttu-id="2b9b6-162">Nei progetti 2.0 spostare la chiamata `SeedData.Initialize` al metodo `Main` di *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="2b9b6-162">In 2.0 projects, move the `SeedData.Initialize` call to the `Main` method of *Program.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program2.cs?name=snippet_Main2Code&highlight=10)]

<span data-ttu-id="2b9b6-163">A partire dalla versione 2.0, non è una buona prassi eseguire alcuna operazione in `BuildWebHost` ad eccezione della compilazione e della configurazione dell'host Web.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-163">As of 2.0, it's bad practice to do anything in `BuildWebHost` except build and configure the web host.</span></span> <span data-ttu-id="2b9b6-164">Tutto ciò che riguarda l'esecuzione dell'applicazione deve essere gestito all'esterno di `BuildWebHost`, in genere nel metodo `Main` di *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-164">Anything that's about running the application should be handled outside of `BuildWebHost` &mdash; typically in the `Main` method of *Program.cs*.</span></span>

<a name="view-compilation"></a>

## <a name="review-razor-view-compilation-setting"></a><span data-ttu-id="2b9b6-165">Verificare l'impostazione di compilazione della vista Razor</span><span class="sxs-lookup"><span data-stu-id="2b9b6-165">Review Razor view compilation setting</span></span>
<span data-ttu-id="2b9b6-166">Tempi di avvio dell'applicazione più rapidi e aggregazioni pubblicate più piccole sono di importanza fondamentale per l'utente.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-166">Faster application startup time and smaller published bundles are of utmost importance to you.</span></span> <span data-ttu-id="2b9b6-167">Per questi motivi, [la compilazione della vista Razor](xref:mvc/views/view-compilation) è abilitata per impostazione predefinita in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-167">For these reasons, [Razor view compilation](xref:mvc/views/view-compilation) is enabled by default in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="2b9b6-168">L'impostazione della proprietà `MvcRazorCompileOnPublish` su true non è più necessaria.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-168">Setting the `MvcRazorCompileOnPublish` property to true is no longer required.</span></span> <span data-ttu-id="2b9b6-169">A meno che non si stia disattivando la compilazione della vista, la proprietà può essere rimossa dal file *csproj*.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-169">Unless you're disabling view compilation, the property may be removed from the *.csproj* file.</span></span>

<span data-ttu-id="2b9b6-170">Quando la destinazione è .NET Framework, è necessario fare comunque riferimento in modo esplicito al pacchetto NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) nel file *csproj*:</span><span class="sxs-lookup"><span data-stu-id="2b9b6-170">When targeting .NET Framework, you still need to explicitly reference the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet package in your *.csproj* file:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a><span data-ttu-id="2b9b6-171">Basarsi sulle funzionalità "light-up" di Application Insights</span><span class="sxs-lookup"><span data-stu-id="2b9b6-171">Rely on Application Insights "light-up" features</span></span>
<span data-ttu-id="2b9b6-172">Un semplice programma di installazione di strumentazione delle prestazioni dell'applicazione è importante.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-172">Effortless setup of application performance instrumentation is important.</span></span> <span data-ttu-id="2b9b6-173">È ora possibile basarsi sulle nuove funzionalità "light-up" di [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) disponibili negli strumenti di Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-173">You can now rely on the new [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "light-up" features available in the Visual Studio 2017 tooling.</span></span>

<span data-ttu-id="2b9b6-174">Per impostazione predefinita, i progetti ASP.NET Core 1.1 creati in Visual Studio 2017 hanno aggiunto Application Insights.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-174">ASP.NET Core 1.1 projects created in Visual Studio 2017 added Application Insights by default.</span></span> <span data-ttu-id="2b9b6-175">Se non si usa Application Insights SDK direttamente, di fuori di *Program.cs* e *Startup.cs*, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="2b9b6-175">If you're not using the Application Insights SDK directly, outside of *Program.cs* and *Startup.cs*, follow these steps:</span></span>

1. <span data-ttu-id="2b9b6-176">Se la destinazione è .NET Core, rimuovere il nodo `<PackageReference />` seguente dal file *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="2b9b6-176">If targeting .NET Core, remove the following `<PackageReference />` node from the *.csproj* file:</span></span>
    
    [!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=10)]

2. <span data-ttu-id="2b9b6-177">Se la destinazione è .NET Core, rimuovere la chiamata del metodo di estensione `UseApplicationInsights` da *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="2b9b6-177">If targeting .NET Core, remove the `UseApplicationInsights` extension method invocation from *Program.cs*:</span></span>

    [!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. <span data-ttu-id="2b9b6-178">Rimuovere la chiamata di API sul lato client di Application Insights da *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-178">Remove the Application Insights client-side API call from *_Layout.cshtml*.</span></span> <span data-ttu-id="2b9b6-179">Comprende le due righe di codice seguenti:</span><span class="sxs-lookup"><span data-stu-id="2b9b6-179">It comprises the following two lines of code:</span></span>

    [!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Shared/_Layout.cshtml?range=1,19&dedent=4)]

<span data-ttu-id="2b9b6-180">Se si usa Application Insights SDK direttamente, continuare a farlo.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-180">If you are using the Application Insights SDK directly, continue to do so.</span></span> <span data-ttu-id="2b9b6-181">Il [metapacchetto](xref:fundamentals/metapackage) 2.0 comprende la versione più recente di Application Insights, pertanto viene visualizzato un errore di downgrade del pacchetto se si fa riferimento a una versione precedente.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-181">The 2.0 [metapackage](xref:fundamentals/metapackage) includes the latest version of Application Insights, so a package downgrade error appears if you're referencing an older version.</span></span>

<a name="auth-and-identity"></a>

## <a name="adopt-authenticationidentity-improvements"></a><span data-ttu-id="2b9b6-182">Adottare i miglioramenti di autenticazione/identità</span><span class="sxs-lookup"><span data-stu-id="2b9b6-182">Adopt authentication/Identity improvements</span></span>
<span data-ttu-id="2b9b6-183">ASP.NET Core 2.0 ha un nuovo modello di autenticazione e un numero di modifiche significative per ASP.NET Identity Core.</span><span class="sxs-lookup"><span data-stu-id="2b9b6-183">ASP.NET Core 2.0 has a new authentication model and a number of significant changes to ASP.NET Core Identity.</span></span> <span data-ttu-id="2b9b6-184">Se il progetto è stato creato con l'autenticazione Account utente individuali abilitata o se è stata aggiunta manualmente l'autenticazione o l'identità, vedere [Eseguire la migrazione di autenticazione e identità ad ASP.NET 2.0 Core](xref:migration/1x-to-2x/identity-2x).</span><span class="sxs-lookup"><span data-stu-id="2b9b6-184">If you created your project with Individual User Accounts enabled, or if you have manually added authentication or Identity, see [Migrate Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2b9b6-185">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="2b9b6-185">Additional resources</span></span>

* [<span data-ttu-id="2b9b6-186">Modifiche importanti apportate in ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="2b9b6-186">Breaking Changes in ASP.NET Core 2.0</span></span>](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
