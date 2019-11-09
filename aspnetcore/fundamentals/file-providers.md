---
title: Provider di file in ASP.NET Core
author: guardrex
description: Informazioni su come ASP.NET Core astrae l'accesso al file system tramite l'utilizzo di provider di file.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: fundamentals/file-providers
ms.openlocfilehash: 531f7acd7a704a74e6142d201f613f05288deecb
ms.sourcegitcommit: 4818385c3cfe0805e15138a2c1785b62deeaab90
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/09/2019
ms.locfileid: "73896847"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="c54f5-103">Provider di file in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c54f5-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="c54f5-104">Di [Steve Smith](https://ardalis.com/) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c54f5-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c54f5-105">ASP.NET Core astrae l'accesso al file system tramite l'utilizzo di provider di file.</span><span class="sxs-lookup"><span data-stu-id="c54f5-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="c54f5-106">I provider di file vengono usati in tutto il framework di ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="c54f5-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="c54f5-107">`IWebHostEnvironment` espone la radice del [contenuto](xref:fundamentals/index#content-root) dell'app e la [radice Web](xref:fundamentals/index#web-root) come tipi di `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="c54f5-107">`IWebHostEnvironment` exposes the app's [content root](xref:fundamentals/index#content-root) and [web root](xref:fundamentals/index#web-root) as `IFileProvider` types.</span></span>
* <span data-ttu-id="c54f5-108">Il [middleware dei file statici](xref:fundamentals/static-files) usa i provider di file per trovare i file statici.</span><span class="sxs-lookup"><span data-stu-id="c54f5-108">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="c54f5-109">[Razor](xref:mvc/views/razor) usa i provider di file per individuare pagine e viste.</span><span class="sxs-lookup"><span data-stu-id="c54f5-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="c54f5-110">Gli strumenti .NET Core usano i provider di file e i criteri GLOB per specificare i file da pubblicare.</span><span class="sxs-lookup"><span data-stu-id="c54f5-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="c54f5-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c54f5-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="c54f5-112">Interfacce del provider di file</span><span class="sxs-lookup"><span data-stu-id="c54f5-112">File Provider interfaces</span></span>

<span data-ttu-id="c54f5-113">L'interfaccia primaria è <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="c54f5-113">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="c54f5-114">`IFileProvider` espone metodi per:</span><span class="sxs-lookup"><span data-stu-id="c54f5-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="c54f5-115">Ottenere informazioni sui file (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span><span class="sxs-lookup"><span data-stu-id="c54f5-115">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="c54f5-116">Ottenere informazioni sulle directory (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span><span class="sxs-lookup"><span data-stu-id="c54f5-116">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="c54f5-117">Configurare le notifiche di modifica (usando un <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span><span class="sxs-lookup"><span data-stu-id="c54f5-117">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="c54f5-118">`IFileInfo` fornisce metodi e proprietà per l'uso di file:</span><span class="sxs-lookup"><span data-stu-id="c54f5-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="c54f5-119"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in byte)</span><span class="sxs-lookup"><span data-stu-id="c54f5-119"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="c54f5-120">Data <xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified></span><span class="sxs-lookup"><span data-stu-id="c54f5-120"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="c54f5-121">È possibile leggere dal file usando il metodo [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*).</span><span class="sxs-lookup"><span data-stu-id="c54f5-121">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="c54f5-122">L'app di esempio dimostra come configurare un provider di file in `Startup.ConfigureServices` da usare in tutta l'app tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c54f5-122">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="c54f5-123">Implementazioni dei provider di file</span><span class="sxs-lookup"><span data-stu-id="c54f5-123">File Provider implementations</span></span>

<span data-ttu-id="c54f5-124">Sono disponibili tre implementazioni di `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="c54f5-124">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="c54f5-125">Implementazione</span><span class="sxs-lookup"><span data-stu-id="c54f5-125">Implementation</span></span> | <span data-ttu-id="c54f5-126">Descrizione</span><span class="sxs-lookup"><span data-stu-id="c54f5-126">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="c54f5-127">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="c54f5-127">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="c54f5-128">Il provider fisico viene usato per accedere ai file fisici del sistema.</span><span class="sxs-lookup"><span data-stu-id="c54f5-128">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="c54f5-129">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="c54f5-129">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="c54f5-130">Il provider incorporato nel manifesto viene usato per accedere ai file incorporati negli assembly.</span><span class="sxs-lookup"><span data-stu-id="c54f5-130">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="c54f5-131">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="c54f5-131">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="c54f5-132">Il provider composito consente di offrire un accesso combinato a file e directory da uno o più provider.</span><span class="sxs-lookup"><span data-stu-id="c54f5-132">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="c54f5-133">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="c54f5-133">PhysicalFileProvider</span></span>

<span data-ttu-id="c54f5-134"><xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> consente di accedere al file system fisico.</span><span class="sxs-lookup"><span data-stu-id="c54f5-134">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="c54f5-135">`PhysicalFileProvider` usa il tipo <xref:System.IO.File?displayProperty=fullName> (per il provider fisico) e definisce una directory e i relativi elementi figlio come ambito per tutti i percorsi.</span><span class="sxs-lookup"><span data-stu-id="c54f5-135">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="c54f5-136">La definizione dell'ambito impedisce l'accesso al file system al di fuori della directory specificata e dei relativi elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="c54f5-136">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="c54f5-137">Lo scenario più comune per la creazione e l'uso di un `PhysicalFileProvider` consiste nel richiedere un oggetto `IFileProvider` in un costruttore tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c54f5-137">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="c54f5-138">Per la creazione diretta di un'istanza di questo provider, è richiesto un percorso di directory che viene usato come percorso di base per tutte le richieste effettuate tramite il provider.</span><span class="sxs-lookup"><span data-stu-id="c54f5-138">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="c54f5-139">Il codice seguente illustra come creare un `PhysicalFileProvider` e usarlo per ottenere il contenuto della directory e informazioni sui file:</span><span class="sxs-lookup"><span data-stu-id="c54f5-139">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="c54f5-140">Tipi nell'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="c54f5-140">Types in the preceding example:</span></span>

* <span data-ttu-id="c54f5-141">`provider` è `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="c54f5-141">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="c54f5-142">`contents` è `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="c54f5-142">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="c54f5-143">`fileInfo` è `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="c54f5-143">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="c54f5-144">Il provider di file può essere usato per scorrere la directory specificata da `applicationRoot` oppure per chiamare `GetFileInfo` per ottenere informazioni su un file.</span><span class="sxs-lookup"><span data-stu-id="c54f5-144">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="c54f5-145">Il provider di file non ha accesso all'esterno della directory `applicationRoot`.</span><span class="sxs-lookup"><span data-stu-id="c54f5-145">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="c54f5-146">L'app di esempio crea il provider nella classe `Startup.ConfigureServices` dell'app usando [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span><span class="sxs-lookup"><span data-stu-id="c54f5-146">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="c54f5-147">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="c54f5-147">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="c54f5-148"><xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> consente di accedere ai file incorporati negli assembly.</span><span class="sxs-lookup"><span data-stu-id="c54f5-148">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="c54f5-149">`ManifestEmbeddedFileProvider` usa un manifesto compilato nell'assembly per ricostruire i percorsi originali dei file incorporati.</span><span class="sxs-lookup"><span data-stu-id="c54f5-149">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="c54f5-150">Aggiungere un riferimento al pacchetto nel progetto per il pacchetto [Microsoft.Extensions.FileProviders.Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded).</span><span class="sxs-lookup"><span data-stu-id="c54f5-150">Add a package reference to the project for the [Microsoft.Extensions.FileProviders.Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) package.</span></span>

<span data-ttu-id="c54f5-151">Per generare un manifesto dei file incorporati, impostare la proprietà `<GenerateEmbeddedFilesManifest>` su `true`.</span><span class="sxs-lookup"><span data-stu-id="c54f5-151">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="c54f5-152">Specificare i file da incorporare con [\<EmbeddedResource>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span><span class="sxs-lookup"><span data-stu-id="c54f5-152">Specify the files to embed with [\<EmbeddedResource>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="c54f5-153">Usare [modelli GLOB](#glob-patterns) per specificare uno o più file da incorporare nell'assembly.</span><span class="sxs-lookup"><span data-stu-id="c54f5-153">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="c54f5-154">L'app di esempio crea un `ManifestEmbeddedFileProvider` e passa l'assembly attualmente in esecuzione al rispettivo costruttore.</span><span class="sxs-lookup"><span data-stu-id="c54f5-154">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="c54f5-155">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c54f5-155">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(typeof(Program).Assembly);
```

<span data-ttu-id="c54f5-156">Overload aggiuntivi consentono di:</span><span class="sxs-lookup"><span data-stu-id="c54f5-156">Additional overloads allow you to:</span></span>

* <span data-ttu-id="c54f5-157">Specificare un percorso di file relativo.</span><span class="sxs-lookup"><span data-stu-id="c54f5-157">Specify a relative file path.</span></span>
* <span data-ttu-id="c54f5-158">Definire la data dell'ultima modifica come ambito dei file.</span><span class="sxs-lookup"><span data-stu-id="c54f5-158">Scope files to a last modified date.</span></span>
* <span data-ttu-id="c54f5-159">Assegnare un nome alla risorsa incorporata contenente il manifesto dei file incorporati.</span><span class="sxs-lookup"><span data-stu-id="c54f5-159">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="c54f5-160">Overload</span><span class="sxs-lookup"><span data-stu-id="c54f5-160">Overload</span></span> | <span data-ttu-id="c54f5-161">Descrizione</span><span class="sxs-lookup"><span data-stu-id="c54f5-161">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="c54f5-162">Accetta un parametro di percorso relativo `root` facoltativo.</span><span class="sxs-lookup"><span data-stu-id="c54f5-162">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="c54f5-163">Specificare `root` per definire come ambito delle chiamate a <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> le risorse incluse nel percorso specificato.</span><span class="sxs-lookup"><span data-stu-id="c54f5-163">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="c54f5-164">Accetta un parametro di percorso relativo `root` facoltativo e un parametro di data `lastModified` (<xref:System.DateTimeOffset>).</span><span class="sxs-lookup"><span data-stu-id="c54f5-164">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="c54f5-165">La data `lastModified` definisce la data dell'ultima modifica come ambito per le istanze di <xref:Microsoft.Extensions.FileProviders.IFileInfo> restituite da <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="c54f5-165">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="c54f5-166">Accetta un parametro di percorso relativo `root` facoltativo, un parametro di data `lastModified` e il parametro `manifestName`.</span><span class="sxs-lookup"><span data-stu-id="c54f5-166">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="c54f5-167">`manifestName` rappresenta il nome della risorsa incorporata contenente il manifesto.</span><span class="sxs-lookup"><span data-stu-id="c54f5-167">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="c54f5-168">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="c54f5-168">CompositeFileProvider</span></span>

<span data-ttu-id="c54f5-169"><xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combina le istanze `IFileProvider` esponendo una sola interfaccia per l'uso dei file di più provider.</span><span class="sxs-lookup"><span data-stu-id="c54f5-169">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="c54f5-170">Quando si crea `CompositeFileProvider`, passare una o più istanze `IFileProvider` al relativo costruttore.</span><span class="sxs-lookup"><span data-stu-id="c54f5-170">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="c54f5-171">Nell'app di esempio, un `PhysicalFileProvider` e un `ManifestEmbeddedFileProvider` forniscono i file a un `CompositeFileProvider` registrato nel contenitore dei servizi dell'app:</span><span class="sxs-lookup"><span data-stu-id="c54f5-171">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="c54f5-172">Controllo delle modifiche</span><span class="sxs-lookup"><span data-stu-id="c54f5-172">Watch for changes</span></span>

<span data-ttu-id="c54f5-173">Il metodo [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) consente di controllare uno o più file o directory per il rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="c54f5-173">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="c54f5-174">`Watch` accetta una stringa di percorso in cui è possibile usare [criteri GLOB](#glob-patterns) per specificare più file.</span><span class="sxs-lookup"><span data-stu-id="c54f5-174">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="c54f5-175">`Watch` restituisce un oggetto <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span><span class="sxs-lookup"><span data-stu-id="c54f5-175">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="c54f5-176">Il token di modifica espone:</span><span class="sxs-lookup"><span data-stu-id="c54f5-176">The change token exposes:</span></span>

* <span data-ttu-id="c54f5-177"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; Una proprietà che può essere ispezionata per determinare se è stata apportata una modifica.</span><span class="sxs-lookup"><span data-stu-id="c54f5-177"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="c54f5-178"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Chiamato quando vengono rilevate modifiche alla stringa di percorso specificata.</span><span class="sxs-lookup"><span data-stu-id="c54f5-178"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="c54f5-179">Ogni token di modifica chiama solo il relativo callback associato in risposta a una singola modifica.</span><span class="sxs-lookup"><span data-stu-id="c54f5-179">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="c54f5-180">Per abilitare un monitoraggio continuo, usare <xref:System.Threading.Tasks.TaskCompletionSource`1> come illustrato di seguito oppure ricreare istanze di `IChangeToken` in risposta alle modifiche.</span><span class="sxs-lookup"><span data-stu-id="c54f5-180">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="c54f5-181">Nell'app di esempio, l'app console *WatchConsole* viene configurata per visualizzare un messaggio quando viene modificato un file di testo:</span><span class="sxs-lookup"><span data-stu-id="c54f5-181">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/3.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="c54f5-182">È possibile che alcuni file system, ad esempio i contenitori Docker e le condivisioni di rete, non inviino sempre le notifiche di modifica.</span><span class="sxs-lookup"><span data-stu-id="c54f5-182">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="c54f5-183">Impostare la variabile di ambiente `DOTNET_USE_POLLING_FILE_WATCHER` su `1` o `true` per eseguire il polling delle modifiche nel file system ogni quattro secondi (intervallo non configurabile).</span><span class="sxs-lookup"><span data-stu-id="c54f5-183">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="c54f5-184">Modelli GLOB</span><span class="sxs-lookup"><span data-stu-id="c54f5-184">Glob patterns</span></span>

<span data-ttu-id="c54f5-185">Nei percorsi del file system vengono usati criteri con caratteri jolly chiamati *criteri GLOB (o globbing)* .</span><span class="sxs-lookup"><span data-stu-id="c54f5-185">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="c54f5-186">Specificare gruppi di file con questi criteri.</span><span class="sxs-lookup"><span data-stu-id="c54f5-186">Specify groups of files with these patterns.</span></span> <span data-ttu-id="c54f5-187">I due caratteri jolly sono `*` e `**`:</span><span class="sxs-lookup"><span data-stu-id="c54f5-187">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="c54f5-188">Cerca qualsiasi elemento a livello della cartella corrente, qualsiasi nome di file o qualsiasi estensione di file.</span><span class="sxs-lookup"><span data-stu-id="c54f5-188">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="c54f5-189">Le corrispondenze vengono terminate con i caratteri `/` e `.` nel percorso dei file.</span><span class="sxs-lookup"><span data-stu-id="c54f5-189">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="c54f5-190">Cerca qualsiasi elemento in più livelli di directory.</span><span class="sxs-lookup"><span data-stu-id="c54f5-190">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="c54f5-191">Può essere usato per cercare in modo ricorsivo numerosi file all'interno di una gerarchia di directory.</span><span class="sxs-lookup"><span data-stu-id="c54f5-191">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="c54f5-192">**Esempi di criteri GLOB**</span><span class="sxs-lookup"><span data-stu-id="c54f5-192">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="c54f5-193">Cerca un file specifico in una directory specifica.</span><span class="sxs-lookup"><span data-stu-id="c54f5-193">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="c54f5-194">Cerca tutti i file con estensione *txt* in una directory specifica.</span><span class="sxs-lookup"><span data-stu-id="c54f5-194">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="c54f5-195">Cerca tutti i file `appsettings.json` nelle directory esattamente un livello sotto la cartella *directory*.</span><span class="sxs-lookup"><span data-stu-id="c54f5-195">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="c54f5-196">Cerca tutti i file con estensione *txt* che si trovano in qualsiasi posizione nella cartella *directory*.</span><span class="sxs-lookup"><span data-stu-id="c54f5-196">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c54f5-197">ASP.NET Core astrae l'accesso al file system tramite l'utilizzo di provider di file.</span><span class="sxs-lookup"><span data-stu-id="c54f5-197">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="c54f5-198">I provider di file vengono usati in tutto il framework di ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="c54f5-198">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="c54f5-199"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> espone la radice del [contenuto](xref:fundamentals/index#content-root) dell'app e la [radice Web](xref:fundamentals/index#web-root) come tipi di `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="c54f5-199"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> exposes the app's [content root](xref:fundamentals/index#content-root) and [web root](xref:fundamentals/index#web-root) as `IFileProvider` types.</span></span>
* <span data-ttu-id="c54f5-200">Il [middleware dei file statici](xref:fundamentals/static-files) usa i provider di file per trovare i file statici.</span><span class="sxs-lookup"><span data-stu-id="c54f5-200">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="c54f5-201">[Razor](xref:mvc/views/razor) usa i provider di file per individuare pagine e viste.</span><span class="sxs-lookup"><span data-stu-id="c54f5-201">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="c54f5-202">Gli strumenti .NET Core usano i provider di file e i criteri GLOB per specificare i file da pubblicare.</span><span class="sxs-lookup"><span data-stu-id="c54f5-202">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="c54f5-203">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c54f5-203">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="c54f5-204">Interfacce del provider di file</span><span class="sxs-lookup"><span data-stu-id="c54f5-204">File Provider interfaces</span></span>

<span data-ttu-id="c54f5-205">L'interfaccia primaria è <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="c54f5-205">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="c54f5-206">`IFileProvider` espone metodi per:</span><span class="sxs-lookup"><span data-stu-id="c54f5-206">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="c54f5-207">Ottenere informazioni sui file (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span><span class="sxs-lookup"><span data-stu-id="c54f5-207">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="c54f5-208">Ottenere informazioni sulle directory (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span><span class="sxs-lookup"><span data-stu-id="c54f5-208">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="c54f5-209">Configurare le notifiche di modifica (usando un <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span><span class="sxs-lookup"><span data-stu-id="c54f5-209">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="c54f5-210">`IFileInfo` fornisce metodi e proprietà per l'uso di file:</span><span class="sxs-lookup"><span data-stu-id="c54f5-210">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="c54f5-211"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in byte)</span><span class="sxs-lookup"><span data-stu-id="c54f5-211"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="c54f5-212">Data <xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified></span><span class="sxs-lookup"><span data-stu-id="c54f5-212"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="c54f5-213">È possibile leggere dal file usando il metodo [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*).</span><span class="sxs-lookup"><span data-stu-id="c54f5-213">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="c54f5-214">L'app di esempio dimostra come configurare un provider di file in `Startup.ConfigureServices` da usare in tutta l'app tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c54f5-214">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="c54f5-215">Implementazioni dei provider di file</span><span class="sxs-lookup"><span data-stu-id="c54f5-215">File Provider implementations</span></span>

<span data-ttu-id="c54f5-216">Sono disponibili tre implementazioni di `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="c54f5-216">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="c54f5-217">Implementazione</span><span class="sxs-lookup"><span data-stu-id="c54f5-217">Implementation</span></span> | <span data-ttu-id="c54f5-218">Descrizione</span><span class="sxs-lookup"><span data-stu-id="c54f5-218">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="c54f5-219">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="c54f5-219">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="c54f5-220">Il provider fisico viene usato per accedere ai file fisici del sistema.</span><span class="sxs-lookup"><span data-stu-id="c54f5-220">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="c54f5-221">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="c54f5-221">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="c54f5-222">Il provider incorporato nel manifesto viene usato per accedere ai file incorporati negli assembly.</span><span class="sxs-lookup"><span data-stu-id="c54f5-222">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="c54f5-223">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="c54f5-223">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="c54f5-224">Il provider composito consente di offrire un accesso combinato a file e directory da uno o più provider.</span><span class="sxs-lookup"><span data-stu-id="c54f5-224">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="c54f5-225">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="c54f5-225">PhysicalFileProvider</span></span>

<span data-ttu-id="c54f5-226"><xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> consente di accedere al file system fisico.</span><span class="sxs-lookup"><span data-stu-id="c54f5-226">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="c54f5-227">`PhysicalFileProvider` usa il tipo <xref:System.IO.File?displayProperty=fullName> (per il provider fisico) e definisce una directory e i relativi elementi figlio come ambito per tutti i percorsi.</span><span class="sxs-lookup"><span data-stu-id="c54f5-227">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="c54f5-228">La definizione dell'ambito impedisce l'accesso al file system al di fuori della directory specificata e dei relativi elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="c54f5-228">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="c54f5-229">Lo scenario più comune per la creazione e l'uso di un `PhysicalFileProvider` consiste nel richiedere un oggetto `IFileProvider` in un costruttore tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c54f5-229">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="c54f5-230">Per la creazione diretta di un'istanza di questo provider, è richiesto un percorso di directory che viene usato come percorso di base per tutte le richieste effettuate tramite il provider.</span><span class="sxs-lookup"><span data-stu-id="c54f5-230">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="c54f5-231">Il codice seguente illustra come creare un `PhysicalFileProvider` e usarlo per ottenere il contenuto della directory e informazioni sui file:</span><span class="sxs-lookup"><span data-stu-id="c54f5-231">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="c54f5-232">Tipi nell'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="c54f5-232">Types in the preceding example:</span></span>

* <span data-ttu-id="c54f5-233">`provider` è `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="c54f5-233">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="c54f5-234">`contents` è `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="c54f5-234">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="c54f5-235">`fileInfo` è `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="c54f5-235">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="c54f5-236">Il provider di file può essere usato per scorrere la directory specificata da `applicationRoot` oppure per chiamare `GetFileInfo` per ottenere informazioni su un file.</span><span class="sxs-lookup"><span data-stu-id="c54f5-236">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="c54f5-237">Il provider di file non ha accesso all'esterno della directory `applicationRoot`.</span><span class="sxs-lookup"><span data-stu-id="c54f5-237">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="c54f5-238">L'app di esempio crea il provider nella classe `Startup.ConfigureServices` dell'app usando [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span><span class="sxs-lookup"><span data-stu-id="c54f5-238">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="c54f5-239">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="c54f5-239">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="c54f5-240"><xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> consente di accedere ai file incorporati negli assembly.</span><span class="sxs-lookup"><span data-stu-id="c54f5-240">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="c54f5-241">`ManifestEmbeddedFileProvider` usa un manifesto compilato nell'assembly per ricostruire i percorsi originali dei file incorporati.</span><span class="sxs-lookup"><span data-stu-id="c54f5-241">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="c54f5-242">Per generare un manifesto dei file incorporati, impostare la proprietà `<GenerateEmbeddedFilesManifest>` su `true`.</span><span class="sxs-lookup"><span data-stu-id="c54f5-242">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="c54f5-243">Specificare i file da incorporare con [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span><span class="sxs-lookup"><span data-stu-id="c54f5-243">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="c54f5-244">Usare [modelli GLOB](#glob-patterns) per specificare uno o più file da incorporare nell'assembly.</span><span class="sxs-lookup"><span data-stu-id="c54f5-244">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="c54f5-245">L'app di esempio crea un `ManifestEmbeddedFileProvider` e passa l'assembly attualmente in esecuzione al rispettivo costruttore.</span><span class="sxs-lookup"><span data-stu-id="c54f5-245">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="c54f5-246">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c54f5-246">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(typeof(Program).Assembly);
```

<span data-ttu-id="c54f5-247">Overload aggiuntivi consentono di:</span><span class="sxs-lookup"><span data-stu-id="c54f5-247">Additional overloads allow you to:</span></span>

* <span data-ttu-id="c54f5-248">Specificare un percorso di file relativo.</span><span class="sxs-lookup"><span data-stu-id="c54f5-248">Specify a relative file path.</span></span>
* <span data-ttu-id="c54f5-249">Definire la data dell'ultima modifica come ambito dei file.</span><span class="sxs-lookup"><span data-stu-id="c54f5-249">Scope files to a last modified date.</span></span>
* <span data-ttu-id="c54f5-250">Assegnare un nome alla risorsa incorporata contenente il manifesto dei file incorporati.</span><span class="sxs-lookup"><span data-stu-id="c54f5-250">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="c54f5-251">Overload</span><span class="sxs-lookup"><span data-stu-id="c54f5-251">Overload</span></span> | <span data-ttu-id="c54f5-252">Descrizione</span><span class="sxs-lookup"><span data-stu-id="c54f5-252">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="c54f5-253">Accetta un parametro di percorso relativo `root` facoltativo.</span><span class="sxs-lookup"><span data-stu-id="c54f5-253">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="c54f5-254">Specificare `root` per definire come ambito delle chiamate a <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> le risorse incluse nel percorso specificato.</span><span class="sxs-lookup"><span data-stu-id="c54f5-254">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="c54f5-255">Accetta un parametro di percorso relativo `root` facoltativo e un parametro di data `lastModified` (<xref:System.DateTimeOffset>).</span><span class="sxs-lookup"><span data-stu-id="c54f5-255">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="c54f5-256">La data `lastModified` definisce la data dell'ultima modifica come ambito per le istanze di <xref:Microsoft.Extensions.FileProviders.IFileInfo> restituite da <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="c54f5-256">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="c54f5-257">Accetta un parametro di percorso relativo `root` facoltativo, un parametro di data `lastModified` e il parametro `manifestName`.</span><span class="sxs-lookup"><span data-stu-id="c54f5-257">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="c54f5-258">`manifestName` rappresenta il nome della risorsa incorporata contenente il manifesto.</span><span class="sxs-lookup"><span data-stu-id="c54f5-258">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="c54f5-259">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="c54f5-259">CompositeFileProvider</span></span>

<span data-ttu-id="c54f5-260"><xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combina le istanze `IFileProvider` esponendo una sola interfaccia per l'uso dei file di più provider.</span><span class="sxs-lookup"><span data-stu-id="c54f5-260">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="c54f5-261">Quando si crea `CompositeFileProvider`, passare una o più istanze `IFileProvider` al relativo costruttore.</span><span class="sxs-lookup"><span data-stu-id="c54f5-261">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="c54f5-262">Nell'app di esempio, un `PhysicalFileProvider` e un `ManifestEmbeddedFileProvider` forniscono i file a un `CompositeFileProvider` registrato nel contenitore dei servizi dell'app:</span><span class="sxs-lookup"><span data-stu-id="c54f5-262">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="c54f5-263">Controllo delle modifiche</span><span class="sxs-lookup"><span data-stu-id="c54f5-263">Watch for changes</span></span>

<span data-ttu-id="c54f5-264">Il metodo [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) consente di controllare uno o più file o directory per il rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="c54f5-264">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="c54f5-265">`Watch` accetta una stringa di percorso in cui è possibile usare [criteri GLOB](#glob-patterns) per specificare più file.</span><span class="sxs-lookup"><span data-stu-id="c54f5-265">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="c54f5-266">`Watch` restituisce un oggetto <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span><span class="sxs-lookup"><span data-stu-id="c54f5-266">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="c54f5-267">Il token di modifica espone:</span><span class="sxs-lookup"><span data-stu-id="c54f5-267">The change token exposes:</span></span>

* <span data-ttu-id="c54f5-268"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; Una proprietà che può essere ispezionata per determinare se è stata apportata una modifica.</span><span class="sxs-lookup"><span data-stu-id="c54f5-268"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="c54f5-269"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Chiamato quando vengono rilevate modifiche alla stringa di percorso specificata.</span><span class="sxs-lookup"><span data-stu-id="c54f5-269"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="c54f5-270">Ogni token di modifica chiama solo il relativo callback associato in risposta a una singola modifica.</span><span class="sxs-lookup"><span data-stu-id="c54f5-270">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="c54f5-271">Per abilitare un monitoraggio continuo, usare <xref:System.Threading.Tasks.TaskCompletionSource`1> come illustrato di seguito oppure ricreare istanze di `IChangeToken` in risposta alle modifiche.</span><span class="sxs-lookup"><span data-stu-id="c54f5-271">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="c54f5-272">Nell'app di esempio, l'app console *WatchConsole* viene configurata per visualizzare un messaggio quando viene modificato un file di testo:</span><span class="sxs-lookup"><span data-stu-id="c54f5-272">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="c54f5-273">È possibile che alcuni file system, ad esempio i contenitori Docker e le condivisioni di rete, non inviino sempre le notifiche di modifica.</span><span class="sxs-lookup"><span data-stu-id="c54f5-273">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="c54f5-274">Impostare la variabile di ambiente `DOTNET_USE_POLLING_FILE_WATCHER` su `1` o `true` per eseguire il polling delle modifiche nel file system ogni quattro secondi (intervallo non configurabile).</span><span class="sxs-lookup"><span data-stu-id="c54f5-274">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="c54f5-275">Modelli GLOB</span><span class="sxs-lookup"><span data-stu-id="c54f5-275">Glob patterns</span></span>

<span data-ttu-id="c54f5-276">Nei percorsi del file system vengono usati criteri con caratteri jolly chiamati *criteri GLOB (o globbing)* .</span><span class="sxs-lookup"><span data-stu-id="c54f5-276">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="c54f5-277">Specificare gruppi di file con questi criteri.</span><span class="sxs-lookup"><span data-stu-id="c54f5-277">Specify groups of files with these patterns.</span></span> <span data-ttu-id="c54f5-278">I due caratteri jolly sono `*` e `**`:</span><span class="sxs-lookup"><span data-stu-id="c54f5-278">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="c54f5-279">Cerca qualsiasi elemento a livello della cartella corrente, qualsiasi nome di file o qualsiasi estensione di file.</span><span class="sxs-lookup"><span data-stu-id="c54f5-279">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="c54f5-280">Le corrispondenze vengono terminate con i caratteri `/` e `.` nel percorso dei file.</span><span class="sxs-lookup"><span data-stu-id="c54f5-280">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="c54f5-281">Cerca qualsiasi elemento in più livelli di directory.</span><span class="sxs-lookup"><span data-stu-id="c54f5-281">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="c54f5-282">Può essere usato per cercare in modo ricorsivo numerosi file all'interno di una gerarchia di directory.</span><span class="sxs-lookup"><span data-stu-id="c54f5-282">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="c54f5-283">**Esempi di criteri GLOB**</span><span class="sxs-lookup"><span data-stu-id="c54f5-283">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="c54f5-284">Cerca un file specifico in una directory specifica.</span><span class="sxs-lookup"><span data-stu-id="c54f5-284">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="c54f5-285">Cerca tutti i file con estensione *txt* in una directory specifica.</span><span class="sxs-lookup"><span data-stu-id="c54f5-285">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="c54f5-286">Cerca tutti i file `appsettings.json` nelle directory esattamente un livello sotto la cartella *directory*.</span><span class="sxs-lookup"><span data-stu-id="c54f5-286">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="c54f5-287">Cerca tutti i file con estensione *txt* che si trovano in qualsiasi posizione nella cartella *directory*.</span><span class="sxs-lookup"><span data-stu-id="c54f5-287">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end
