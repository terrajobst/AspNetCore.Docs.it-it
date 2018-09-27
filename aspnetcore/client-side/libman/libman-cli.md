---
title: Usare l'interfaccia della riga di comando LibMan (CLI) con ASP.NET Core
author: scottaddie
description: Informazioni su come usare l'interfaccia della riga di comando LibMan (CLI) in un progetto ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/30/2018
uid: client-side/libman/libman-cli
ms.openlocfilehash: 5667f79648a60b8fd9496f8041ef08891ab766af
ms.sourcegitcommit: 9bdba90b2c97a4016188434657194b2d7027d6e3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47402085"
---
# <a name="use-the-libman-command-line-interface-cli-with-aspnet-core"></a><span data-ttu-id="387df-103">Usare l'interfaccia della riga di comando LibMan (CLI) con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="387df-103">Use the LibMan command-line interface (CLI) with ASP.NET Core</span></span>

<span data-ttu-id="387df-104">Di [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="387df-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="387df-105">Il [LibMan](xref:client-side/libman/index) CLI è uno strumento multipiattaforma che ha supportato ovunque .NET Core è supportato.</span><span class="sxs-lookup"><span data-stu-id="387df-105">The [LibMan](xref:client-side/libman/index) CLI is a cross-platform tool that's supported everywhere .NET Core is supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="387df-106">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="387df-106">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="387df-107">Installazione</span><span class="sxs-lookup"><span data-stu-id="387df-107">Installation</span></span>

<span data-ttu-id="387df-108">Per installare CLI LibMan:</span><span class="sxs-lookup"><span data-stu-id="387df-108">To install the LibMan CLI:</span></span>

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

<span data-ttu-id="387df-109">Oggetto [strumento globale di .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) viene installato dalle [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="387df-109">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet package.</span></span>

<span data-ttu-id="387df-110">Per installare CLI LibMan da un'origine pacchetto NuGet specifica:</span><span class="sxs-lookup"><span data-stu-id="387df-110">To install the LibMan CLI from a specific NuGet package source:</span></span>

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

<span data-ttu-id="387df-111">Nell'esempio precedente, uno strumento globale di .NET Core è installato nel computer Windows locale *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* file.</span><span class="sxs-lookup"><span data-stu-id="387df-111">In the preceding example, a .NET Core Global Tool is installed from the local Windows machine's *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* file.</span></span>

## <a name="usage"></a><span data-ttu-id="387df-112">Utilizzo</span><span class="sxs-lookup"><span data-stu-id="387df-112">Usage</span></span>

<span data-ttu-id="387df-113">Al termine dell'installazione della riga di comando, è possibile utilizzare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="387df-113">After successful installation of the CLI, the following command can be used:</span></span>

```console
libman
```

<span data-ttu-id="387df-114">Per visualizzare la versione installata dell'interfaccia della riga:</span><span class="sxs-lookup"><span data-stu-id="387df-114">To view the installed CLI version:</span></span>

```console
libman --version
```

<span data-ttu-id="387df-115">Per visualizzare i comandi dell'interfaccia della riga disponibili:</span><span class="sxs-lookup"><span data-stu-id="387df-115">To view the available CLI commands:</span></span>

```console
libman --help
```

<span data-ttu-id="387df-116">Il comando precedente visualizza l'output simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="387df-116">The preceding command displays output similar to the following:</span></span>

```console
 1.0.163+g45474d37ed

Usage: libman [options] [command]

Options:
  --help|-h  Show help information
  --version  Show version information

Commands:
  cache      List or clean libman cache contents
  clean      Deletes all library files defined in libman.json from the project
  init       Create a new libman.json
  install    Add a library definition to the libman.json file, and download the 
             library to the specified location
  restore    Downloads all files from provider and saves them to specified 
             destination
  uninstall  Deletes all files for the specified library from their specified 
             destination, then removes the specified library definition from 
             libman.json
  update     Updates the specified library

Use "libman [command] --help" for more information about a command.
```

<span data-ttu-id="387df-117">Le sezioni seguenti descrivono i comandi dell'interfaccia della riga disponibili.</span><span class="sxs-lookup"><span data-stu-id="387df-117">The following sections outline the available CLI commands.</span></span>

## <a name="initialize-libman-in-the-project"></a><span data-ttu-id="387df-118">Inizializzare LibMan nel progetto</span><span class="sxs-lookup"><span data-stu-id="387df-118">Initialize LibMan in the project</span></span>

<span data-ttu-id="387df-119">Il `libman init` comando crea un *libman.json* file se non ne esiste uno.</span><span class="sxs-lookup"><span data-stu-id="387df-119">The `libman init` command creates a *libman.json* file if one doesn't exist.</span></span> <span data-ttu-id="387df-120">Il file viene creato con il contenuto del modello di elemento predefinito.</span><span class="sxs-lookup"><span data-stu-id="387df-120">The file is created with the default item template content.</span></span>

### <a name="synopsis"></a><span data-ttu-id="387df-121">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="387df-121">Synopsis</span></span>

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a><span data-ttu-id="387df-122">Opzioni</span><span class="sxs-lookup"><span data-stu-id="387df-122">Options</span></span>

<span data-ttu-id="387df-123">Le opzioni seguenti sono disponibili per il `libman init` comando:</span><span class="sxs-lookup"><span data-stu-id="387df-123">The following options are available for the `libman init` command:</span></span>

* `-d|--default-destination <PATH>`

  <span data-ttu-id="387df-124">Un percorso relativo della cartella corrente.</span><span class="sxs-lookup"><span data-stu-id="387df-124">A path relative to the current folder.</span></span> <span data-ttu-id="387df-125">I file di libreria vengono installati in questa posizione se nessun `destination` proprietà è definita per una libreria *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="387df-125">Library files are installed in this location if no `destination` property is defined for a library in *libman.json*.</span></span> <span data-ttu-id="387df-126">Il `<PATH>` valore viene scritto il `defaultDestination` proprietà di *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="387df-126">The `<PATH>` value is written to the `defaultDestination` property of *libman.json*.</span></span>

* `-p|--default-provider <PROVIDER>`

  <span data-ttu-id="387df-127">Il provider da utilizzare se non è stato definito alcun provider per una determinata libreria.</span><span class="sxs-lookup"><span data-stu-id="387df-127">The provider to use if no provider is defined for a given library.</span></span> <span data-ttu-id="387df-128">Il `<PROVIDER>` valore viene scritto il `defaultProvider` proprietà di *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="387df-128">The `<PROVIDER>` value is written to the `defaultProvider` property of *libman.json*.</span></span> <span data-ttu-id="387df-129">Sostituire `<PROVIDER>` con uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="387df-129">Replace `<PROVIDER>` with one of the following values:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="387df-130">Esempi</span><span class="sxs-lookup"><span data-stu-id="387df-130">Examples</span></span>

<span data-ttu-id="387df-131">Per creare un *libman.json* file in un progetto ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="387df-131">To create a *libman.json* file in an ASP.NET Core project:</span></span>

* <span data-ttu-id="387df-132">Passare alla radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="387df-132">Navigate to the project root.</span></span>
* <span data-ttu-id="387df-133">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="387df-133">Run the following command:</span></span>

  ```console
  libman init
  ```

* <span data-ttu-id="387df-134">Digitare il nome del provider predefinito o premere `Enter` per usare il provider CDNJS predefinito.</span><span class="sxs-lookup"><span data-stu-id="387df-134">Type the name of the default provider, or press `Enter` to use the default CDNJS provider.</span></span> <span data-ttu-id="387df-135">I valori validi includono:</span><span class="sxs-lookup"><span data-stu-id="387df-135">Valid values include:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![comando init libman - provider predefinito](_static/libman-init-provider.png)

<span data-ttu-id="387df-137">Oggetto *libman.json* file viene aggiunto alla radice del progetto con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="387df-137">A *libman.json* file is added to the project root with the following content:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a><span data-ttu-id="387df-138">Aggiungere i file di libreria</span><span class="sxs-lookup"><span data-stu-id="387df-138">Add library files</span></span>

<span data-ttu-id="387df-139">Il `libman install` comando Scarica e installa i file di libreria nel progetto.</span><span class="sxs-lookup"><span data-stu-id="387df-139">The `libman install` command downloads and installs library files into the project.</span></span> <span data-ttu-id="387df-140">Oggetto *libman.json* file viene aggiunto se non ne esiste uno.</span><span class="sxs-lookup"><span data-stu-id="387df-140">A *libman.json* file is added if one doesn't exist.</span></span> <span data-ttu-id="387df-141">Il *libman.json* file viene modificato per archiviare i dettagli di configurazione per i file di libreria.</span><span class="sxs-lookup"><span data-stu-id="387df-141">The *libman.json* file is modified to store configuration details for the library files.</span></span>

### <a name="synopsis"></a><span data-ttu-id="387df-142">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="387df-142">Synopsis</span></span>

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="387df-143">Argomenti</span><span class="sxs-lookup"><span data-stu-id="387df-143">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="387df-144">Il nome della raccolta da installare.</span><span class="sxs-lookup"><span data-stu-id="387df-144">The name of the library to install.</span></span> <span data-ttu-id="387df-145">Questo nome può includere una notazione numero versione (ad esempio, `@1.2.0`).</span><span class="sxs-lookup"><span data-stu-id="387df-145">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="387df-146">Opzioni</span><span class="sxs-lookup"><span data-stu-id="387df-146">Options</span></span>

<span data-ttu-id="387df-147">Le opzioni seguenti sono disponibili per il `libman install` comando:</span><span class="sxs-lookup"><span data-stu-id="387df-147">The following options are available for the `libman install` command:</span></span>

* `-d|--destination <PATH>`

  <span data-ttu-id="387df-148">Posizione in cui installare la libreria.</span><span class="sxs-lookup"><span data-stu-id="387df-148">The location to install the library.</span></span> <span data-ttu-id="387df-149">Se non specificato, viene utilizzato il percorso predefinito.</span><span class="sxs-lookup"><span data-stu-id="387df-149">If not specified, the default location is used.</span></span> <span data-ttu-id="387df-150">Se nessun `defaultDestination` proprietà viene specificata *libman.json*, questa opzione è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="387df-150">If no `defaultDestination` property is specified in *libman.json*, this option is required.</span></span>

* `--files <FILE>`

  <span data-ttu-id="387df-151">Specificare il nome del file di installazione dalla libreria.</span><span class="sxs-lookup"><span data-stu-id="387df-151">Specify the name of the file to install from the library.</span></span> <span data-ttu-id="387df-152">Se non specificato, vengono installati tutti i file dalla libreria.</span><span class="sxs-lookup"><span data-stu-id="387df-152">If not specified, all files from the library are installed.</span></span> <span data-ttu-id="387df-153">Fornisce uno `--files` essere installate le opzioni per ogni file.</span><span class="sxs-lookup"><span data-stu-id="387df-153">Provide one `--files` option per file to be installed.</span></span> <span data-ttu-id="387df-154">Sono supportati anche i percorsi relativi.</span><span class="sxs-lookup"><span data-stu-id="387df-154">Relative paths are supported too.</span></span> <span data-ttu-id="387df-155">Ad esempio: `--files dist/browser/signalr.js`.</span><span class="sxs-lookup"><span data-stu-id="387df-155">For example: `--files dist/browser/signalr.js`.</span></span>

* `-p|--provider <PROVIDER>`

  <span data-ttu-id="387df-156">Il nome del provider da utilizzare per l'acquisizione della libreria.</span><span class="sxs-lookup"><span data-stu-id="387df-156">The name of the provider to use for the library acquisition.</span></span> <span data-ttu-id="387df-157">Sostituire `<PROVIDER>` con uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="387df-157">Replace `<PROVIDER>` with one of the following values:</span></span>
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  <span data-ttu-id="387df-158">Se non specificato, il `defaultProvider` proprietà nel *libman.json* viene usato.</span><span class="sxs-lookup"><span data-stu-id="387df-158">If not specified, the `defaultProvider` property in *libman.json* is used.</span></span> <span data-ttu-id="387df-159">Se nessun `defaultProvider` proprietà viene specificata *libman.json*, questa opzione è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="387df-159">If no `defaultProvider` property is specified in *libman.json*, this option is required.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="387df-160">Esempi</span><span class="sxs-lookup"><span data-stu-id="387df-160">Examples</span></span>

<span data-ttu-id="387df-161">Tenere presente quanto segue *libman.json* file:</span><span class="sxs-lookup"><span data-stu-id="387df-161">Consider the following *libman.json* file:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

<span data-ttu-id="387df-162">Per installare la versione di jQuery 3.2.1 *jquery.min.js* del file per il *wwwroot/script/jquery* cartella utilizzando il provider CDNJS:</span><span class="sxs-lookup"><span data-stu-id="387df-162">To install the jQuery version 3.2.1 *jquery.min.js* file to the *wwwroot/scripts/jquery* folder using the CDNJS provider:</span></span>

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot/scripts/jquery --files jquery.min.js
```

<span data-ttu-id="387df-163">Il *libman.json* file è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="387df-163">The *libman.json* file resembles the following:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot/scripts/jquery",
      "files": [
        "jquery.min.js"
      ]
    }
  ]
}
```

<span data-ttu-id="387df-164">Per installare il *calendar.js* e *calendar.css* dei file dalla *c:\\temp\\contosoCalendar\\*  utilizzando il file system provider:</span><span class="sxs-lookup"><span data-stu-id="387df-164">To install the *calendar.js* and *calendar.css* files from *C:\\temp\\contosoCalendar\\* using the file system provider:</span></span>

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

<span data-ttu-id="387df-165">Viene visualizzato il prompt seguente per due motivi:</span><span class="sxs-lookup"><span data-stu-id="387df-165">The following prompt appears for two reasons:</span></span>

* <span data-ttu-id="387df-166">Il *libman.json* file non contiene un `defaultDestination` proprietà.</span><span class="sxs-lookup"><span data-stu-id="387df-166">The *libman.json* file doesn't contain a `defaultDestination` property.</span></span>
* <span data-ttu-id="387df-167">Il `libman install` comando non contiene il `-d|--destination` opzione.</span><span class="sxs-lookup"><span data-stu-id="387df-167">The `libman install` command doesn't contain the `-d|--destination` option.</span></span>

![libman installare command - destinazione](_static/libman-install-destination.png)

<span data-ttu-id="387df-169">Dopo aver accettato la destinazione predefinita, il *libman.json* file è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="387df-169">After accepting the default destination, the *libman.json* file resembles the following:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot/scripts/jquery",
      "files": [
        "jquery.min.js"
      ]
    },
    {
      "library": "C:\\temp\\contosoCalendar\\",
      "provider": "filesystem",
      "destination": "wwwroot/lib/contosoCalendar",
      "files": [
        "calendar.js",
        "calendar.css"
      ]
    }
  ]
}
```

## <a name="restore-library-files"></a><span data-ttu-id="387df-170">Ripristinare i file di libreria</span><span class="sxs-lookup"><span data-stu-id="387df-170">Restore library files</span></span>

<span data-ttu-id="387df-171">Il `libman restore` comando Installa definiti nel file di libreria *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="387df-171">The `libman restore` command installs library files defined in *libman.json*.</span></span> <span data-ttu-id="387df-172">È necessario attenersi alle regole che seguono:</span><span class="sxs-lookup"><span data-stu-id="387df-172">The following rules apply:</span></span>

* <span data-ttu-id="387df-173">Se nessun *libman.json* file presente nella radice del progetto, viene restituito un errore.</span><span class="sxs-lookup"><span data-stu-id="387df-173">If no *libman.json* file exists in the project root, an error is returned.</span></span>
* <span data-ttu-id="387df-174">Se una libreria specifica di un provider, il `defaultProvider` proprietà nel *libman.json* viene ignorato.</span><span class="sxs-lookup"><span data-stu-id="387df-174">If a library specifies a provider, the `defaultProvider` property in *libman.json* is ignored.</span></span>
* <span data-ttu-id="387df-175">Se una libreria specifica una destinazione, il `defaultDestination` proprietà nel *libman.json* viene ignorato.</span><span class="sxs-lookup"><span data-stu-id="387df-175">If a library specifies a destination, the `defaultDestination` property in *libman.json* is ignored.</span></span>

### <a name="synopsis"></a><span data-ttu-id="387df-176">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="387df-176">Synopsis</span></span>

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a><span data-ttu-id="387df-177">Opzioni</span><span class="sxs-lookup"><span data-stu-id="387df-177">Options</span></span>

<span data-ttu-id="387df-178">Le opzioni seguenti sono disponibili per il `libman restore` comando:</span><span class="sxs-lookup"><span data-stu-id="387df-178">The following options are available for the `libman restore` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="387df-179">Esempi</span><span class="sxs-lookup"><span data-stu-id="387df-179">Examples</span></span>

<span data-ttu-id="387df-180">Per ripristinare i file della libreria definiti in *libman.json*:</span><span class="sxs-lookup"><span data-stu-id="387df-180">To restore the library files defined in *libman.json*:</span></span>

```console
libman restore
```

## <a name="delete-library-files"></a><span data-ttu-id="387df-181">Eliminare i file di libreria</span><span class="sxs-lookup"><span data-stu-id="387df-181">Delete library files</span></span>

<span data-ttu-id="387df-182">Il `libman clean` comando Elimina i file di libreria precedentemente ripristinati tramite LibMan.</span><span class="sxs-lookup"><span data-stu-id="387df-182">The `libman clean` command deletes library files previously restored via LibMan.</span></span> <span data-ttu-id="387df-183">Le cartelle che diventano vuote dopo questa operazione vengono eliminate.</span><span class="sxs-lookup"><span data-stu-id="387df-183">Folders that become empty after this operation are deleted.</span></span> <span data-ttu-id="387df-184">I file di libreria associate le configurazioni nel `libraries` proprietà di *libman.json* non vengono rimossi.</span><span class="sxs-lookup"><span data-stu-id="387df-184">The library files' associated configurations in the `libraries` property of *libman.json* aren't removed.</span></span>

### <a name="synopsis"></a><span data-ttu-id="387df-185">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="387df-185">Synopsis</span></span>

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a><span data-ttu-id="387df-186">Opzioni</span><span class="sxs-lookup"><span data-stu-id="387df-186">Options</span></span>

<span data-ttu-id="387df-187">Le opzioni seguenti sono disponibili per il `libman clean` comando:</span><span class="sxs-lookup"><span data-stu-id="387df-187">The following options are available for the `libman clean` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="387df-188">Esempi</span><span class="sxs-lookup"><span data-stu-id="387df-188">Examples</span></span>

<span data-ttu-id="387df-189">Per eliminare i file di libreria installati tramite LibMan:</span><span class="sxs-lookup"><span data-stu-id="387df-189">To delete library files installed via LibMan:</span></span>

```console
libman clean
```

## <a name="uninstall-library-files"></a><span data-ttu-id="387df-190">Disinstallare i file di libreria</span><span class="sxs-lookup"><span data-stu-id="387df-190">Uninstall library files</span></span>

<span data-ttu-id="387df-191">Il `libman uninstall` comando:</span><span class="sxs-lookup"><span data-stu-id="387df-191">The `libman uninstall` command:</span></span>

* <span data-ttu-id="387df-192">Elimina tutti i file associati con la libreria specificata dall'oggetto di destinazione in *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="387df-192">Deletes all files associated with the specified library from the destination in *libman.json*.</span></span>
* <span data-ttu-id="387df-193">Rimuove la configurazione della raccolta associata dal *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="387df-193">Removes the associated library configuration from *libman.json*.</span></span>

<span data-ttu-id="387df-194">Si verifica un errore quando:</span><span class="sxs-lookup"><span data-stu-id="387df-194">An error occurs when:</span></span>

* <span data-ttu-id="387df-195">No *libman.json* file presente nella radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="387df-195">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="387df-196">La libreria specificata non esiste.</span><span class="sxs-lookup"><span data-stu-id="387df-196">The specified library doesn't exist.</span></span>

<span data-ttu-id="387df-197">Se è installata più di una libreria con lo stesso nome, viene chiesto di scegliere uno.</span><span class="sxs-lookup"><span data-stu-id="387df-197">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="387df-198">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="387df-198">Synopsis</span></span>

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="387df-199">Argomenti</span><span class="sxs-lookup"><span data-stu-id="387df-199">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="387df-200">Il nome della raccolta da disinstallare.</span><span class="sxs-lookup"><span data-stu-id="387df-200">The name of the library to uninstall.</span></span> <span data-ttu-id="387df-201">Questo nome può includere una notazione numero versione (ad esempio, `@1.2.0`).</span><span class="sxs-lookup"><span data-stu-id="387df-201">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="387df-202">Opzioni</span><span class="sxs-lookup"><span data-stu-id="387df-202">Options</span></span>

<span data-ttu-id="387df-203">Le opzioni seguenti sono disponibili per il `libman uninstall` comando:</span><span class="sxs-lookup"><span data-stu-id="387df-203">The following options are available for the `libman uninstall` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="387df-204">Esempi</span><span class="sxs-lookup"><span data-stu-id="387df-204">Examples</span></span>

<span data-ttu-id="387df-205">Tenere presente quanto segue *libman.json* file:</span><span class="sxs-lookup"><span data-stu-id="387df-205">Consider the following *libman.json* file:</span></span>

[!code-json[](samples/LibManSample/libman.json)]

* <span data-ttu-id="387df-206">Per disinstallare jQuery, uno dei seguenti comandi esito positivo:</span><span class="sxs-lookup"><span data-stu-id="387df-206">To uninstall jQuery, either of the following commands succeed:</span></span>

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* <span data-ttu-id="387df-207">Per disinstallare i file Lodash installati tramite il `filesystem` provider:</span><span class="sxs-lookup"><span data-stu-id="387df-207">To uninstall the Lodash files installed via the `filesystem` provider:</span></span>

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a><span data-ttu-id="387df-208">Versione di aggiornamento libreria</span><span class="sxs-lookup"><span data-stu-id="387df-208">Update library version</span></span>

<span data-ttu-id="387df-209">Il `libman update` comando Aggiorna una libreria installata tramite LibMan alla versione specificata.</span><span class="sxs-lookup"><span data-stu-id="387df-209">The `libman update` command updates a library installed via LibMan to the specified version.</span></span>

<span data-ttu-id="387df-210">Si verifica un errore quando:</span><span class="sxs-lookup"><span data-stu-id="387df-210">An error occurs when:</span></span>

* <span data-ttu-id="387df-211">No *libman.json* file presente nella radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="387df-211">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="387df-212">La libreria specificata non esiste.</span><span class="sxs-lookup"><span data-stu-id="387df-212">The specified library doesn't exist.</span></span>

<span data-ttu-id="387df-213">Se è installata più di una libreria con lo stesso nome, viene chiesto di scegliere uno.</span><span class="sxs-lookup"><span data-stu-id="387df-213">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="387df-214">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="387df-214">Synopsis</span></span>

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="387df-215">Argomenti</span><span class="sxs-lookup"><span data-stu-id="387df-215">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="387df-216">Il nome della raccolta da aggiornare.</span><span class="sxs-lookup"><span data-stu-id="387df-216">The name of the library to update.</span></span>

### <a name="options"></a><span data-ttu-id="387df-217">Opzioni</span><span class="sxs-lookup"><span data-stu-id="387df-217">Options</span></span>

<span data-ttu-id="387df-218">Le opzioni seguenti sono disponibili per il `libman update` comando:</span><span class="sxs-lookup"><span data-stu-id="387df-218">The following options are available for the `libman update` command:</span></span>

* `-pre`

  <span data-ttu-id="387df-219">Ottenere la versione provvisoria più recente della libreria.</span><span class="sxs-lookup"><span data-stu-id="387df-219">Obtain the latest prerelease version of the library.</span></span>

* `--to <VERSION>`

  <span data-ttu-id="387df-220">Ottenere una versione specifica della libreria.</span><span class="sxs-lookup"><span data-stu-id="387df-220">Obtain a specific version of the library.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="387df-221">Esempi</span><span class="sxs-lookup"><span data-stu-id="387df-221">Examples</span></span>

* <span data-ttu-id="387df-222">L'aggiornamento alla versione più recente di jQuery:</span><span class="sxs-lookup"><span data-stu-id="387df-222">To update jQuery to the latest version:</span></span>

  ```console
  libman update jquery
  ```

* <span data-ttu-id="387df-223">Per aggiornare jQuery alla versione 3.3.1:</span><span class="sxs-lookup"><span data-stu-id="387df-223">To update jQuery to version 3.3.1:</span></span>

  ```console
  libman update jquery --to 3.3.1
  ```

* <span data-ttu-id="387df-224">Per aggiornare la versione provvisoria più recente di jQuery:</span><span class="sxs-lookup"><span data-stu-id="387df-224">To update jQuery to the latest prerelease version:</span></span>

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a><span data-ttu-id="387df-225">Gestire cache della libreria</span><span class="sxs-lookup"><span data-stu-id="387df-225">Manage library cache</span></span>

<span data-ttu-id="387df-226">Il `libman cache` comando gestisce la cache della libreria LibMan.</span><span class="sxs-lookup"><span data-stu-id="387df-226">The `libman cache` command manages the LibMan library cache.</span></span> <span data-ttu-id="387df-227">Il `filesystem` provider non usa la cache della libreria.</span><span class="sxs-lookup"><span data-stu-id="387df-227">The `filesystem` provider doesn't use the library cache.</span></span>

### <a name="synopsis"></a><span data-ttu-id="387df-228">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="387df-228">Synopsis</span></span>

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="387df-229">Argomenti</span><span class="sxs-lookup"><span data-stu-id="387df-229">Arguments</span></span>

`PROVIDER`

<span data-ttu-id="387df-230">Usata solo con il `clean` comando.</span><span class="sxs-lookup"><span data-stu-id="387df-230">Only used with the `clean` command.</span></span> <span data-ttu-id="387df-231">Specifica la cache del provider da pulire.</span><span class="sxs-lookup"><span data-stu-id="387df-231">Specifies the provider cache to clean.</span></span> <span data-ttu-id="387df-232">I valori validi includono:</span><span class="sxs-lookup"><span data-stu-id="387df-232">Valid values include:</span></span>

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a><span data-ttu-id="387df-233">Opzioni</span><span class="sxs-lookup"><span data-stu-id="387df-233">Options</span></span>

<span data-ttu-id="387df-234">Le opzioni seguenti sono disponibili per il `libman cache` comando:</span><span class="sxs-lookup"><span data-stu-id="387df-234">The following options are available for the `libman cache` command:</span></span>

* `--files`

  <span data-ttu-id="387df-235">Elencare i nomi dei file memorizzati nella cache.</span><span class="sxs-lookup"><span data-stu-id="387df-235">List the names of files that are cached.</span></span>

* `--libraries`

  <span data-ttu-id="387df-236">Elencare i nomi di librerie che vengono memorizzati nella cache.</span><span class="sxs-lookup"><span data-stu-id="387df-236">List the names of libraries that are cached.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="387df-237">Esempi</span><span class="sxs-lookup"><span data-stu-id="387df-237">Examples</span></span>

* <span data-ttu-id="387df-238">Per visualizzare i nomi delle librerie memorizzato nella cache per ogni provider, usare uno dei seguenti comandi:</span><span class="sxs-lookup"><span data-stu-id="387df-238">To view the names of cached libraries per provider, use one of the following commands:</span></span>

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  <span data-ttu-id="387df-239">Viene visualizzato output simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="387df-239">Output similar to the following is displayed:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      font-awesome
      jquery
      knockout
      lodash.js
      react
  ```

