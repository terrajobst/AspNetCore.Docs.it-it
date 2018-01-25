---
title: Provider di file in ASP.NET Core
author: ardalis
description: Informazioni su come ASP.NET Core astrae l'accesso al file system tramite l'utilizzo di provider di File.
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/file-providers
ms.openlocfilehash: 10f3276d3e71e8a29b452d4c62865cbb82298513
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="7b775-103">Provider di file in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7b775-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="7b775-104">Da [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="7b775-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="7b775-105">ASP.NET Core astrae l'accesso al file system tramite l'utilizzo di provider di File.</span><span class="sxs-lookup"><span data-stu-id="7b775-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span>

<span data-ttu-id="7b775-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7b775-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-abstractions"></a><span data-ttu-id="7b775-107">Astrazioni di Provider di file</span><span class="sxs-lookup"><span data-stu-id="7b775-107">File Provider abstractions</span></span>

<span data-ttu-id="7b775-108">Provider di file sono un'astrazione sui sistemi di file.</span><span class="sxs-lookup"><span data-stu-id="7b775-108">File Providers are an abstraction over file systems.</span></span> <span data-ttu-id="7b775-109">L'interfaccia principale è `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="7b775-109">The main interface is `IFileProvider`.</span></span> <span data-ttu-id="7b775-110">`IFileProvider`espone i metodi per ottenere informazioni sul file (`IFileInfo`), le informazioni di directory (`IDirectoryContents`) e per impostare le notifiche di modifica (utilizzando un `IChangeToken`).</span><span class="sxs-lookup"><span data-stu-id="7b775-110">`IFileProvider` exposes methods to get file information (`IFileInfo`), directory information (`IDirectoryContents`), and to set up change notifications (using an `IChangeToken`).</span></span>

<span data-ttu-id="7b775-111">`IFileInfo`fornisce metodi e proprietà sui singoli file o directory.</span><span class="sxs-lookup"><span data-stu-id="7b775-111">`IFileInfo` provides methods and properties about individual files or directories.</span></span> <span data-ttu-id="7b775-112">Ha due proprietà booleane, `Exists` e `IsDirectory`, nonché le proprietà che descrivono il file `Name`, `Length` (in byte), e `LastModified` Data.</span><span class="sxs-lookup"><span data-stu-id="7b775-112">It has two boolean properties, `Exists` and `IsDirectory`, as well as properties describing the file's `Name`, `Length` (in bytes), and `LastModified` date.</span></span> <span data-ttu-id="7b775-113">È possibile leggere dal file utilizzando il relativo `CreateReadStream` metodo.</span><span class="sxs-lookup"><span data-stu-id="7b775-113">You can read from the file using its `CreateReadStream` method.</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="7b775-114">Le implementazioni del Provider di file</span><span class="sxs-lookup"><span data-stu-id="7b775-114">File Provider implementations</span></span>

<span data-ttu-id="7b775-115">Tre implementazioni di `IFileProvider` sono disponibili: incorporata, fisico e composito.</span><span class="sxs-lookup"><span data-stu-id="7b775-115">Three implementations of `IFileProvider` are available: Physical, Embedded, and Composite.</span></span> <span data-ttu-id="7b775-116">Il provider fisico viene utilizzato per accedere ai file del sistema in uso.</span><span class="sxs-lookup"><span data-stu-id="7b775-116">The physical provider is used to access the actual system's files.</span></span> <span data-ttu-id="7b775-117">Il provider incorporato viene utilizzato per accedere ai file incorporati negli assembly.</span><span class="sxs-lookup"><span data-stu-id="7b775-117">The embedded provider is used to access files embedded in assemblies.</span></span> <span data-ttu-id="7b775-118">Il provider composito viene utilizzato per fornire accesso combinato al file e directory da uno o più provider.</span><span class="sxs-lookup"><span data-stu-id="7b775-118">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span>

### <a name="physicalfileprovider"></a><span data-ttu-id="7b775-119">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="7b775-119">PhysicalFileProvider</span></span>

<span data-ttu-id="7b775-120">Il `PhysicalFileProvider` fornisce l'accesso al file system fisico.</span><span class="sxs-lookup"><span data-stu-id="7b775-120">The `PhysicalFileProvider` provides access to the physical file system.</span></span> <span data-ttu-id="7b775-121">Esegue il wrapping di `System.IO.File` tipo (per il provider fisico), dell'ambito di tutti i percorsi di una directory e i relativi elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="7b775-121">It wraps the `System.IO.File` type (for the physical provider), scoping all paths to a directory and its children.</span></span> <span data-ttu-id="7b775-122">Questo ambito limita l'accesso a una directory e i relativi elementi figlio, impedendo l'accesso al file system di fuori di questo limite.</span><span class="sxs-lookup"><span data-stu-id="7b775-122">This scoping limits access to a certain directory and its children, preventing access to the file system outside of this boundary.</span></span> <span data-ttu-id="7b775-123">Quando si crea questo provider, è necessario fornirlo con un percorso di directory che serve come percorso di base per tutte le richieste effettuate a questo provider (e che limita l'accesso all'esterno di questo percorso).</span><span class="sxs-lookup"><span data-stu-id="7b775-123">When instantiating this provider, you must provide it with a directory path, which serves as the base path for all requests made to this provider (and which restricts access outside of this path).</span></span> <span data-ttu-id="7b775-124">In un'applicazione ASP.NET di base, è possibile creare un'istanza di un `PhysicalFileProvider` provider direttamente oppure è possibile richiedere un `IFileProvider` in un Controller o del servizio tramite [inserimento di dipendenze](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="7b775-124">In an ASP.NET Core app, you can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a Controller or service's constructor through [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="7b775-125">Il secondo approccio produrrà in genere una soluzione più flessibile e testabile.</span><span class="sxs-lookup"><span data-stu-id="7b775-125">The latter approach will typically yield a more flexible and testable solution.</span></span>

<span data-ttu-id="7b775-126">Nell'esempio seguente viene illustrato come creare un `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="7b775-126">The sample below shows how to create a `PhysicalFileProvider`.</span></span>


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

<span data-ttu-id="7b775-127">È possibile scorrere il contenuto della directory o informazioni fornendo un sottopercorso del file specifico.</span><span class="sxs-lookup"><span data-stu-id="7b775-127">You can iterate through its directory contents or get a specific file's information by providing a subpath.</span></span>

<span data-ttu-id="7b775-128">Per richiedere un provider da un controller, specificarlo nel costruttore del controller e la assegna a un campo locale.</span><span class="sxs-lookup"><span data-stu-id="7b775-128">To request a provider from a controller, specify it in the controller's constructor and assign it to a local field.</span></span> <span data-ttu-id="7b775-129">Utilizzare l'istanza locale di metodi di azione:</span><span class="sxs-lookup"><span data-stu-id="7b775-129">Use the local instance from your action methods:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

<span data-ttu-id="7b775-130">Quindi il provider di creare l'app `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="7b775-130">Then, create the provider in the app's `Startup` class:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

<span data-ttu-id="7b775-131">Nel *cshtml* fare scorrere il `IDirectoryContents` fornito:</span><span class="sxs-lookup"><span data-stu-id="7b775-131">In the *Index.cshtml* view, iterate through the `IDirectoryContents` provided:</span></span>

[!code-html[Main](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

<span data-ttu-id="7b775-132">il risultato:</span><span class="sxs-lookup"><span data-stu-id="7b775-132">The result:</span></span>

![File provider esempio applicazione elenco fisico file e cartelle](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a><span data-ttu-id="7b775-134">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="7b775-134">EmbeddedFileProvider</span></span>

<span data-ttu-id="7b775-135">Il `EmbeddedFileProvider` viene utilizzato per accedere ai file incorporati negli assembly.</span><span class="sxs-lookup"><span data-stu-id="7b775-135">The `EmbeddedFileProvider` is used to access files embedded in assemblies.</span></span> <span data-ttu-id="7b775-136">In .NET Core, incorporare file in un assembly con il `<EmbeddedResource>` elemento il *csproj* file:</span><span class="sxs-lookup"><span data-stu-id="7b775-136">In .NET Core, you embed files in an assembly with the `<EmbeddedResource>` element in the *.csproj* file:</span></span>

[!code-json[Main](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

<span data-ttu-id="7b775-137">È possibile utilizzare [modelli il glob](#globbing-patterns) quando si specificano file da incorporare nell'assembly.</span><span class="sxs-lookup"><span data-stu-id="7b775-137">You can use [globbing patterns](#globbing-patterns) when specifying files to embed in the assembly.</span></span> <span data-ttu-id="7b775-138">Questi modelli possono essere utilizzati per la corrispondenza di uno o più file.</span><span class="sxs-lookup"><span data-stu-id="7b775-138">These patterns can be used to match one or more files.</span></span>

> [!NOTE]
> <span data-ttu-id="7b775-139">È improbabile che si desidera mai incorpora ogni file. js del progetto nell'assembly; l'esempio precedente è solo a scopo dimostrativo.</span><span class="sxs-lookup"><span data-stu-id="7b775-139">It's unlikely you would ever want to actually embed every .js file in your project in its assembly; the above sample is for demo purposes only.</span></span>

<span data-ttu-id="7b775-140">Quando si crea un `EmbeddedFileProvider`, passare l'assembly verrà visualizzato al costruttore.</span><span class="sxs-lookup"><span data-stu-id="7b775-140">When creating an `EmbeddedFileProvider`, pass the assembly it will read to its constructor.</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="7b775-141">Il frammento di codice sopra riportato di seguito viene illustrato come creare un `EmbeddedFileProvider` con accesso all'assembly attualmente in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="7b775-141">The snippet above demonstrates how to create an `EmbeddedFileProvider` with access to the currently executing assembly.</span></span>

<span data-ttu-id="7b775-142">Aggiornamento dell'app di esempio per utilizzare un `EmbeddedFileProvider` otterrà l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="7b775-142">Updating the sample app to use an `EmbeddedFileProvider` results in the following output:</span></span>

![Applicazione di esempio di provider file elenco di file incorporati](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> <span data-ttu-id="7b775-144">Le risorse incorporate non espongono le directory.</span><span class="sxs-lookup"><span data-stu-id="7b775-144">Embedded resources don't expose directories.</span></span> <span data-ttu-id="7b775-145">Invece, il percorso della risorsa (tramite lo spazio dei nomi) è incorporato nel nome di file mediante `.` separatori.</span><span class="sxs-lookup"><span data-stu-id="7b775-145">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span>

> [!TIP]
> <span data-ttu-id="7b775-146">Il `EmbeddedFileProvider` costruttore accetta un parametro facoltativo `baseNamespace` parametro.</span><span class="sxs-lookup"><span data-stu-id="7b775-146">The `EmbeddedFileProvider` constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="7b775-147">La definizione di questo ambito chiamate a `GetDirectoryContents` a tali risorse nello spazio dei nomi specificato.</span><span class="sxs-lookup"><span data-stu-id="7b775-147">Specifying this will scope calls to `GetDirectoryContents` to those resources under the provided namespace.</span></span>

### <a name="compositefileprovider"></a><span data-ttu-id="7b775-148">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="7b775-148">CompositeFileProvider</span></span>

<span data-ttu-id="7b775-149">Il `CompositeFileProvider` combina `IFileProvider` istanze, l'esposizione di una singola interfaccia per l'utilizzo di file da più provider.</span><span class="sxs-lookup"><span data-stu-id="7b775-149">The `CompositeFileProvider` combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="7b775-150">Quando si crea il `CompositeFileProvider`, si passa a uno o più `IFileProvider` istanze al costruttore:</span><span class="sxs-lookup"><span data-stu-id="7b775-150">When creating the `CompositeFileProvider`, you pass one or more `IFileProvider` instances to its constructor:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

<span data-ttu-id="7b775-151">Aggiornamento dell'app di esempio per utilizzare un `CompositeFileProvider` che include entrambi i provider, fisici e incorporati configurati in precedenza, viene generato il seguente output:</span><span class="sxs-lookup"><span data-stu-id="7b775-151">Updating the sample app to use a `CompositeFileProvider` that includes both the physical and embedded providers configured previously, results in the following output:</span></span>

![Applicazione di esempio di provider file listato fisici file e cartelle e file incorporati](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a><span data-ttu-id="7b775-153">Analizzare le modifiche apportate</span><span class="sxs-lookup"><span data-stu-id="7b775-153">Watching for changes</span></span>

<span data-ttu-id="7b775-154">Il `IFileProvider` `Watch` metodo fornisce un modo per controllare uno o più file o directory per le modifiche.</span><span class="sxs-lookup"><span data-stu-id="7b775-154">The `IFileProvider` `Watch` method provides a way to watch one or more files or directories for changes.</span></span> <span data-ttu-id="7b775-155">Questo metodo accetta una stringa di percorso, che è possibile utilizzare [modelli il glob](#globbing-patterns) per specificare più file e restituisce un `IChangeToken`.</span><span class="sxs-lookup"><span data-stu-id="7b775-155">This method accepts a path string, which can use [globbing patterns](#globbing-patterns) to specify multiple files, and returns an `IChangeToken`.</span></span> <span data-ttu-id="7b775-156">Questo token espone un `HasChanged` proprietà che può essere controllato, e un `RegisterChangeCallback` metodo che viene chiamato quando vengono rilevate modifiche alla stringa di percorso specificata.</span><span class="sxs-lookup"><span data-stu-id="7b775-156">This token exposes a `HasChanged` property that can be inspected, and a `RegisterChangeCallback` method that's called when changes are detected to the specified path string.</span></span> <span data-ttu-id="7b775-157">Si noti che il token di ogni modifica chiama solo callback associato in risposta a una singola modifica.</span><span class="sxs-lookup"><span data-stu-id="7b775-157">Note that each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="7b775-158">Per abilitare il monitoraggio costante, è possibile utilizzare un `TaskCompletionSource` come illustrato di seguito, oppure ricreare `IChangeToken` istanze in risposta alle modifiche.</span><span class="sxs-lookup"><span data-stu-id="7b775-158">To enable constant monitoring, you can use a `TaskCompletionSource` as shown below, or re-create `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="7b775-159">Nell'esempio di questo articolo, un'applicazione console è configurata per visualizzare un messaggio ogni volta che viene modificato un file di testo:</span><span class="sxs-lookup"><span data-stu-id="7b775-159">In this article's sample, a console application is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[Main](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="7b775-160">Il risultato dopo il salvataggio del file più volte:</span><span class="sxs-lookup"><span data-stu-id="7b775-160">The result, after saving the file several times:</span></span>

![Finestra di comando dopo l'esecuzione dotnet eseguire Mostra il monitoraggio del file quotes.txt per le modifiche dell'applicazione e che il file è stato modificato cinque volte.](file-providers/_static/watch-console.png)

> [!NOTE]
> <span data-ttu-id="7b775-162">Alcuni file System, ad esempio i contenitori di Docker e condivisioni di rete, non può inviare in modo affidabile le notifiche di modifica.</span><span class="sxs-lookup"><span data-stu-id="7b775-162">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="7b775-163">Impostare il `DOTNET_USE_POLLINGFILEWATCHER` variabile di ambiente `1` o `true` per eseguire il polling del file system per modifiche ogni 4 secondi.</span><span class="sxs-lookup"><span data-stu-id="7b775-163">Set the `DOTNET_USE_POLLINGFILEWATCHER` environment variable to `1` or `true` to poll the file system for changes every 4 seconds.</span></span>

## <a name="globbing-patterns"></a><span data-ttu-id="7b775-164">Modelli il glob</span><span class="sxs-lookup"><span data-stu-id="7b775-164">Globbing patterns</span></span>

<span data-ttu-id="7b775-165">Percorsi del file system utilizzano i modelli jolly chiamati *modelli il glob*.</span><span class="sxs-lookup"><span data-stu-id="7b775-165">File system paths use wildcard patterns called *globbing patterns*.</span></span> <span data-ttu-id="7b775-166">Questi modelli semplici possono essere utilizzati per specificare i gruppi di file.</span><span class="sxs-lookup"><span data-stu-id="7b775-166">These simple patterns can be used to specify groups of files.</span></span> <span data-ttu-id="7b775-167">I due caratteri jolly sono `*` e `**`.</span><span class="sxs-lookup"><span data-stu-id="7b775-167">The two wildcard characters are `*` and `**`.</span></span>

**`*`**

   <span data-ttu-id="7b775-168">Corrisponde a qualsiasi livello di cartella corrente, o qualsiasi nome di file o qualsiasi estensione di file.</span><span class="sxs-lookup"><span data-stu-id="7b775-168">Matches anything at the current folder level, or any filename, or any file extension.</span></span> <span data-ttu-id="7b775-169">Corrispondenze vengono terminate dal `/` e `.` caratteri nel percorso del file.</span><span class="sxs-lookup"><span data-stu-id="7b775-169">Matches are terminated by `/` and `.` characters in the file path.</span></span>

<strong><code>**</code></strong>

   <span data-ttu-id="7b775-170">Corrisponde a qualsiasi elemento in più livelli di directory.</span><span class="sxs-lookup"><span data-stu-id="7b775-170">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="7b775-171">Può essere utilizzato in modo ricorsivo corrispondono molti file all'interno di una gerarchia di directory.</span><span class="sxs-lookup"><span data-stu-id="7b775-171">Can be used to recursively match many files within a directory hierarchy.</span></span>

### <a name="globbing-pattern-examples"></a><span data-ttu-id="7b775-172">Esempi di criteri il glob</span><span class="sxs-lookup"><span data-stu-id="7b775-172">Globbing pattern examples</span></span>

**`directory/file.txt`**

   <span data-ttu-id="7b775-173">Consente di ricercare un file specifico in una directory specifica.</span><span class="sxs-lookup"><span data-stu-id="7b775-173">Matches a specific file in a specific directory.</span></span>

**<code>directory/*.txt</code>**

   <span data-ttu-id="7b775-174">Corrisponde a tutti i file con `.txt` estensione in una directory specifica.</span><span class="sxs-lookup"><span data-stu-id="7b775-174">Matches all files with `.txt` extension in a specific directory.</span></span>

**`directory/*/bower.json`**

   <span data-ttu-id="7b775-175">Corrisponde a tutti `bower.json` file nella directory esattamente un livello sotto il `directory` directory.</span><span class="sxs-lookup"><span data-stu-id="7b775-175">Matches all `bower.json` files in directories exactly one level below the `directory` directory.</span></span>

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   <span data-ttu-id="7b775-176">Corrisponde a tutti i file con `.txt` trovata in un punto qualsiasi in un'estensione di `directory` directory.</span><span class="sxs-lookup"><span data-stu-id="7b775-176">Matches all files with `.txt` extension found anywhere under the `directory` directory.</span></span>

## <a name="file-provider-usage-in-aspnet-core"></a><span data-ttu-id="7b775-177">File di utilizzo del Provider in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7b775-177">File Provider usage in ASP.NET Core</span></span>

<span data-ttu-id="7b775-178">Parti diverse di ASP.NET Core utilizzano provider di file.</span><span class="sxs-lookup"><span data-stu-id="7b775-178">Several parts of ASP.NET Core utilize file providers.</span></span> <span data-ttu-id="7b775-179">`IHostingEnvironment`espone l'applicazione radice del contenuto e nella radice web come `IFileProvider` tipi.</span><span class="sxs-lookup"><span data-stu-id="7b775-179">`IHostingEnvironment` exposes the app's content root and web root as `IFileProvider` types.</span></span> <span data-ttu-id="7b775-180">Il middleware di file statici Usa provider di file per individuare i file statici.</span><span class="sxs-lookup"><span data-stu-id="7b775-180">The static files middleware uses file providers to locate static files.</span></span> <span data-ttu-id="7b775-181">Razor consente un uso massiccio di `IFileProvider` viste di individuazione.</span><span class="sxs-lookup"><span data-stu-id="7b775-181">Razor makes heavy use of `IFileProvider` in locating views.</span></span> <span data-ttu-id="7b775-182">Dotnet pubblicare i provider di file Usa funzionalità e i pattern il glob per specificare i file che devono essere pubblicati.</span><span class="sxs-lookup"><span data-stu-id="7b775-182">Dotnet's publish functionality uses file providers and globbing patterns to specify which files should be published.</span></span>

## <a name="recommendations-for-use-in-apps"></a><span data-ttu-id="7b775-183">Consigli per l'uso nelle App</span><span class="sxs-lookup"><span data-stu-id="7b775-183">Recommendations for use in apps</span></span>

<span data-ttu-id="7b775-184">Se l'applicazione ASP.NET di base richiede l'accesso al file system, è possibile richiedere un'istanza di `IFileProvider` tramite l'inserimento di dipendenze, quindi utilizzare i metodi per eseguire l'accesso, come illustrato in questo esempio.</span><span class="sxs-lookup"><span data-stu-id="7b775-184">If your ASP.NET Core app requires file system access, you can request an instance of `IFileProvider` through dependency injection, and then use its methods to perform the access, as shown in this sample.</span></span> <span data-ttu-id="7b775-185">Ciò consente di configurare il provider di una volta, all'avvio, l'app e riduce il numero di tipi di implementazione che crea un'istanza dell'app.</span><span class="sxs-lookup"><span data-stu-id="7b775-185">This allows you to configure the provider once, when the app starts up, and reduces the number of implementation types your app instantiates.</span></span>
