---
title: Usare l'interfaccia della riga di comando LibMan (CLI) con ASP.NET Core
author: scottaddie
description: Informazioni su come usare l'interfaccia della riga di comando LibMan (CLI) in un progetto ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/30/2018
uid: client-side/libman/libman-cli
ms.openlocfilehash: 5667f79648a60b8fd9496f8041ef08891ab766af
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64894758"
---
# <a name="use-the-libman-command-line-interface-cli-with-aspnet-core"></a>Usare l'interfaccia della riga di comando LibMan (CLI) con ASP.NET Core

Di [Scott Addie](https://twitter.com/Scott_Addie)

Il [LibMan](xref:client-side/libman/index) CLI è uno strumento multipiattaforma che ha supportato ovunque .NET Core è supportato.

## <a name="prerequisites"></a>Prerequisiti

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a>Installazione

Per installare CLI LibMan:

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

Oggetto [strumento globale di .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) viene installato dalle [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) pacchetto NuGet.

Per installare CLI LibMan da un'origine pacchetto NuGet specifica:

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

Nell'esempio precedente, uno strumento globale di .NET Core è installato nel computer Windows locale *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* file.

## <a name="usage"></a>Utilizzo

Al termine dell'installazione della riga di comando, è possibile utilizzare il comando seguente:

```console
libman
```

Per visualizzare la versione installata dell'interfaccia della riga:

```console
libman --version
```

Per visualizzare i comandi dell'interfaccia della riga disponibili:

```console
libman --help
```

Il comando precedente visualizza l'output simile al seguente:

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

Le sezioni seguenti descrivono i comandi dell'interfaccia della riga disponibili.

## <a name="initialize-libman-in-the-project"></a>Inizializzare LibMan nel progetto

Il `libman init` comando crea un *libman.json* file se non ne esiste uno. Il file viene creato con il contenuto del modello di elemento predefinito.

### <a name="synopsis"></a>Riepilogo

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a>Opzioni

Le opzioni seguenti sono disponibili per il `libman init` comando:

* `-d|--default-destination <PATH>`

  Un percorso relativo della cartella corrente. I file di libreria vengono installati in questa posizione se nessun `destination` proprietà è definita per una libreria *libman.json*. Il `<PATH>` valore viene scritto il `defaultDestination` proprietà di *libman.json*.

* `-p|--default-provider <PROVIDER>`

  Il provider da utilizzare se non è stato definito alcun provider per una determinata libreria. Il `<PROVIDER>` valore viene scritto il `defaultProvider` proprietà di *libman.json*. Sostituire `<PROVIDER>` con uno dei valori seguenti:

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Esempi

Per creare un *libman.json* file in un progetto ASP.NET Core:

* Passare alla radice del progetto.
* Eseguire il comando seguente:

  ```console
  libman init
  ```

* Digitare il nome del provider predefinito o premere `Enter` per usare il provider CDNJS predefinito. I valori validi includono:

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![comando init libman - provider predefinito](_static/libman-init-provider.png)

Oggetto *libman.json* file viene aggiunto alla radice del progetto con il contenuto seguente:

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a>Aggiungere i file di libreria

Il `libman install` comando Scarica e installa i file di libreria nel progetto. Oggetto *libman.json* file viene aggiunto se non ne esiste uno. Il *libman.json* file viene modificato per archiviare i dettagli di configurazione per i file di libreria.

### <a name="synopsis"></a>Riepilogo

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a>Argomenti

`LIBRARY`

Il nome della raccolta da installare. Questo nome può includere una notazione numero versione (ad esempio, `@1.2.0`).

### <a name="options"></a>Opzioni

Le opzioni seguenti sono disponibili per il `libman install` comando:

* `-d|--destination <PATH>`

  Posizione in cui installare la libreria. Se non specificato, viene utilizzato il percorso predefinito. Se nessun `defaultDestination` proprietà viene specificata *libman.json*, questa opzione è obbligatoria.

* `--files <FILE>`

  Specificare il nome del file di installazione dalla libreria. Se non specificato, vengono installati tutti i file dalla libreria. Fornisce uno `--files` essere installate le opzioni per ogni file. Sono supportati anche i percorsi relativi. Ad esempio: `--files dist/browser/signalr.js`.

* `-p|--provider <PROVIDER>`

  Il nome del provider da utilizzare per l'acquisizione della libreria. Sostituire `<PROVIDER>` con uno dei valori seguenti:
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  Se non specificato, il `defaultProvider` proprietà nel *libman.json* viene usato. Se nessun `defaultProvider` proprietà viene specificata *libman.json*, questa opzione è obbligatoria.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Esempi

Tenere presente quanto segue *libman.json* file:

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

Per installare la versione di jQuery 3.2.1 *jquery.min.js* del file per il *wwwroot/script/jquery* cartella utilizzando il provider CDNJS:

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot/scripts/jquery --files jquery.min.js
```

Il *libman.json* file è simile al seguente:

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

Per installare il *calendar.js* e *calendar.css* dei file dalla *c:\\temp\\contosoCalendar\\*  utilizzando il file system provider:

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

Viene visualizzato il prompt seguente per due motivi:

* Il *libman.json* file non contiene un `defaultDestination` proprietà.
* Il `libman install` comando non contiene il `-d|--destination` opzione.

![libman installare command - destinazione](_static/libman-install-destination.png)

Dopo aver accettato la destinazione predefinita, il *libman.json* file è simile al seguente:

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

## <a name="restore-library-files"></a>Ripristinare i file di libreria

Il `libman restore` comando Installa definiti nel file di libreria *libman.json*. È necessario attenersi alle regole che seguono:

* Se nessun *libman.json* file presente nella radice del progetto, viene restituito un errore.
* Se una libreria specifica di un provider, il `defaultProvider` proprietà nel *libman.json* viene ignorato.
* Se una libreria specifica una destinazione, il `defaultDestination` proprietà nel *libman.json* viene ignorato.

### <a name="synopsis"></a>Riepilogo

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a>Opzioni

Le opzioni seguenti sono disponibili per il `libman restore` comando:

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Esempi

Per ripristinare i file della libreria definiti in *libman.json*:

```console
libman restore
```

## <a name="delete-library-files"></a>Eliminare i file di libreria

Il `libman clean` comando Elimina i file di libreria precedentemente ripristinati tramite LibMan. Le cartelle che diventano vuote dopo questa operazione vengono eliminate. I file di libreria associate le configurazioni nel `libraries` proprietà di *libman.json* non vengono rimossi.

### <a name="synopsis"></a>Riepilogo

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a>Opzioni

Le opzioni seguenti sono disponibili per il `libman clean` comando:

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Esempi

Per eliminare i file di libreria installati tramite LibMan:

```console
libman clean
```

## <a name="uninstall-library-files"></a>Disinstallare i file di libreria

Il `libman uninstall` comando:

* Elimina tutti i file associati con la libreria specificata dall'oggetto di destinazione in *libman.json*.
* Rimuove la configurazione della raccolta associata dal *libman.json*.

Si verifica un errore quando:

* No *libman.json* file presente nella radice del progetto.
* La libreria specificata non esiste.

Se è installata più di una libreria con lo stesso nome, viene chiesto di scegliere uno.

### <a name="synopsis"></a>Riepilogo

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a>Argomenti

`LIBRARY`

Il nome della raccolta da disinstallare. Questo nome può includere una notazione numero versione (ad esempio, `@1.2.0`).

### <a name="options"></a>Opzioni

Le opzioni seguenti sono disponibili per il `libman uninstall` comando:

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Esempi

Tenere presente quanto segue *libman.json* file:

[!code-json[](samples/LibManSample/libman.json)]

* Per disinstallare jQuery, uno dei seguenti comandi esito positivo:

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* Per disinstallare i file Lodash installati tramite il `filesystem` provider:

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a>Versione di aggiornamento libreria

Il `libman update` comando Aggiorna una libreria installata tramite LibMan alla versione specificata.

Si verifica un errore quando:

* No *libman.json* file presente nella radice del progetto.
* La libreria specificata non esiste.

Se è installata più di una libreria con lo stesso nome, viene chiesto di scegliere uno.

### <a name="synopsis"></a>Riepilogo

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a>Argomenti

`LIBRARY`

Il nome della raccolta da aggiornare.

### <a name="options"></a>Opzioni

Le opzioni seguenti sono disponibili per il `libman update` comando:

* `-pre`

  Ottenere la versione provvisoria più recente della libreria.

* `--to <VERSION>`

  Ottenere una versione specifica della libreria.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Esempi

* L'aggiornamento alla versione più recente di jQuery:

  ```console
  libman update jquery
  ```

* Per aggiornare jQuery alla versione 3.3.1:

  ```console
  libman update jquery --to 3.3.1
  ```

* Per aggiornare la versione provvisoria più recente di jQuery:

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a>Gestire cache della libreria

Il `libman cache` comando gestisce la cache della libreria LibMan. Il `filesystem` provider non usa la cache della libreria.

### <a name="synopsis"></a>Riepilogo

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a>Argomenti

`PROVIDER`

Usata solo con il `clean` comando. Specifica la cache del provider da pulire. I valori validi includono:

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a>Opzioni

Le opzioni seguenti sono disponibili per il `libman cache` comando:

* `--files`

  Elencare i nomi dei file memorizzati nella cache.

* `--libraries`

  Elencare i nomi di librerie che vengono memorizzati nella cache.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Esempi

* Per visualizzare i nomi delle librerie memorizzato nella cache per ogni provider, usare uno dei seguenti comandi:

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  Viene visualizzato output simile al seguente:

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

* Per visualizzare i nomi dei file di libreria memorizzato nella cache per ogni provider:

  ```console
  libman cache list --files
  ```

  Viene visualizzato output simile al seguente:

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

  Si noti che l'output precedente mostra che jQuery versioni 3.2.1 e 3.3.1 vengono memorizzati nella cache del provider di CDNJS.

* Per svuotare la cache della libreria per il provider CDNJS:

  ```console
  libman cache clean cdnjs
  ```

  Dopo aver svuotare la cache del provider CDNJS, il `libman cache list` comandi Visualizza quanto segue:

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

* Per svuotare la cache per tutti i provider supportati:

  ```console
  libman cache clean
  ```

  Dopo tutte le cache del provider, svuotare il `libman cache list` comandi Visualizza quanto segue:

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a>Risorse aggiuntive

* [Installare uno strumento globale](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [Repository GitHub di LibMan](https://github.com/aspnet/LibraryManager)
