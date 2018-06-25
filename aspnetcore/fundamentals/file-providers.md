---
title: Provider di file in ASP.NET Core
author: ardalis
description: Informazioni su come ASP.NET Core astrae l'accesso al file system tramite l'utilizzo di provider di file.
ms.author: riande
ms.date: 02/14/2017
uid: fundamentals/file-providers
ms.openlocfilehash: 0d356322ea9f4cc2caead81746bf9ede4a87923f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276240"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="c7758-103">Provider di file in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7758-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="c7758-104">[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="c7758-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="c7758-105">ASP.NET Core astrae l'accesso al file system tramite l'utilizzo di provider di file.</span><span class="sxs-lookup"><span data-stu-id="c7758-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span>

<span data-ttu-id="c7758-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c7758-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-abstractions"></a><span data-ttu-id="c7758-107">Astrazioni dei provider di file</span><span class="sxs-lookup"><span data-stu-id="c7758-107">File Provider abstractions</span></span>

<span data-ttu-id="c7758-108">I provider di file sono astrazioni dei file system.</span><span class="sxs-lookup"><span data-stu-id="c7758-108">File Providers are an abstraction over file systems.</span></span> <span data-ttu-id="c7758-109">L'interfaccia principale è `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="c7758-109">The main interface is `IFileProvider`.</span></span> <span data-ttu-id="c7758-110">`IFileProvider` espone metodi per ottenere informazioni sui file (`IFileInfo`) e informazioni sulle directory (`IDirectoryContents`) e per impostare le notifiche di modifica (usando `IChangeToken`).</span><span class="sxs-lookup"><span data-stu-id="c7758-110">`IFileProvider` exposes methods to get file information (`IFileInfo`), directory information (`IDirectoryContents`), and to set up change notifications (using an `IChangeToken`).</span></span>

<span data-ttu-id="c7758-111">`IFileInfo` offre metodi e proprietà relative a singoli file o directory.</span><span class="sxs-lookup"><span data-stu-id="c7758-111">`IFileInfo` provides methods and properties about individual files or directories.</span></span> <span data-ttu-id="c7758-112">Ha due proprietà booleane, `Exists` e `IsDirectory`, e proprietà che descrivono `Name`, `Length` (in byte) e data `LastModified` del file.</span><span class="sxs-lookup"><span data-stu-id="c7758-112">It has two boolean properties, `Exists` and `IsDirectory`, as well as properties describing the file's `Name`, `Length` (in bytes), and `LastModified` date.</span></span> <span data-ttu-id="c7758-113">È possibile leggere dal file usando il relativo metodo `CreateReadStream`.</span><span class="sxs-lookup"><span data-stu-id="c7758-113">You can read from the file using its `CreateReadStream` method.</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="c7758-114">Implementazioni dei provider di file</span><span class="sxs-lookup"><span data-stu-id="c7758-114">File Provider implementations</span></span>

<span data-ttu-id="c7758-115">Sono disponibili tre implementazioni di `IFileProvider`: fisica, incorporata e composita.</span><span class="sxs-lookup"><span data-stu-id="c7758-115">Three implementations of `IFileProvider` are available: Physical, Embedded, and Composite.</span></span> <span data-ttu-id="c7758-116">Il provider fisico consente di accedere ai file del sistema.</span><span class="sxs-lookup"><span data-stu-id="c7758-116">The physical provider is used to access the actual system's files.</span></span> <span data-ttu-id="c7758-117">Il provider incorporato consente di accedere ai file incorporati negli assembly.</span><span class="sxs-lookup"><span data-stu-id="c7758-117">The embedded provider is used to access files embedded in assemblies.</span></span> <span data-ttu-id="c7758-118">Il provider composito consente di offrire un accesso combinato a file e directory da uno o più provider.</span><span class="sxs-lookup"><span data-stu-id="c7758-118">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span>

### <a name="physicalfileprovider"></a><span data-ttu-id="c7758-119">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="c7758-119">PhysicalFileProvider</span></span>

