---
title: Provider di file in ASP.NET Core
author: guardrex
description: Informazioni su come ASP.NET Core astrae l'accesso al file system tramite l'utilizzo di provider di file.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/30/2019
uid: fundamentals/file-providers
ms.openlocfilehash: 2ce40ea0d576d08a6b42c3eb6693754f2a0bddce
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/02/2019
ms.locfileid: "58809221"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="521fa-103">Provider di file in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="521fa-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="521fa-104">Di [Steve Smith](https://ardalis.com/) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="521fa-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="521fa-105">ASP.NET Core astrae l'accesso al file system tramite l'utilizzo di provider di file.</span><span class="sxs-lookup"><span data-stu-id="521fa-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="521fa-106">I provider di file vengono usati in tutto il framework di ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="521fa-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="521fa-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) espone la radice del contenuto dell'app e la radice Web come tipi `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="521fa-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) exposes the app's content root and web root as `IFileProvider` types.</span></span>
* <span data-ttu-id="521fa-108">Il [middleware dei file statici](xref:fundamentals/static-files) usa i provider di file per trovare i file statici.</span><span class="sxs-lookup"><span data-stu-id="521fa-108">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="521fa-109">[Razor](xref:mvc/views/razor) usa i provider di file per individuare pagine e viste.</span><span class="sxs-lookup"><span data-stu-id="521fa-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="521fa-110">Gli strumenti .NET Core usano i provider di file e i criteri GLOB per specificare i file da pubblicare.</span><span class="sxs-lookup"><span data-stu-id="521fa-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="521fa-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="521fa-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="521fa-112">Interfacce del provider di file</span><span class="sxs-lookup"><span data-stu-id="521fa-112">File Provider interfaces</span></span>

