---
title: Provider di file in ASP.NET Core
author: guardrex
description: Informazioni su come ASP.NET Core astrae l'accesso al file system tramite l'utilizzo di provider di file.
ms.author: riande
ms.custom: mvc
ms.date: 08/01/2018
uid: fundamentals/file-providers
ms.openlocfilehash: a0d326f5fc995cb903380315879d39a8ce851d06
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913216"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="01657-103">Provider di file in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="01657-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="01657-104">Di [Steve Smith](https://ardalis.com/) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="01657-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="01657-105">ASP.NET Core astrae l'accesso al file system tramite l'utilizzo di provider di file.</span><span class="sxs-lookup"><span data-stu-id="01657-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="01657-106">I provider di file vengono usati in tutto il framework di ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="01657-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="01657-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) espone la radice del contenuto dell'app e la radice Web come tipi `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="01657-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) exposes the app's content root and web root as `IFileProvider` types.</span></span>
* <span data-ttu-id="01657-108">Il [middleware dei file statici](xref:fundamentals/static-files) usa i provider di file per individuare i file statici.</span><span class="sxs-lookup"><span data-stu-id="01657-108">[Static Files Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="01657-109">[Razor](xref:mvc/views/razor) usa i provider di file per individuare pagine e viste.</span><span class="sxs-lookup"><span data-stu-id="01657-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="01657-110">Gli strumenti .NET Core usano i provider di file e i criteri GLOB per specificare i file da pubblicare.</span><span class="sxs-lookup"><span data-stu-id="01657-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="01657-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="01657-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="01657-112">Interfacce del provider di file</span><span class="sxs-lookup"><span data-stu-id="01657-112">File Provider interfaces</span></span>

<span data-ttu-id="01657-113">L'interfaccia primaria è [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span><span class="sxs-lookup"><span data-stu-id="01657-113">The primary interface is [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> <span data-ttu-id="01657-114">`IFileProvider` espone metodi per:</span><span class="sxs-lookup"><span data-stu-id="01657-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="01657-115">Ottenere informazioni sui file ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span><span class="sxs-lookup"><span data-stu-id="01657-115">Obtain file information ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span></span>
* <span data-ttu-id="01657-116">Ottenere informazioni sulla directory ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span><span class="sxs-lookup"><span data-stu-id="01657-116">Obtain directory information ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span></span>
* <span data-ttu-id="01657-117">Configurare le notifiche di modifica (usando un [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span><span class="sxs-lookup"><span data-stu-id="01657-117">Set up change notifications (using an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span></span>

<span data-ttu-id="01657-118">`IFileInfo` fornisce metodi e proprietà per l'uso di file:</span><span class="sxs-lookup"><span data-stu-id="01657-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* [<span data-ttu-id="01657-119">Exists</span><span class="sxs-lookup"><span data-stu-id="01657-119">Exists</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.exists)
* [<span data-ttu-id="01657-120">IsDirectory</span><span class="sxs-lookup"><span data-stu-id="01657-120">IsDirectory</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.isdirectory)
* [<span data-ttu-id="01657-121">Name</span><span class="sxs-lookup"><span data-stu-id="01657-121">Name</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.name)
* <span data-ttu-id="01657-122">[Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (in byte)</span><span class="sxs-lookup"><span data-stu-id="01657-122">[Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (in bytes)</span></span>
* <span data-ttu-id="01657-123">Data [LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified)</span><span class="sxs-lookup"><span data-stu-id="01657-123">[LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) date</span></span>

<span data-ttu-id="01657-124">È possibile leggere dal file usando il metodo [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream).</span><span class="sxs-lookup"><span data-stu-id="01657-124">You can read from the file using the [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) method.</span></span>

<span data-ttu-id="01657-125">L'app di esempio dimostra come configurare un provider di file in `Startup.ConfigureServices` da usare in tutta l'app tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="01657-125">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="01657-126">Implementazioni dei provider di file</span><span class="sxs-lookup"><span data-stu-id="01657-126">File Provider implementations</span></span>

<span data-ttu-id="01657-127">Sono disponibili tre implementazioni di `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="01657-127">Three implementations of `IFileProvider` are available.</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="01657-128">Implementazione</span><span class="sxs-lookup"><span data-stu-id="01657-128">Implementation</span></span> | <span data-ttu-id="01657-129">Descrizione</span><span class="sxs-lookup"><span data-stu-id="01657-129">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="01657-130">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="01657-130">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="01657-131">Il provider fisico viene usato per accedere ai file fisici del sistema.</span><span class="sxs-lookup"><span data-stu-id="01657-131">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="01657-132">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="01657-132">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="01657-133">Il provider incorporato nel manifesto viene usato per accedere ai file incorporati negli assembly.</span><span class="sxs-lookup"><span data-stu-id="01657-133">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="01657-134">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="01657-134">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="01657-135">Il provider composito consente di offrire un accesso combinato a file e directory da uno o più provider.</span><span class="sxs-lookup"><span data-stu-id="01657-135">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="01657-136">Implementazione</span><span class="sxs-lookup"><span data-stu-id="01657-136">Implementation</span></span> | <span data-ttu-id="01657-137">Descrizione</span><span class="sxs-lookup"><span data-stu-id="01657-137">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="01657-138">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="01657-138">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="01657-139">Il provider fisico viene usato per accedere ai file fisici del sistema.</span><span class="sxs-lookup"><span data-stu-id="01657-139">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="01657-140">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="01657-140">EmbeddedFileProvider</span></span>](#embeddedfileprovider) | <span data-ttu-id="01657-141">Il provider incorporato consente di accedere ai file incorporati negli assembly.</span><span class="sxs-lookup"><span data-stu-id="01657-141">The embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="01657-142">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="01657-142">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="01657-143">Il provider composito consente di offrire un accesso combinato a file e directory da uno o più provider.</span><span class="sxs-lookup"><span data-stu-id="01657-143">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

::: moniker-end

### <a name="physicalfileprovider"></a><span data-ttu-id="01657-144">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="01657-144">PhysicalFileProvider</span></span>

<span data-ttu-id="01657-145">[PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) consente di accedere al file system fisico.</span><span class="sxs-lookup"><span data-stu-id="01657-145">The [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) provides access to the physical file system.</span></span> <span data-ttu-id="01657-146">`PhysicalFileProvider` usa il tipo [System.IO.File](/dotnet/api/system.io.file) (per il provider fisico) e definisce una directory e i relativi elementi figlio come ambito per tutti i percorsi.</span><span class="sxs-lookup"><span data-stu-id="01657-146">`PhysicalFileProvider` uses the [System.IO.File](/dotnet/api/system.io.file) type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="01657-147">La definizione dell'ambito impedisce l'accesso al file system al di fuori della directory specificata e dei relativi elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="01657-147">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="01657-148">Per la creazione di un'istanza di questo provider, è richiesto un percorso di directory che viene usato come percorso di base per tutte le richieste effettuate tramite il provider.</span><span class="sxs-lookup"><span data-stu-id="01657-148">When instantiating this provider, a directory path is required and serves as the base path for all requests made using the provider.</span></span> <span data-ttu-id="01657-149">È possibile creare direttamente un'istanza di un provider `PhysicalFileProvider` oppure richiedere un `IFileProvider` in un costruttore tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="01657-149">You can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="01657-150">**Tipi statici**</span><span class="sxs-lookup"><span data-stu-id="01657-150">**Static types**</span></span>

<span data-ttu-id="01657-151">Il codice seguente illustra come creare un `PhysicalFileProvider` e usarlo per ottenere il contenuto della directory e informazioni sui file:</span><span class="sxs-lookup"><span data-stu-id="01657-151">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="01657-152">Tipi nell'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="01657-152">Types in the preceding example:</span></span>

* <span data-ttu-id="01657-153">`provider` è `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="01657-153">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="01657-154">`contents` è `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="01657-154">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="01657-155">`fileInfo` è `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="01657-155">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="01657-156">Il provider di file può essere usato per scorrere la directory specificata da `applicationRoot` oppure per chiamare `GetFileInfo` per ottenere informazioni su un file.</span><span class="sxs-lookup"><span data-stu-id="01657-156">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="01657-157">Il provider di file non ha accesso all'esterno della directory `applicationRoot`.</span><span class="sxs-lookup"><span data-stu-id="01657-157">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="01657-158">L'app di esempio crea il provider nella classe `Startup.ConfigureServices` dell'app usando [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span><span class="sxs-lookup"><span data-stu-id="01657-158">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

<span data-ttu-id="01657-159">**Ottenere i tipi di provider di file con inserimento delle dipendenze**</span><span class="sxs-lookup"><span data-stu-id="01657-159">**Obtain File Provider types with dependency injection**</span></span>

<span data-ttu-id="01657-160">Inserire il provider in un costruttore di classe e assegnarlo a un campo locale.</span><span class="sxs-lookup"><span data-stu-id="01657-160">Inject the provider into any class constructor and assign it to a local field.</span></span> <span data-ttu-id="01657-161">Usare il campo in tutti i metodi della classe per accedere ai file.</span><span class="sxs-lookup"><span data-stu-id="01657-161">Use the field throughout the class's methods to access files.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="01657-162">Nell'app di esempio, la classe `IndexModel` riceve un'istanza di `IFileProvider` per ottenere il contenuto della directory per il percorso di base dell'app.</span><span class="sxs-lookup"><span data-stu-id="01657-162">In the sample app, the `IndexModel` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="01657-163">*Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="01657-163">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="01657-164">Viene eseguita l'iterazione di `IDirectoryContents` nella pagina.</span><span class="sxs-lookup"><span data-stu-id="01657-164">The `IDirectoryContents` are iterated in the page.</span></span>

<span data-ttu-id="01657-165">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="01657-165">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="01657-166">Nell'app di esempio, la classe `HomeController` riceve un'istanza di `IFileProvider` per ottenere il contenuto della directory per il percorso di base dell'app.</span><span class="sxs-lookup"><span data-stu-id="01657-166">In the sample app, the `HomeController` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="01657-167">*Controllers/HomeController.cs*:</span><span class="sxs-lookup"><span data-stu-id="01657-167">*Controllers/HomeController.cs*:</span></span>

[!code-csharp[](file-providers/samples/1.x/FileProviderSample/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="01657-168">Viene eseguita l'iterazione di `IDirectoryContents` nella vista.</span><span class="sxs-lookup"><span data-stu-id="01657-168">The `IDirectoryContents` are iterated in the view.</span></span>

<span data-ttu-id="01657-169">*Views/Home/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="01657-169">*Views/Home/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/1.x/FileProviderSample/Views/Home/Index.cshtml?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="01657-170">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="01657-170">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="01657-171">[ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) viene usato per accedere ai file incorporati negli assembly.</span><span class="sxs-lookup"><span data-stu-id="01657-171">The [ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="01657-172">`ManifestEmbeddedFileProvider` usa un manifesto compilato nell'assembly per ricostruire i percorsi originali dei file incorporati.</span><span class="sxs-lookup"><span data-stu-id="01657-172">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

> [!NOTE]
> <span data-ttu-id="01657-173">`ManifestEmbeddedFileProvider` è disponibile in ASP.NET Core 2.1 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="01657-173">The `ManifestEmbeddedFileProvider` is available in ASP.NET Core 2.1 or later.</span></span> <span data-ttu-id="01657-174">Per accedere ai file incorporati negli assembly di ASP.NET Core 2.0 o versioni precedenti, vedere la [versione per ASP.NET Core 1.x di questo argomento](/aspnet/core/fundamentals/file-providers?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="01657-174">To access files embedded in assemblies in ASP.NET Core 2.0 or earlier, see the [ASP.NET Core 1.x version of this topic](/aspnet/core/fundamentals/file-providers?view=aspnetcore-1.1).</span></span>

<span data-ttu-id="01657-175">Per generare un manifesto dei file incorporati, impostare la proprietà `<GenerateEmbeddedFilesManifest>` su `true`.</span><span class="sxs-lookup"><span data-stu-id="01657-175">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="01657-176">Specificare i file da incorporare con [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span><span class="sxs-lookup"><span data-stu-id="01657-176">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

<span data-ttu-id="01657-177">Usare [modelli GLOB](#glob-patterns) per specificare uno o più file da incorporare nell'assembly.</span><span class="sxs-lookup"><span data-stu-id="01657-177">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="01657-178">L'app di esempio crea un `ManifestEmbeddedFileProvider` e passa l'assembly attualmente in esecuzione al rispettivo costruttore.</span><span class="sxs-lookup"><span data-stu-id="01657-178">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="01657-179">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="01657-179">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="01657-180">Overload aggiuntivi consentono di:</span><span class="sxs-lookup"><span data-stu-id="01657-180">Additional overloads allow you to:</span></span>

* <span data-ttu-id="01657-181">Specificare un percorso di file relativo.</span><span class="sxs-lookup"><span data-stu-id="01657-181">Specify a relative file path.</span></span>
* <span data-ttu-id="01657-182">Definire la data dell'ultima modifica come ambito dei file.</span><span class="sxs-lookup"><span data-stu-id="01657-182">Scope files to a last modified date.</span></span>
* <span data-ttu-id="01657-183">Assegnare un nome alla risorsa incorporata contenente il manifesto dei file incorporati.</span><span class="sxs-lookup"><span data-stu-id="01657-183">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="01657-184">Overload</span><span class="sxs-lookup"><span data-stu-id="01657-184">Overload</span></span> | <span data-ttu-id="01657-185">Descrizione</span><span class="sxs-lookup"><span data-stu-id="01657-185">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="01657-186">ManifestEmbeddedFileProvider(Assembly, String)</span><span class="sxs-lookup"><span data-stu-id="01657-186">ManifestEmbeddedFileProvider(Assembly, String)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_) | <span data-ttu-id="01657-187">Accetta un parametro di percorso relativo `root` facoltativo.</span><span class="sxs-lookup"><span data-stu-id="01657-187">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="01657-188">Specificare `root` per definire come ambito delle chiamate a [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) le risorse incluse nel percorso specificato.</span><span class="sxs-lookup"><span data-stu-id="01657-188">Specify the `root` to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided path.</span></span> |
| [<span data-ttu-id="01657-189">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="01657-189">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_DateTimeOffset_) | <span data-ttu-id="01657-190">Accetta un parametro di percorso relativo `root` facoltativo e un parametro di data `lastModified` ([DateTimeOffset](/dotnet/api/system.datetimeoffset)).</span><span class="sxs-lookup"><span data-stu-id="01657-190">Accepts an optional `root` relative path parameter and a `lastModified` date ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) parameter.</span></span> <span data-ttu-id="01657-191">La data `lastModified` definisce la data dell'ultima modifica come ambito per le istanze [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) restituite da [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span><span class="sxs-lookup"><span data-stu-id="01657-191">The `lastModified` date scopes the last modification date for the [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) instances returned by the [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> |
| [<span data-ttu-id="01657-192">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="01657-192">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_String_System_DateTimeOffset_) | <span data-ttu-id="01657-193">Accetta un parametro di percorso relativo `root` facoltativo, un parametro di data `lastModified` e il parametro `manifestName`.</span><span class="sxs-lookup"><span data-stu-id="01657-193">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="01657-194">`manifestName` rappresenta il nome della risorsa incorporata contenente il manifesto.</span><span class="sxs-lookup"><span data-stu-id="01657-194">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

### <a name="embeddedfileprovider"></a><span data-ttu-id="01657-195">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="01657-195">EmbeddedFileProvider</span></span>

<span data-ttu-id="01657-196">[EmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider) viene usato per accedere ai file incorporati negli assembly.</span><span class="sxs-lookup"><span data-stu-id="01657-196">The [EmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="01657-197">Specificare i file da incorporare con la proprietà [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="01657-197">Specify the files to embed with the [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) property in the project file:</span></span>

```xml
<ItemGroup>
  <EmbeddedResource Include="Resource.txt" />
</ItemGroup>
```

<span data-ttu-id="01657-198">Usare [modelli GLOB](#glob-patterns) per specificare uno o più file da incorporare nell'assembly.</span><span class="sxs-lookup"><span data-stu-id="01657-198">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="01657-199">L'app di esempio crea un `EmbeddedFileProvider` e passa l'assembly attualmente in esecuzione al rispettivo costruttore.</span><span class="sxs-lookup"><span data-stu-id="01657-199">The sample app creates an `EmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="01657-200">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="01657-200">*Startup.cs*:</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="01657-201">Le risorse incorporate non espongono le directory.</span><span class="sxs-lookup"><span data-stu-id="01657-201">Embedded resources don't expose directories.</span></span> <span data-ttu-id="01657-202">Il percorso della risorsa (tramite il relativo spazio dei nomi) viene invece incorporato nel nome file usando i separatori `.`.</span><span class="sxs-lookup"><span data-stu-id="01657-202">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span> <span data-ttu-id="01657-203">Nell'app di esempio, `baseNamespace` è `FileProviderSample.`.</span><span class="sxs-lookup"><span data-stu-id="01657-203">In the sample app, the `baseNamespace` is `FileProviderSample.`.</span></span>

<span data-ttu-id="01657-204">Il costruttore [EmbeddedFileProvider(Assembly, String)](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_EmbeddedFileProvider__ctor_System_Reflection_Assembly_) accetta un parametro `baseNamespace` facoltativo.</span><span class="sxs-lookup"><span data-stu-id="01657-204">The [EmbeddedFileProvider(Assembly, String)](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_EmbeddedFileProvider__ctor_System_Reflection_Assembly_) constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="01657-205">Specificare lo spazio dei nomi di base per definire come ambito delle chiamate a [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) le risorse incluse nello spazio dei nomi specificato.</span><span class="sxs-lookup"><span data-stu-id="01657-205">Specify the base namespace to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided namespace.</span></span>

::: moniker-end

### <a name="compositefileprovider"></a><span data-ttu-id="01657-206">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="01657-206">CompositeFileProvider</span></span>

<span data-ttu-id="01657-207">[CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) combina le istanze di `IFileProvider` esponendo una singola interfaccia per l'utilizzo dei file da più provider.</span><span class="sxs-lookup"><span data-stu-id="01657-207">The [CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="01657-208">Quando si crea `CompositeFileProvider`, passare una o più istanze `IFileProvider` al relativo costruttore.</span><span class="sxs-lookup"><span data-stu-id="01657-208">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="01657-209">Nell'app di esempio, un `PhysicalFileProvider` e un `ManifestEmbeddedFileProvider` forniscono i file a un `CompositeFileProvider` registrato nel contenitore dei servizi dell'app:</span><span class="sxs-lookup"><span data-stu-id="01657-209">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="01657-210">Nell'app di esempio, un `PhysicalFileProvider` e un `EmbeddedFileProvider` forniscono i file a un `CompositeFileProvider` registrato nel contenitore dei servizi dell'app:</span><span class="sxs-lookup"><span data-stu-id="01657-210">In the sample app, a `PhysicalFileProvider` and an `EmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/1.x/FileProviderSample/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="watch-for-changes"></a><span data-ttu-id="01657-211">Controllo delle modifiche</span><span class="sxs-lookup"><span data-stu-id="01657-211">Watch for changes</span></span>

<span data-ttu-id="01657-212">Il metodo [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) consente di controllare uno o più file o directory per il rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="01657-212">The [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="01657-213">`Watch` accetta una stringa di percorso in cui è possibile usare [criteri GLOB](#glob-patterns) per specificare più file.</span><span class="sxs-lookup"><span data-stu-id="01657-213">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="01657-214">`Watch` restituisce un [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span><span class="sxs-lookup"><span data-stu-id="01657-214">`Watch` returns an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span></span> <span data-ttu-id="01657-215">Il token di modifica espone:</span><span class="sxs-lookup"><span data-stu-id="01657-215">The change token exposes:</span></span>

* <span data-ttu-id="01657-216">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): una proprietà che può essere ispezionata per determinare se è stata modificata.</span><span class="sxs-lookup"><span data-stu-id="01657-216">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="01657-217">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): chiamato quando vengono rilevate modifiche alla stringa di percorso specificata.</span><span class="sxs-lookup"><span data-stu-id="01657-217">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="01657-218">Ogni token di modifica chiama solo il relativo callback associato in risposta a una singola modifica.</span><span class="sxs-lookup"><span data-stu-id="01657-218">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="01657-219">Per abilitare un monitoraggio continuo, usare [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) come illustrato di seguito oppure ricreare istanze di `IChangeToken` in risposta alle modifiche.</span><span class="sxs-lookup"><span data-stu-id="01657-219">To enable constant monitoring, use a [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="01657-220">Nell'app di esempio, l'app console *WatchConsole* viene configurata per visualizzare un messaggio quando viene modificato un file di testo:</span><span class="sxs-lookup"><span data-stu-id="01657-220">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](file-providers/samples/1.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

::: moniker-end

<span data-ttu-id="01657-221">È possibile che alcuni file system, ad esempio i contenitori Docker e le condivisioni di rete, non inviino sempre le notifiche di modifica.</span><span class="sxs-lookup"><span data-stu-id="01657-221">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="01657-222">Impostare la variabile di ambiente `DOTNET_USE_POLLING_FILE_WATCHER` su `1` o `true` per eseguire il polling delle modifiche nel file system ogni quattro secondi (intervallo non configurabile).</span><span class="sxs-lookup"><span data-stu-id="01657-222">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="01657-223">Modelli GLOB</span><span class="sxs-lookup"><span data-stu-id="01657-223">Glob patterns</span></span>

<span data-ttu-id="01657-224">Nei percorsi del file system vengono usati criteri con caratteri jolly chiamati *criteri GLOB (o globbing)*.</span><span class="sxs-lookup"><span data-stu-id="01657-224">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="01657-225">Specificare gruppi di file con questi criteri.</span><span class="sxs-lookup"><span data-stu-id="01657-225">Specify groups of files with these patterns.</span></span> <span data-ttu-id="01657-226">I due caratteri jolly sono `*` e `**`:</span><span class="sxs-lookup"><span data-stu-id="01657-226">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="01657-227">Cerca qualsiasi elemento a livello della cartella corrente, qualsiasi nome di file o qualsiasi estensione di file.</span><span class="sxs-lookup"><span data-stu-id="01657-227">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="01657-228">Le corrispondenze vengono terminate con i caratteri `/` e `.` nel percorso dei file.</span><span class="sxs-lookup"><span data-stu-id="01657-228">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="01657-229">Cerca qualsiasi elemento in più livelli di directory.</span><span class="sxs-lookup"><span data-stu-id="01657-229">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="01657-230">Può essere usato per cercare in modo ricorsivo numerosi file all'interno di una gerarchia di directory.</span><span class="sxs-lookup"><span data-stu-id="01657-230">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="01657-231">**Esempi di criteri GLOB**</span><span class="sxs-lookup"><span data-stu-id="01657-231">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="01657-232">Cerca un file specifico in una directory specifica.</span><span class="sxs-lookup"><span data-stu-id="01657-232">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="01657-233">Cerca tutti i file con estensione *txt* in una directory specifica.</span><span class="sxs-lookup"><span data-stu-id="01657-233">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="01657-234">Cerca tutti i file `appsettings.json` nelle directory esattamente un livello sotto la cartella *directory*.</span><span class="sxs-lookup"><span data-stu-id="01657-234">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="01657-235">Cerca tutti i file con estensione *txt* che si trovano in qualsiasi posizione nella cartella *directory*.</span><span class="sxs-lookup"><span data-stu-id="01657-235">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>
