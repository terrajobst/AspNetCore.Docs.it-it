---
title: Provider di file in ASP.NET Core
author: ardalis
description: Informazioni su come ASP.NET Core astrae l'accesso al file system tramite l'utilizzo di provider di File.
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 1e35d362-0005-4f84-a187-274ca203a787
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/file-providers
ms.openlocfilehash: fd847db992b20ab096b54378418d2b9bccff67be
ms.sourcegitcommit: 2b263e87217658caa42eedc4f9d2d21ef0ab5d59
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/12/2018
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="6db12-104">Provider di file in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6db12-104">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="6db12-105">Da [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="6db12-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="6db12-106">ASP.NET Core astrae l'accesso al file system tramite l'utilizzo di provider di File.</span><span class="sxs-lookup"><span data-stu-id="6db12-106">ASP.NET Core abstracts file system access through the use of File Providers.</span></span>

<span data-ttu-id="6db12-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6db12-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-abstractions"></a><span data-ttu-id="6db12-108">Astrazioni di Provider di file</span><span class="sxs-lookup"><span data-stu-id="6db12-108">File Provider abstractions</span></span>

<span data-ttu-id="6db12-109">Provider di file sono un'astrazione sui sistemi di file.</span><span class="sxs-lookup"><span data-stu-id="6db12-109">File Providers are an abstraction over file systems.</span></span> <span data-ttu-id="6db12-110">L'interfaccia principale è `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="6db12-110">The main interface is `IFileProvider`.</span></span> <span data-ttu-id="6db12-111">`IFileProvider`espone i metodi per ottenere informazioni sul file (`IFileInfo`), le informazioni di directory (`IDirectoryContents`) e per impostare le notifiche di modifica (utilizzando un `IChangeToken`).</span><span class="sxs-lookup"><span data-stu-id="6db12-111">`IFileProvider` exposes methods to get file information (`IFileInfo`), directory information (`IDirectoryContents`), and to set up change notifications (using an `IChangeToken`).</span></span>

<span data-ttu-id="6db12-112">`IFileInfo`fornisce metodi e proprietà sui singoli file o directory.</span><span class="sxs-lookup"><span data-stu-id="6db12-112">`IFileInfo` provides methods and properties about individual files or directories.</span></span> <span data-ttu-id="6db12-113">Ha due proprietà booleane, `Exists` e `IsDirectory`, nonché le proprietà che descrivono il file `Name`, `Length` (in byte), e `LastModified` Data.</span><span class="sxs-lookup"><span data-stu-id="6db12-113">It has two boolean properties, `Exists` and `IsDirectory`, as well as properties describing the file's `Name`, `Length` (in bytes), and `LastModified` date.</span></span> <span data-ttu-id="6db12-114">È possibile leggere dal file utilizzando il relativo `CreateReadStream` metodo.</span><span class="sxs-lookup"><span data-stu-id="6db12-114">You can read from the file using its `CreateReadStream` method.</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="6db12-115">Le implementazioni del Provider di file</span><span class="sxs-lookup"><span data-stu-id="6db12-115">File Provider implementations</span></span>

<span data-ttu-id="6db12-116">Tre implementazioni di `IFileProvider` sono disponibili: incorporata, fisico e composito.</span><span class="sxs-lookup"><span data-stu-id="6db12-116">Three implementations of `IFileProvider` are available: Physical, Embedded, and Composite.</span></span> <span data-ttu-id="6db12-117">Il provider fisico viene utilizzato per accedere ai file del sistema in uso.</span><span class="sxs-lookup"><span data-stu-id="6db12-117">The physical provider is used to access the actual system's files.</span></span> <span data-ttu-id="6db12-118">Il provider incorporato viene utilizzato per accedere ai file incorporati negli assembly.</span><span class="sxs-lookup"><span data-stu-id="6db12-118">The embedded provider is used to access files embedded in assemblies.</span></span> <span data-ttu-id="6db12-119">Il provider composito viene utilizzato per fornire accesso combinato al file e directory da uno o più provider.</span><span class="sxs-lookup"><span data-stu-id="6db12-119">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span>

### <a name="physicalfileprovider"></a><span data-ttu-id="6db12-120">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="6db12-120">PhysicalFileProvider</span></span>

<span data-ttu-id="6db12-121">Il `PhysicalFileProvider` fornisce l'accesso al file system fisico.</span><span class="sxs-lookup"><span data-stu-id="6db12-121">The `PhysicalFileProvider` provides access to the physical file system.</span></span> <span data-ttu-id="6db12-122">Esegue il wrapping di `System.IO.File` tipo (per il provider fisico), dell'ambito di tutti i percorsi di una directory e i relativi elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="6db12-122">It wraps the `System.IO.File` type (for the physical provider), scoping all paths to a directory and its children.</span></span> <span data-ttu-id="6db12-123">Questo ambito limita l'accesso a una directory e i relativi elementi figlio, impedendo l'accesso al file system di fuori di questo limite.</span><span class="sxs-lookup"><span data-stu-id="6db12-123">This scoping limits access to a certain directory and its children, preventing access to the file system outside of this boundary.</span></span> <span data-ttu-id="6db12-124">Quando si crea questo provider, è necessario fornirlo con un percorso di directory che serve come percorso di base per tutte le richieste effettuate a questo provider (e che limita l'accesso all'esterno di questo percorso).</span><span class="sxs-lookup"><span data-stu-id="6db12-124">When instantiating this provider, you must provide it with a directory path, which serves as the base path for all requests made to this provider (and which restricts access outside of this path).</span></span> <span data-ttu-id="6db12-125">In un'applicazione ASP.NET di base, è possibile creare un'istanza di un `PhysicalFileProvider` provider direttamente oppure è possibile richiedere un `IFileProvider` in un Controller o del servizio tramite [inserimento di dipendenze](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="6db12-125">In an ASP.NET Core app, you can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a Controller or service's constructor through [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="6db12-126">Il secondo approccio produrrà in genere una soluzione più flessibile e testabile.</span><span class="sxs-lookup"><span data-stu-id="6db12-126">The latter approach will typically yield a more flexible and testable solution.</span></span>

<span data-ttu-id="6db12-127">Nell'esempio seguente viene illustrato come creare un `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="6db12-127">The sample below shows how to create a `PhysicalFileProvider`.</span></span>


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