<span data-ttu-id="521fa-113">L'interfaccia primaria è [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span><span class="sxs-lookup"><span data-stu-id="521fa-113">The primary interface is [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> <span data-ttu-id="521fa-114">`IFileProvider` espone metodi per:</span><span class="sxs-lookup"><span data-stu-id="521fa-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="521fa-115">Ottenere informazioni sui file ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span><span class="sxs-lookup"><span data-stu-id="521fa-115">Obtain file information ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span></span>
* <span data-ttu-id="521fa-116">Ottenere informazioni sulla directory ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span><span class="sxs-lookup"><span data-stu-id="521fa-116">Obtain directory information ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span></span>
* <span data-ttu-id="521fa-117">Configurare le notifiche di modifica (usando un [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span><span class="sxs-lookup"><span data-stu-id="521fa-117">Set up change notifications (using an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span></span>

<span data-ttu-id="521fa-118">`IFileInfo` fornisce metodi e proprietà per l'uso di file:</span><span class="sxs-lookup"><span data-stu-id="521fa-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* [<span data-ttu-id="521fa-119">Exists</span><span class="sxs-lookup"><span data-stu-id="521fa-119">Exists</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.exists)
* [<span data-ttu-id="521fa-120">IsDirectory</span><span class="sxs-lookup"><span data-stu-id="521fa-120">IsDirectory</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.isdirectory)
* [<span data-ttu-id="521fa-121">Name</span><span class="sxs-lookup"><span data-stu-id="521fa-121">Name</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.name)
* <span data-ttu-id="521fa-122">[Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (in byte)</span><span class="sxs-lookup"><span data-stu-id="521fa-122">[Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (in bytes)</span></span>
* <span data-ttu-id="521fa-123">Data [LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified)</span><span class="sxs-lookup"><span data-stu-id="521fa-123">[LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) date</span></span>

<span data-ttu-id="521fa-124">È possibile leggere dal file usando il metodo [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream).</span><span class="sxs-lookup"><span data-stu-id="521fa-124">You can read from the file using the [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) method.</span></span>

<span data-ttu-id="521fa-125">L'app di esempio dimostra come configurare un provider di file in `Startup.ConfigureServices` da usare in tutta l'app tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="521fa-125">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="521fa-126">Implementazioni dei provider di file</span><span class="sxs-lookup"><span data-stu-id="521fa-126">File Provider implementations</span></span>

<span data-ttu-id="521fa-127">Sono disponibili tre implementazioni di `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="521fa-127">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="521fa-128">Implementazione</span><span class="sxs-lookup"><span data-stu-id="521fa-128">Implementation</span></span> | <span data-ttu-id="521fa-129">Description</span><span class="sxs-lookup"><span data-stu-id="521fa-129">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="521fa-130">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="521fa-130">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="521fa-131">Il provider fisico viene usato per accedere ai file fisici del sistema.</span><span class="sxs-lookup"><span data-stu-id="521fa-131">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="521fa-132">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="521fa-132">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="521fa-133">Il provider incorporato nel manifesto viene usato per accedere ai file incorporati negli assembly.</span><span class="sxs-lookup"><span data-stu-id="521fa-133">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="521fa-134">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="521fa-134">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="521fa-135">Il provider composito consente di offrire un accesso combinato a file e directory da uno o più provider.</span><span class="sxs-lookup"><span data-stu-id="521fa-135">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="521fa-136">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="521fa-136">PhysicalFileProvider</span></span>

<span data-ttu-id="521fa-137">[PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) consente di accedere al file system fisico.</span><span class="sxs-lookup"><span data-stu-id="521fa-137">The [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) provides access to the physical file system.</span></span> <span data-ttu-id="521fa-138">`PhysicalFileProvider` usa il tipo [System.IO.File](/dotnet/api/system.io.file) (per il provider fisico) e definisce una directory e i relativi elementi figlio come ambito per tutti i percorsi.</span><span class="sxs-lookup"><span data-stu-id="521fa-138">`PhysicalFileProvider` uses the [System.IO.File](/dotnet/api/system.io.file) type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="521fa-139">La definizione dell'ambito impedisce l'accesso al file system al di fuori della directory specificata e dei relativi elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="521fa-139">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="521fa-140">Per la creazione di un'istanza di questo provider, è richiesto un percorso di directory che viene usato come percorso di base per tutte le richieste effettuate tramite il provider.</span><span class="sxs-lookup"><span data-stu-id="521fa-140">When instantiating this provider, a directory path is required and serves as the base path for all requests made using the provider.</span></span> <span data-ttu-id="521fa-141">È possibile creare direttamente un'istanza di un provider `PhysicalFileProvider` oppure richiedere un `IFileProvider` in un costruttore tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="521fa-141">You can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="521fa-142">**Tipi statici**</span><span class="sxs-lookup"><span data-stu-id="521fa-142">**Static types**</span></span>

<span data-ttu-id="521fa-143">Il codice seguente illustra come creare un `PhysicalFileProvider` e usarlo per ottenere il contenuto della directory e informazioni sui file:</span><span class="sxs-lookup"><span data-stu-id="521fa-143">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="521fa-144">Tipi nell'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="521fa-144">Types in the preceding example:</span></span>

* <span data-ttu-id="521fa-145">`provider` è `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="521fa-145">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="521fa-146">`contents` è `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="521fa-146">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="521fa-147">`fileInfo` è `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="521fa-147">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="521fa-148">Il provider di file può essere usato per scorrere la directory specificata da `applicationRoot` oppure per chiamare `GetFileInfo` per ottenere informazioni su un file.</span><span class="sxs-lookup"><span data-stu-id="521fa-148">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="521fa-149">Il provider di file non ha accesso all'esterno della directory `applicationRoot`.</span><span class="sxs-lookup"><span data-stu-id="521fa-149">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="521fa-150">L'app di esempio crea il provider nella classe `Startup.ConfigureServices` dell'app usando [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span><span class="sxs-lookup"><span data-stu-id="521fa-150">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

<span data-ttu-id="521fa-151">**Ottenere i tipi di provider di file con inserimento delle dipendenze**</span><span class="sxs-lookup"><span data-stu-id="521fa-151">**Obtain File Provider types with dependency injection**</span></span>

<span data-ttu-id="521fa-152">Inserire il provider in un costruttore di classe e assegnarlo a un campo locale.</span><span class="sxs-lookup"><span data-stu-id="521fa-152">Inject the provider into any class constructor and assign it to a local field.</span></span> <span data-ttu-id="521fa-153">Usare il campo in tutti i metodi della classe per accedere ai file.</span><span class="sxs-lookup"><span data-stu-id="521fa-153">Use the field throughout the class's methods to access files.</span></span>

<span data-ttu-id="521fa-154">Nell'app di esempio, la classe `IndexModel` riceve un'istanza di `IFileProvider` per ottenere il contenuto della directory per il percorso di base dell'app.</span><span class="sxs-lookup"><span data-stu-id="521fa-154">In the sample app, the `IndexModel` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="521fa-155">*Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="521fa-155">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="521fa-156">Viene eseguita l'iterazione di `IDirectoryContents` nella pagina.</span><span class="sxs-lookup"><span data-stu-id="521fa-156">The `IDirectoryContents` are iterated in the page.</span></span>

<span data-ttu-id="521fa-157">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="521fa-157">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml?name=snippet1)]

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="521fa-158">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="521fa-158">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="521fa-159">[ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) viene usato per accedere ai file incorporati negli assembly.</span><span class="sxs-lookup"><span data-stu-id="521fa-159">The [ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="521fa-160">`ManifestEmbeddedFileProvider` usa un manifesto compilato nell'assembly per ricostruire i percorsi originali dei file incorporati.</span><span class="sxs-lookup"><span data-stu-id="521fa-160">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="521fa-161">Per generare un manifesto dei file incorporati, impostare la proprietà `<GenerateEmbeddedFilesManifest>` su `true`.</span><span class="sxs-lookup"><span data-stu-id="521fa-161">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="521fa-162">Specificare i file da incorporare con [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span><span class="sxs-lookup"><span data-stu-id="521fa-162">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

<span data-ttu-id="521fa-163">Usare [modelli GLOB](#glob-patterns) per specificare uno o più file da incorporare nell'assembly.</span><span class="sxs-lookup"><span data-stu-id="521fa-163">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="521fa-164">L'app di esempio crea un `ManifestEmbeddedFileProvider` e passa l'assembly attualmente in esecuzione al rispettivo costruttore.</span><span class="sxs-lookup"><span data-stu-id="521fa-164">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="521fa-165">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="521fa-165">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="521fa-166">Overload aggiuntivi consentono di:</span><span class="sxs-lookup"><span data-stu-id="521fa-166">Additional overloads allow you to:</span></span>

* <span data-ttu-id="521fa-167">Specificare un percorso di file relativo.</span><span class="sxs-lookup"><span data-stu-id="521fa-167">Specify a relative file path.</span></span>
* <span data-ttu-id="521fa-168">Definire la data dell'ultima modifica come ambito dei file.</span><span class="sxs-lookup"><span data-stu-id="521fa-168">Scope files to a last modified date.</span></span>
* <span data-ttu-id="521fa-169">Assegnare un nome alla risorsa incorporata contenente il manifesto dei file incorporati.</span><span class="sxs-lookup"><span data-stu-id="521fa-169">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="521fa-170">Overload</span><span class="sxs-lookup"><span data-stu-id="521fa-170">Overload</span></span> | <span data-ttu-id="521fa-171">Description</span><span class="sxs-lookup"><span data-stu-id="521fa-171">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="521fa-172">ManifestEmbeddedFileProvider(Assembly, String)</span><span class="sxs-lookup"><span data-stu-id="521fa-172">ManifestEmbeddedFileProvider(Assembly, String)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_) | <span data-ttu-id="521fa-173">Accetta un parametro di percorso relativo `root` facoltativo.</span><span class="sxs-lookup"><span data-stu-id="521fa-173">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="521fa-174">Specificare `root` per definire come ambito delle chiamate a [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) le risorse incluse nel percorso specificato.</span><span class="sxs-lookup"><span data-stu-id="521fa-174">Specify the `root` to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided path.</span></span> |
| [<span data-ttu-id="521fa-175">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="521fa-175">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_DateTimeOffset_) | <span data-ttu-id="521fa-176">Accetta un parametro di percorso relativo `root` facoltativo e un parametro di data `lastModified` ([DateTimeOffset](/dotnet/api/system.datetimeoffset)).</span><span class="sxs-lookup"><span data-stu-id="521fa-176">Accepts an optional `root` relative path parameter and a `lastModified` date ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) parameter.</span></span> <span data-ttu-id="521fa-177">La data `lastModified` definisce la data dell'ultima modifica come ambito per le istanze [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) restituite da [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span><span class="sxs-lookup"><span data-stu-id="521fa-177">The `lastModified` date scopes the last modification date for the [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) instances returned by the [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> |
| [<span data-ttu-id="521fa-178">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="521fa-178">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_String_System_DateTimeOffset_) | <span data-ttu-id="521fa-179">Accetta un parametro di percorso relativo `root` facoltativo, un parametro di data `lastModified` e il parametro `manifestName`.</span><span class="sxs-lookup"><span data-stu-id="521fa-179">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="521fa-180">`manifestName` rappresenta il nome della risorsa incorporata contenente il manifesto.</span><span class="sxs-lookup"><span data-stu-id="521fa-180">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="521fa-181">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="521fa-181">CompositeFileProvider</span></span>

<span data-ttu-id="521fa-182">[CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) combina le istanze di `IFileProvider` esponendo una singola interfaccia per l'utilizzo dei file da più provider.</span><span class="sxs-lookup"><span data-stu-id="521fa-182">The [CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="521fa-183">Quando si crea `CompositeFileProvider`, passare una o più istanze `IFileProvider` al relativo costruttore.</span><span class="sxs-lookup"><span data-stu-id="521fa-183">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="521fa-184">Nell'app di esempio, un `PhysicalFileProvider` e un `ManifestEmbeddedFileProvider` forniscono i file a un `CompositeFileProvider` registrato nel contenitore dei servizi dell'app:</span><span class="sxs-lookup"><span data-stu-id="521fa-184">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="521fa-185">Controllo delle modifiche</span><span class="sxs-lookup"><span data-stu-id="521fa-185">Watch for changes</span></span>

<span data-ttu-id="521fa-186">Il metodo [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) consente di controllare uno o più file o directory per il rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="521fa-186">The [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="521fa-187">`Watch` accetta una stringa di percorso in cui è possibile usare [criteri GLOB](#glob-patterns) per specificare più file.</span><span class="sxs-lookup"><span data-stu-id="521fa-187">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="521fa-188">`Watch` restituisce un [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span><span class="sxs-lookup"><span data-stu-id="521fa-188">`Watch` returns an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span></span> <span data-ttu-id="521fa-189">Il token di modifica espone:</span><span class="sxs-lookup"><span data-stu-id="521fa-189">The change token exposes:</span></span>

* <span data-ttu-id="521fa-190">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): una proprietà che può essere ispezionata per determinare se è stata apportata una modifica.</span><span class="sxs-lookup"><span data-stu-id="521fa-190">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="521fa-191">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): chiamato quando vengono rilevate modifiche alla stringa di percorso specificata.</span><span class="sxs-lookup"><span data-stu-id="521fa-191">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="521fa-192">Ogni token di modifica chiama solo il relativo callback associato in risposta a una singola modifica.</span><span class="sxs-lookup"><span data-stu-id="521fa-192">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="521fa-193">Per abilitare un monitoraggio continuo, usare [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) come illustrato di seguito oppure ricreare istanze di `IChangeToken` in risposta alle modifiche.</span><span class="sxs-lookup"><span data-stu-id="521fa-193">To enable constant monitoring, use a [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="521fa-194">Nell'app di esempio, l'app console *WatchConsole* viene configurata per visualizzare un messaggio quando viene modificato un file di testo:</span><span class="sxs-lookup"><span data-stu-id="521fa-194">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="521fa-195">È possibile che alcuni file system, ad esempio i contenitori Docker e le condivisioni di rete, non inviino sempre le notifiche di modifica.</span><span class="sxs-lookup"><span data-stu-id="521fa-195">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="521fa-196">Impostare la variabile di ambiente `DOTNET_USE_POLLING_FILE_WATCHER` su `1` o `true` per eseguire il polling delle modifiche nel file system ogni quattro secondi (intervallo non configurabile).</span><span class="sxs-lookup"><span data-stu-id="521fa-196">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="521fa-197">Modelli GLOB</span><span class="sxs-lookup"><span data-stu-id="521fa-197">Glob patterns</span></span>

<span data-ttu-id="521fa-198">Nei percorsi del file system vengono usati criteri con caratteri jolly chiamati *criteri GLOB (o globbing)*.</span><span class="sxs-lookup"><span data-stu-id="521fa-198">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="521fa-199">Specificare gruppi di file con questi criteri.</span><span class="sxs-lookup"><span data-stu-id="521fa-199">Specify groups of files with these patterns.</span></span> <span data-ttu-id="521fa-200">I due caratteri jolly sono `*` e `**`:</span><span class="sxs-lookup"><span data-stu-id="521fa-200">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="521fa-201">Cerca qualsiasi elemento a livello della cartella corrente, qualsiasi nome di file o qualsiasi estensione di file.</span><span class="sxs-lookup"><span data-stu-id="521fa-201">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="521fa-202">Le corrispondenze vengono terminate con i caratteri `/` e `.` nel percorso dei file.</span><span class="sxs-lookup"><span data-stu-id="521fa-202">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="521fa-203">Cerca qualsiasi elemento in più livelli di directory.</span><span class="sxs-lookup"><span data-stu-id="521fa-203">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="521fa-204">Può essere usato per cercare in modo ricorsivo numerosi file all'interno di una gerarchia di directory.</span><span class="sxs-lookup"><span data-stu-id="521fa-204">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="521fa-205">**Esempi di criteri GLOB**</span><span class="sxs-lookup"><span data-stu-id="521fa-205">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="521fa-206">Cerca un file specifico in una directory specifica.</span><span class="sxs-lookup"><span data-stu-id="521fa-206">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="521fa-207">Cerca tutti i file con estensione *txt* in una directory specifica.</span><span class="sxs-lookup"><span data-stu-id="521fa-207">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="521fa-208">Cerca tutti i file `appsettings.json` nelle directory esattamente un livello sotto la cartella *directory*.</span><span class="sxs-lookup"><span data-stu-id="521fa-208">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="521fa-209">Cerca tutti i file con estensione *txt* che si trovano in qualsiasi posizione nella cartella *directory*.</span><span class="sxs-lookup"><span data-stu-id="521fa-209">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>