* <span data-ttu-id="387df-240">Per visualizzare i nomi dei file di libreria memorizzato nella cache per ogni provider:</span><span class="sxs-lookup"><span data-stu-id="387df-240">To view the names of cached library files per provider:</span></span>

  ```console
  libman cache list --files
  ```

  <span data-ttu-id="387df-241">Viene visualizzato output simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="387df-241">Output similar to the following is displayed:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout:
          <list omitted for brevity>
      react:
          <list omitted for brevity>
      vue:
          <list omitted for brevity>
  cdnjs:
      font-awesome
          metadata.json
      jquery
          metadata.json
          3.2.1\core.js
          3.2.1\jquery.js
          3.2.1\jquery.min.js
          3.2.1\jquery.min.map
          3.2.1\jquery.slim.js
          3.2.1\jquery.slim.min.js
          3.2.1\jquery.slim.min.map
          3.3.1\core.js
          3.3.1\jquery.js
          3.3.1\jquery.min.js
          3.3.1\jquery.min.map
          3.3.1\jquery.slim.js
          3.3.1\jquery.slim.min.js
          3.3.1\jquery.slim.min.map
      knockout
          metadata.json
          3.4.2\knockout-debug.js
          3.4.2\knockout-min.js
      lodash.js
          metadata.json
          4.17.10\lodash.js
          4.17.10\lodash.min.js
      react
          metadata.json
  ```

  <span data-ttu-id="387df-242">Si noti che l'output precedente mostra che jQuery versioni 3.2.1 e 3.3.1 vengono memorizzati nella cache del provider di CDNJS.</span><span class="sxs-lookup"><span data-stu-id="387df-242">Notice the preceding output shows that jQuery versions 3.2.1 and 3.3.1 are cached under the CDNJS provider.</span></span>

* <span data-ttu-id="387df-243">Per svuotare la cache della libreria per il provider CDNJS:</span><span class="sxs-lookup"><span data-stu-id="387df-243">To empty the library cache for the CDNJS provider:</span></span>

  ```console
  libman cache clean cdnjs
  ```

  <span data-ttu-id="387df-244">Dopo aver svuotare la cache del provider CDNJS, il `libman cache list` comandi Visualizza quanto segue:</span><span class="sxs-lookup"><span data-stu-id="387df-244">After emptying the CDNJS provider cache, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      (empty)
  ```

* <span data-ttu-id="387df-245">Per svuotare la cache per tutti i provider supportati:</span><span class="sxs-lookup"><span data-stu-id="387df-245">To empty the cache for all supported providers:</span></span>

  ```console
  libman cache clean
  ```

  <span data-ttu-id="387df-246">Dopo tutte le cache del provider, svuotare il `libman cache list` comandi Visualizza quanto segue:</span><span class="sxs-lookup"><span data-stu-id="387df-246">After emptying all provider caches, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a><span data-ttu-id="387df-247">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="387df-247">Additional resources</span></span>

* [<span data-ttu-id="387df-248">Installare uno strumento globale</span><span class="sxs-lookup"><span data-stu-id="387df-248">Install a Global Tool</span></span>](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [<span data-ttu-id="387df-249">Repository GitHub di LibMan</span><span class="sxs-lookup"><span data-stu-id="387df-249">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