<span data-ttu-id="6db12-128">È possibile scorrere il contenuto della directory o informazioni fornendo un sottopercorso del file specifico.</span><span class="sxs-lookup"><span data-stu-id="6db12-128">You can iterate through its directory contents or get a specific file's information by providing a subpath.</span></span>

<span data-ttu-id="6db12-129">Per richiedere un provider da un controller, specificarlo nel costruttore del controller e la assegna a un campo locale.</span><span class="sxs-lookup"><span data-stu-id="6db12-129">To request a provider from a controller, specify it in the controller's constructor and assign it to a local field.</span></span> <span data-ttu-id="6db12-130">Utilizzare l'istanza locale di metodi di azione:</span><span class="sxs-lookup"><span data-stu-id="6db12-130">Use the local instance from your action methods:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

<span data-ttu-id="6db12-131">Quindi il provider di creare l'app `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="6db12-131">Then, create the provider in the app's `Startup` class:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

<span data-ttu-id="6db12-132">Nel *cshtml* fare scorrere il `IDirectoryContents` fornito:</span><span class="sxs-lookup"><span data-stu-id="6db12-132">In the *Index.cshtml* view, iterate through the `IDirectoryContents` provided:</span></span>

[!code-html[Main](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

<span data-ttu-id="6db12-133">il risultato:</span><span class="sxs-lookup"><span data-stu-id="6db12-133">The result:</span></span>

![File provider esempio applicazione elenco fisico file e cartelle](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a><span data-ttu-id="6db12-135">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="6db12-135">EmbeddedFileProvider</span></span>

<span data-ttu-id="6db12-136">Il `EmbeddedFileProvider` viene utilizzato per accedere ai file incorporati negli assembly.</span><span class="sxs-lookup"><span data-stu-id="6db12-136">The `EmbeddedFileProvider` is used to access files embedded in assemblies.</span></span> <span data-ttu-id="6db12-137">In .NET Core, incorporare file in un assembly con il `<EmbeddedResource>` elemento il *csproj* file:</span><span class="sxs-lookup"><span data-stu-id="6db12-137">In .NET Core, you embed files in an assembly with the `<EmbeddedResource>` element in the *.csproj* file:</span></span>

[!code-json[Main](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

<span data-ttu-id="6db12-138">È possibile utilizzare [modelli il glob](#globbing-patterns) quando si specificano file da incorporare nell'assembly.</span><span class="sxs-lookup"><span data-stu-id="6db12-138">You can use [globbing patterns](#globbing-patterns) when specifying files to embed in the assembly.</span></span> <span data-ttu-id="6db12-139">Questi modelli possono essere utilizzati per la corrispondenza di uno o più file.</span><span class="sxs-lookup"><span data-stu-id="6db12-139">These patterns can be used to match one or more files.</span></span>

> [!NOTE]
> <span data-ttu-id="6db12-140">È improbabile che si desidera mai incorpora ogni file. js del progetto nell'assembly; l'esempio precedente è solo a scopo dimostrativo.</span><span class="sxs-lookup"><span data-stu-id="6db12-140">It's unlikely you would ever want to actually embed every .js file in your project in its assembly; the above sample is for demo purposes only.</span></span>

<span data-ttu-id="6db12-141">Quando si crea un `EmbeddedFileProvider`, passare l'assembly verrà visualizzato al costruttore.</span><span class="sxs-lookup"><span data-stu-id="6db12-141">When creating an `EmbeddedFileProvider`, pass the assembly it will read to its constructor.</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="6db12-142">Il frammento di codice sopra riportato di seguito viene illustrato come creare un `EmbeddedFileProvider` con accesso all'assembly attualmente in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="6db12-142">The snippet above demonstrates how to create an `EmbeddedFileProvider` with access to the currently executing assembly.</span></span>

<span data-ttu-id="6db12-143">Aggiornamento dell'app di esempio per utilizzare un `EmbeddedFileProvider` otterrà l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="6db12-143">Updating the sample app to use an `EmbeddedFileProvider` results in the following output:</span></span>

![Applicazione di esempio di provider file elenco di file incorporati](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> <span data-ttu-id="6db12-145">Directory non espongono le risorse incorporate.</span><span class="sxs-lookup"><span data-stu-id="6db12-145">Embedded resources do not expose directories.</span></span> <span data-ttu-id="6db12-146">Invece, il percorso della risorsa (tramite lo spazio dei nomi) è incorporato nel nome di file mediante `.` separatori.</span><span class="sxs-lookup"><span data-stu-id="6db12-146">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span>

> [!TIP]
> <span data-ttu-id="6db12-147">Il `EmbeddedFileProvider` costruttore accetta un parametro facoltativo `baseNamespace` parametro.</span><span class="sxs-lookup"><span data-stu-id="6db12-147">The `EmbeddedFileProvider` constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="6db12-148">La definizione di questo ambito chiamate a `GetDirectoryContents` a tali risorse nello spazio dei nomi specificato.</span><span class="sxs-lookup"><span data-stu-id="6db12-148">Specifying this will scope calls to `GetDirectoryContents` to those resources under the provided namespace.</span></span>

### <a name="compositefileprovider"></a><span data-ttu-id="6db12-149">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="6db12-149">CompositeFileProvider</span></span>

<span data-ttu-id="6db12-150">Il `CompositeFileProvider` combina `IFileProvider` istanze, l'esposizione di una singola interfaccia per l'utilizzo di file da più provider.</span><span class="sxs-lookup"><span data-stu-id="6db12-150">The `CompositeFileProvider` combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="6db12-151">Quando si crea il `CompositeFileProvider`, si passa a uno o più `IFileProvider` istanze al costruttore:</span><span class="sxs-lookup"><span data-stu-id="6db12-151">When creating the `CompositeFileProvider`, you pass one or more `IFileProvider` instances to its constructor:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

<span data-ttu-id="6db12-152">Aggiornamento dell'app di esempio per utilizzare un `CompositeFileProvider` che include entrambi i provider, fisici e incorporati configurati in precedenza, viene generato il seguente output:</span><span class="sxs-lookup"><span data-stu-id="6db12-152">Updating the sample app to use a `CompositeFileProvider` that includes both the physical and embedded providers configured previously, results in the following output:</span></span>

![Applicazione di esempio di provider file listato fisici file e cartelle e file incorporati](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a><span data-ttu-id="6db12-154">Analizzare le modifiche apportate</span><span class="sxs-lookup"><span data-stu-id="6db12-154">Watching for changes</span></span>

<span data-ttu-id="6db12-155">Il `IFileProvider` `Watch` metodo fornisce un modo per controllare uno o più file o directory per le modifiche.</span><span class="sxs-lookup"><span data-stu-id="6db12-155">The `IFileProvider` `Watch` method provides a way to watch one or more files or directories for changes.</span></span> <span data-ttu-id="6db12-156">Questo metodo accetta una stringa di percorso, che è possibile utilizzare [modelli il glob](#globbing-patterns) per specificare più file e restituisce un `IChangeToken`.</span><span class="sxs-lookup"><span data-stu-id="6db12-156">This method accepts a path string, which can use [globbing patterns](#globbing-patterns) to specify multiple files, and returns an `IChangeToken`.</span></span> <span data-ttu-id="6db12-157">Questo token espone un `HasChanged` proprietà che può essere controllato, e un `RegisterChangeCallback` metodo che viene chiamato quando vengono rilevate modifiche alla stringa di percorso specificata.</span><span class="sxs-lookup"><span data-stu-id="6db12-157">This token exposes a `HasChanged` property that can be inspected, and a `RegisterChangeCallback` method that is called when changes are detected to the specified path string.</span></span> <span data-ttu-id="6db12-158">Si noti che il token di ogni modifica chiama solo callback associato in risposta a una singola modifica.</span><span class="sxs-lookup"><span data-stu-id="6db12-158">Note that each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="6db12-159">Per abilitare il monitoraggio costante, è possibile utilizzare un `TaskCompletionSource` come illustrato di seguito, oppure ricreare `IChangeToken` istanze in risposta alle modifiche.</span><span class="sxs-lookup"><span data-stu-id="6db12-159">To enable constant monitoring, you can use a `TaskCompletionSource` as shown below, or re-create `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="6db12-160">Nell'esempio di questo articolo, un'applicazione console è configurata per visualizzare un messaggio ogni volta che viene modificato un file di testo:</span><span class="sxs-lookup"><span data-stu-id="6db12-160">In this article's sample, a console application is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[Main](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="6db12-161">Il risultato dopo il salvataggio del file più volte:</span><span class="sxs-lookup"><span data-stu-id="6db12-161">The result, after saving the file several times:</span></span>

![Finestra di comando dopo l'esecuzione dotnet eseguire Mostra il monitoraggio del file quotes.txt per le modifiche dell'applicazione e che il file è stato modificato cinque volte.](file-providers/_static/watch-console.png)

> [!NOTE]
> <span data-ttu-id="6db12-163">Alcuni file System, ad esempio i contenitori di Docker e condivisioni di rete, non può inviare in modo affidabile le notifiche di modifica.</span><span class="sxs-lookup"><span data-stu-id="6db12-163">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="6db12-164">Impostare il `DOTNET_USE_POLLINGFILEWATCHER` variabile di ambiente `1` o `true` per eseguire il polling del file system per modifiche ogni 4 secondi.</span><span class="sxs-lookup"><span data-stu-id="6db12-164">Set the `DOTNET_USE_POLLINGFILEWATCHER` environment variable to `1` or `true` to poll the file system for changes every 4 seconds.</span></span>

## <a name="globbing-patterns"></a><span data-ttu-id="6db12-165">Modelli il glob</span><span class="sxs-lookup"><span data-stu-id="6db12-165">Globbing patterns</span></span>

<span data-ttu-id="6db12-166">Percorsi del file system utilizzano i modelli jolly chiamati *modelli il glob*.</span><span class="sxs-lookup"><span data-stu-id="6db12-166">File system paths use wildcard patterns called *globbing patterns*.</span></span> <span data-ttu-id="6db12-167">Questi modelli semplici possono essere utilizzati per specificare i gruppi di file.</span><span class="sxs-lookup"><span data-stu-id="6db12-167">These simple patterns can be used to specify groups of files.</span></span> <span data-ttu-id="6db12-168">I due caratteri jolly sono `*` e `**`.</span><span class="sxs-lookup"><span data-stu-id="6db12-168">The two wildcard characters are `*` and `**`.</span></span>

**`*`**

   <span data-ttu-id="6db12-169">Corrisponde a qualsiasi livello di cartella corrente, o qualsiasi nome di file o qualsiasi estensione di file.</span><span class="sxs-lookup"><span data-stu-id="6db12-169">Matches anything at the current folder level, or any filename, or any file extension.</span></span> <span data-ttu-id="6db12-170">Corrispondenze vengono terminate dal `/` e `.` caratteri nel percorso del file.</span><span class="sxs-lookup"><span data-stu-id="6db12-170">Matches are terminated by `/` and `.` characters in the file path.</span></span>

<strong><code>**</code></strong>

   <span data-ttu-id="6db12-171">Corrisponde a qualsiasi elemento in più livelli di directory.</span><span class="sxs-lookup"><span data-stu-id="6db12-171">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="6db12-172">Può essere utilizzato in modo ricorsivo corrispondono molti file all'interno di una gerarchia di directory.</span><span class="sxs-lookup"><span data-stu-id="6db12-172">Can be used to recursively match many files within a directory hierarchy.</span></span>

### <a name="globbing-pattern-examples"></a><span data-ttu-id="6db12-173">Esempi di criteri il glob</span><span class="sxs-lookup"><span data-stu-id="6db12-173">Globbing pattern examples</span></span>

**`directory/file.txt`**

   <span data-ttu-id="6db12-174">Consente di ricercare un file specifico in una directory specifica.</span><span class="sxs-lookup"><span data-stu-id="6db12-174">Matches a specific file in a specific directory.</span></span>

**<code>directory/*.txt</code>**

   <span data-ttu-id="6db12-175">Corrisponde a tutti i file con `.txt` estensione in una directory specifica.</span><span class="sxs-lookup"><span data-stu-id="6db12-175">Matches all files with `.txt` extension in a specific directory.</span></span>

**`directory/*/bower.json`**

   <span data-ttu-id="6db12-176">Corrisponde a tutti `bower.json` file nella directory esattamente un livello sotto il `directory` directory.</span><span class="sxs-lookup"><span data-stu-id="6db12-176">Matches all `bower.json` files in directories exactly one level below the `directory` directory.</span></span>

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   <span data-ttu-id="6db12-177">Corrisponde a tutti i file con `.txt` trovata in un punto qualsiasi in un'estensione di `directory` directory.</span><span class="sxs-lookup"><span data-stu-id="6db12-177">Matches all files with `.txt` extension found anywhere under the `directory` directory.</span></span>

## <a name="file-provider-usage-in-aspnet-core"></a><span data-ttu-id="6db12-178">File di utilizzo del Provider in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6db12-178">File Provider usage in ASP.NET Core</span></span>

<span data-ttu-id="6db12-179">Parti diverse di ASP.NET Core utilizzano provider di file.</span><span class="sxs-lookup"><span data-stu-id="6db12-179">Several parts of ASP.NET Core utilize file providers.</span></span> <span data-ttu-id="6db12-180">`IHostingEnvironment`espone l'applicazione radice del contenuto e nella radice web come `IFileProvider` tipi.</span><span class="sxs-lookup"><span data-stu-id="6db12-180">`IHostingEnvironment` exposes the app's content root and web root as `IFileProvider` types.</span></span> <span data-ttu-id="6db12-181">Il middleware di file statici Usa provider di file per individuare i file statici.</span><span class="sxs-lookup"><span data-stu-id="6db12-181">The static files middleware uses file providers to locate static files.</span></span> <span data-ttu-id="6db12-182">Razor consente un uso massiccio di `IFileProvider` viste di individuazione.</span><span class="sxs-lookup"><span data-stu-id="6db12-182">Razor makes heavy use of `IFileProvider` in locating views.</span></span> <span data-ttu-id="6db12-183">Dotnet pubblicare i provider di file Usa funzionalità e i pattern il glob per specificare i file che devono essere pubblicati.</span><span class="sxs-lookup"><span data-stu-id="6db12-183">Dotnet's publish functionality uses file providers and globbing patterns to specify which files should be published.</span></span>

## <a name="recommendations-for-use-in-apps"></a><span data-ttu-id="6db12-184">Consigli per l'uso nelle App</span><span class="sxs-lookup"><span data-stu-id="6db12-184">Recommendations for use in apps</span></span>

<span data-ttu-id="6db12-185">Se l'applicazione ASP.NET di base richiede l'accesso al file system, è possibile richiedere un'istanza di `IFileProvider` tramite l'inserimento di dipendenze, quindi utilizzare i metodi per eseguire l'accesso, come illustrato in questo esempio.</span><span class="sxs-lookup"><span data-stu-id="6db12-185">If your ASP.NET Core app requires file system access, you can request an instance of `IFileProvider` through dependency injection, and then use its methods to perform the access, as shown in this sample.</span></span> <span data-ttu-id="6db12-186">Ciò consente di configurare il provider di una volta, all'avvio, l'app e riduce il numero di tipi di implementazione che crea un'istanza dell'app.</span><span class="sxs-lookup"><span data-stu-id="6db12-186">This allows you to configure the provider once, when the app starts up, and reduces the number of implementation types your app instantiates.</span></span>