<span data-ttu-id="c7758-120">`PhysicalFileProvider` consente di accedere al file system fisico.</span><span class="sxs-lookup"><span data-stu-id="c7758-120">The `PhysicalFileProvider` provides access to the physical file system.</span></span> <span data-ttu-id="c7758-121">Esegue il wrapping del tipo `System.IO.File` (per il provider fisico), definendo l'ambito di tutti i percorsi in una directory e nei relativi elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="c7758-121">It wraps the `System.IO.File` type (for the physical provider), scoping all paths to a directory and its children.</span></span> <span data-ttu-id="c7758-122">La definizione dell'ambito limita l'accesso a una determinata directory e ai relativi elementi figlio impedendo l'accesso al file system che non rientra nel limite.</span><span class="sxs-lookup"><span data-stu-id="c7758-122">This scoping limits access to a certain directory and its children, preventing access to the file system outside of this boundary.</span></span> <span data-ttu-id="c7758-123">Quando si crea un'istanza di questo provider, è necessario specificare il percorso di una directory che viene usato come percorso di base per tutte le richieste effettuate dal provider (e che impedisce l'accesso all'esterno del percorso).</span><span class="sxs-lookup"><span data-stu-id="c7758-123">When instantiating this provider, you must provide it with a directory path, which serves as the base path for all requests made to this provider (and which restricts access outside of this path).</span></span> <span data-ttu-id="c7758-124">In un'app ASP.NET Core è possibile creare direttamente un'istanza di un provider `PhysicalFileProvider` oppure richiedere `IFileProvider` in un controller o un costruttore del servizio tramite l'[inserimento di dipendenze](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="c7758-124">In an ASP.NET Core app, you can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a Controller or service's constructor through [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="c7758-125">Il secondo approccio offre in genere una soluzione più flessibile e verificabile.</span><span class="sxs-lookup"><span data-stu-id="c7758-125">The latter approach will typically yield a more flexible and testable solution.</span></span>

<span data-ttu-id="c7758-126">L'esempio seguente illustra come creare un provider `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="c7758-126">The sample below shows how to create a `PhysicalFileProvider`.</span></span>


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

<span data-ttu-id="c7758-127">È possibile scorrere i contenuti della directory oppure ottenere le informazioni su un file specifico specificando un sottopercorso.</span><span class="sxs-lookup"><span data-stu-id="c7758-127">You can iterate through its directory contents or get a specific file's information by providing a subpath.</span></span>

<span data-ttu-id="c7758-128">Per richiedere un provider da un controller, specificarlo nel costruttore del controller e assegnarlo a un campo locale.</span><span class="sxs-lookup"><span data-stu-id="c7758-128">To request a provider from a controller, specify it in the controller's constructor and assign it to a local field.</span></span> <span data-ttu-id="c7758-129">Usare l'istanza locale dai metodi di azione:</span><span class="sxs-lookup"><span data-stu-id="c7758-129">Use the local instance from your action methods:</span></span>

[!code-csharp[](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

<span data-ttu-id="c7758-130">Creare quindi il provider nella classe `Startup` dell'app:</span><span class="sxs-lookup"><span data-stu-id="c7758-130">Then, create the provider in the app's `Startup` class:</span></span>

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

<span data-ttu-id="c7758-131">Nella visualizzazione *Index.cshtml* scorrere `IDirectoryContents`:</span><span class="sxs-lookup"><span data-stu-id="c7758-131">In the *Index.cshtml* view, iterate through the `IDirectoryContents` provided:</span></span>

[!code-html[](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

<span data-ttu-id="c7758-132">Il risultato sarà:</span><span class="sxs-lookup"><span data-stu-id="c7758-132">The result:</span></span>

![Applicazione di esempio di provider di file con elenco dei file fisici e delle cartelle](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a><span data-ttu-id="c7758-134">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="c7758-134">EmbeddedFileProvider</span></span>

<span data-ttu-id="c7758-135">`EmbeddedFileProvider` consente di accedere ai file incorporati negli assembly.</span><span class="sxs-lookup"><span data-stu-id="c7758-135">The `EmbeddedFileProvider` is used to access files embedded in assemblies.</span></span> <span data-ttu-id="c7758-136">In .NET Core i file vengono incorporati in un assembly con l'elemento `<EmbeddedResource>` nel file *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="c7758-136">In .NET Core, you embed files in an assembly with the `<EmbeddedResource>` element in the *.csproj* file:</span></span>

[!code-json[](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

<span data-ttu-id="c7758-137">Durante la specifica dei file da incorporare nell'assembly è possibile usare [criteri GLOB](#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="c7758-137">You can use [globbing patterns](#globbing-patterns) when specifying files to embed in the assembly.</span></span> <span data-ttu-id="c7758-138">Questi criteri possono essere usati per cercare uno o più file.</span><span class="sxs-lookup"><span data-stu-id="c7758-138">These patterns can be used to match one or more files.</span></span>

> [!NOTE]
> <span data-ttu-id="c7758-139">È improbabile che si voglia incorporare ogni file con estensione js nel progetto nel relativo assembly; l'esempio precedente è solo a scopo dimostrativo.</span><span class="sxs-lookup"><span data-stu-id="c7758-139">It's unlikely you would ever want to actually embed every .js file in your project in its assembly; the above sample is for demo purposes only.</span></span>

<span data-ttu-id="c7758-140">Quando si crea un provider `EmbeddedFileProvider`, passare l'assembly che verrà letto al relativo costruttore.</span><span class="sxs-lookup"><span data-stu-id="c7758-140">When creating an `EmbeddedFileProvider`, pass the assembly it will read to its constructor.</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="c7758-141">Il frammento di codice precedente illustra come creare un provider `EmbeddedFileProvider` con accesso all'assembly attualmente in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="c7758-141">The snippet above demonstrates how to create an `EmbeddedFileProvider` with access to the currently executing assembly.</span></span>

<span data-ttu-id="c7758-142">L'aggiornamento dell'app di esempio per l'uso di un provider `EmbeddedFileProvider` produce l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="c7758-142">Updating the sample app to use an `EmbeddedFileProvider` results in the following output:</span></span>

![Applicazione di esempio di provider di file con elenco dei file incorporati](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> <span data-ttu-id="c7758-144">Le risorse incorporate non espongono le directory.</span><span class="sxs-lookup"><span data-stu-id="c7758-144">Embedded resources don't expose directories.</span></span> <span data-ttu-id="c7758-145">Il percorso della risorsa (tramite il relativo spazio dei nomi) viene invece incorporato nel nome file usando i separatori `.`.</span><span class="sxs-lookup"><span data-stu-id="c7758-145">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span>

> [!TIP]
> <span data-ttu-id="c7758-146">Il costruttore `EmbeddedFileProvider` accetta un parametro `baseNamespace` facoltativo.</span><span class="sxs-lookup"><span data-stu-id="c7758-146">The `EmbeddedFileProvider` constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="c7758-147">Questa specifica determinerà la definizione di un ambito delle chiamate a `GetDirectoryContents` corrispondente alle risorse appartenenti allo spazio dei nomi specificato.</span><span class="sxs-lookup"><span data-stu-id="c7758-147">Specifying this will scope calls to `GetDirectoryContents` to those resources under the provided namespace.</span></span>

### <a name="compositefileprovider"></a><span data-ttu-id="c7758-148">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="c7758-148">CompositeFileProvider</span></span>

<span data-ttu-id="c7758-149">`CompositeFileProvider` combina le istanze `IFileProvider` esponendo una sola interfaccia per l'uso dei file di più provider.</span><span class="sxs-lookup"><span data-stu-id="c7758-149">The `CompositeFileProvider` combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="c7758-150">Quando si crea il provider `CompositeFileProvider`, si passano una o più istanze `IFileProvider` al relativo costruttore:</span><span class="sxs-lookup"><span data-stu-id="c7758-150">When creating the `CompositeFileProvider`, you pass one or more `IFileProvider` instances to its constructor:</span></span>

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

<span data-ttu-id="c7758-151">L'aggiornamento dell'app di esempio per l'uso di un provider `CompositeFileProvider` che include i provider fisici e i provider incorporati definiti in precedenza produce l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="c7758-151">Updating the sample app to use a `CompositeFileProvider` that includes both the physical and embedded providers configured previously, results in the following output:</span></span>

![Applicazione di esempio di provider di file con elenco dei file fisici, delle cartelle e dei file incorporati](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a><span data-ttu-id="c7758-153">Controllo per la ricerca di modifiche</span><span class="sxs-lookup"><span data-stu-id="c7758-153">Watching for changes</span></span>

<span data-ttu-id="c7758-154">Il metodo `Watch` di `IFileProvider` consente di controllare uno o più file o directory per la ricerca di modifiche.</span><span class="sxs-lookup"><span data-stu-id="c7758-154">The `IFileProvider` `Watch` method provides a way to watch one or more files or directories for changes.</span></span> <span data-ttu-id="c7758-155">Questo metodo accetta una stringa di percorso in cui è possibile usare [criteri GLOB](#globbing-patterns) per specificare più file e restituisce `IChangeToken`.</span><span class="sxs-lookup"><span data-stu-id="c7758-155">This method accepts a path string, which can use [globbing patterns](#globbing-patterns) to specify multiple files, and returns an `IChangeToken`.</span></span> <span data-ttu-id="c7758-156">Il token espone una proprietà `HasChanged` in cui è possibile eseguire il controllo e un metodo `RegisterChangeCallback` che viene chiamato quando vengono rilevate modifiche nella stringa di percorso specificata.</span><span class="sxs-lookup"><span data-stu-id="c7758-156">This token exposes a `HasChanged` property that can be inspected, and a `RegisterChangeCallback` method that's called when changes are detected to the specified path string.</span></span> <span data-ttu-id="c7758-157">Si noti che ogni token di modifica chiama solo il relativo callback associato in risposta a una singola modifica.</span><span class="sxs-lookup"><span data-stu-id="c7758-157">Note that each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="c7758-158">Per abilitare un monitoraggio continuo, è possibile usare `TaskCompletionSource` come illustrato di seguito oppure ricreare istanze `IChangeToken` in risposta alle modifiche.</span><span class="sxs-lookup"><span data-stu-id="c7758-158">To enable constant monitoring, you can use a `TaskCompletionSource` as shown below, or re-create `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="c7758-159">Nell'esempio di questo articolo viene configurata un'applicazione console in modo che visualizzi un messaggio quando viene modificato un file di testo:</span><span class="sxs-lookup"><span data-stu-id="c7758-159">In this article's sample, a console application is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="c7758-160">Dopo aver salvato il file più volte, il risultato è il seguente:</span><span class="sxs-lookup"><span data-stu-id="c7758-160">The result, after saving the file several times:</span></span>

![Finestra di comando dopo l'esecuzione di dotnet run che mostra il controllo del file quotes.txt da parte dell'applicazione e l'individuazione di cinque modifiche.](file-providers/_static/watch-console.png)

> [!NOTE]
> <span data-ttu-id="c7758-162">È possibile che alcuni file system, ad esempio i contenitori Docker e le condivisioni di rete, non inviino sempre le notifiche di modifica.</span><span class="sxs-lookup"><span data-stu-id="c7758-162">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="c7758-163">Impostare la variabile di ambiente `DOTNET_USE_POLLINGFILEWATCHER` su `1` o `true` per cercare le modifiche nel file system ogni 4 secondi.</span><span class="sxs-lookup"><span data-stu-id="c7758-163">Set the `DOTNET_USE_POLLINGFILEWATCHER` environment variable to `1` or `true` to poll the file system for changes every 4 seconds.</span></span>

## <a name="globbing-patterns"></a><span data-ttu-id="c7758-164">Criteri GLOB</span><span class="sxs-lookup"><span data-stu-id="c7758-164">Globbing patterns</span></span>

<span data-ttu-id="c7758-165">Nei percorsi dei file vengono usati criteri di caratteri jolly chiamati *criteri GLOB*.</span><span class="sxs-lookup"><span data-stu-id="c7758-165">File system paths use wildcard patterns called *globbing patterns*.</span></span> <span data-ttu-id="c7758-166">Questi criteri semplici possono essere usati per specificare gruppi di file.</span><span class="sxs-lookup"><span data-stu-id="c7758-166">These simple patterns can be used to specify groups of files.</span></span> <span data-ttu-id="c7758-167">I due caratteri jolly sono `*` e `**`.</span><span class="sxs-lookup"><span data-stu-id="c7758-167">The two wildcard characters are `*` and `**`.</span></span>

**`*`**

   <span data-ttu-id="c7758-168">Ricerca qualsiasi elemento a livello della cartella corrente o qualsiasi nome file o estensione di file.</span><span class="sxs-lookup"><span data-stu-id="c7758-168">Matches anything at the current folder level, or any filename, or any file extension.</span></span> <span data-ttu-id="c7758-169">Le corrispondenze vengono terminate con i caratteri `/` e `.` nel percorso dei file.</span><span class="sxs-lookup"><span data-stu-id="c7758-169">Matches are terminated by `/` and `.` characters in the file path.</span></span>

<strong><code>**</code></strong>

   <span data-ttu-id="c7758-170">Cerca qualsiasi elemento in più livelli di directory.</span><span class="sxs-lookup"><span data-stu-id="c7758-170">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="c7758-171">Può essere usato per cercare in modo ricorsivo numerosi file all'interno di una gerarchia di directory.</span><span class="sxs-lookup"><span data-stu-id="c7758-171">Can be used to recursively match many files within a directory hierarchy.</span></span>

### <a name="globbing-pattern-examples"></a><span data-ttu-id="c7758-172">Esempi di criteri GLOB</span><span class="sxs-lookup"><span data-stu-id="c7758-172">Globbing pattern examples</span></span>

**`directory/file.txt`**

   <span data-ttu-id="c7758-173">Cerca un file specifico in una directory specifica.</span><span class="sxs-lookup"><span data-stu-id="c7758-173">Matches a specific file in a specific directory.</span></span>

**<code>directory/*.txt</code>**

   <span data-ttu-id="c7758-174">Cerca tutti i file con estensione `.txt` in una directory specifica.</span><span class="sxs-lookup"><span data-stu-id="c7758-174">Matches all files with `.txt` extension in a specific directory.</span></span>

**`directory/*/bower.json`**

   <span data-ttu-id="c7758-175">Cerca tutti i file `bower.json` nelle directory esattamente un livello sotto la directory `directory`.</span><span class="sxs-lookup"><span data-stu-id="c7758-175">Matches all `bower.json` files in directories exactly one level below the `directory` directory.</span></span>

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   <span data-ttu-id="c7758-176">Cerca tutti i file con estensione `.txt` che si trovano in qualsiasi posizione nella directory `directory`.</span><span class="sxs-lookup"><span data-stu-id="c7758-176">Matches all files with `.txt` extension found anywhere under the `directory` directory.</span></span>

## <a name="file-provider-usage-in-aspnet-core"></a><span data-ttu-id="c7758-177">Utilizzo dei provider di file in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7758-177">File Provider usage in ASP.NET Core</span></span>

<span data-ttu-id="c7758-178">Diverse parti di ASP.NET Core utilizzano i provider di file.</span><span class="sxs-lookup"><span data-stu-id="c7758-178">Several parts of ASP.NET Core utilize file providers.</span></span> <span data-ttu-id="c7758-179">`IHostingEnvironment` espone la radice del contenuto dell'app e la radice Web come tipi `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="c7758-179">`IHostingEnvironment` exposes the app's content root and web root as `IFileProvider` types.</span></span> <span data-ttu-id="c7758-180">Il middleware dei file statici usa i provider di file per individuare i file statici.</span><span class="sxs-lookup"><span data-stu-id="c7758-180">The static files middleware uses file providers to locate static files.</span></span> <span data-ttu-id="c7758-181">Razor usa di frequente `IFileProvider` nella ricerca delle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="c7758-181">Razor makes heavy use of `IFileProvider` in locating views.</span></span> <span data-ttu-id="c7758-182">La funzionalità di pubblicazione di Dotnet usa i provider di file e i criteri GLOB per specificare i file da pubblicare.</span><span class="sxs-lookup"><span data-stu-id="c7758-182">Dotnet's publish functionality uses file providers and globbing patterns to specify which files should be published.</span></span>

## <a name="recommendations-for-use-in-apps"></a><span data-ttu-id="c7758-183">Suggerimenti per l'uso nelle app</span><span class="sxs-lookup"><span data-stu-id="c7758-183">Recommendations for use in apps</span></span>

<span data-ttu-id="c7758-184">Se l'app ASP.NET Core richiede l'accesso al file system, è possibile richiedere un'istanza di `IFileProvider` tramite l'inserimento di dipendenze e quindi usarne i metodi per eseguire l'accesso, come illustrato nell'esempio.</span><span class="sxs-lookup"><span data-stu-id="c7758-184">If your ASP.NET Core app requires file system access, you can request an instance of `IFileProvider` through dependency injection, and then use its methods to perform the access, as shown in this sample.</span></span> <span data-ttu-id="c7758-185">In questo modo è possibile configurare il provider una sola volta, ovvero all'avvio dell'app, e ridurre il numero di tipi di implementazioni di cui l'app crea un'istanza.</span><span class="sxs-lookup"><span data-stu-id="c7758-185">This allows you to configure the provider once, when the app starts up, and reduces the number of implementation types your app instantiates.</span></span>
