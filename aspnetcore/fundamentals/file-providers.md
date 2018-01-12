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
# <a name="file-providers-in-aspnet-core"></a>Provider di file in ASP.NET Core

Da [Steve Smith](https://ardalis.com/)

ASP.NET Core astrae l'accesso al file system tramite l'utilizzo di provider di File.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="file-provider-abstractions"></a>Astrazioni di Provider di file

Provider di file sono un'astrazione sui sistemi di file. L'interfaccia principale è `IFileProvider`. `IFileProvider`espone i metodi per ottenere informazioni sul file (`IFileInfo`), le informazioni di directory (`IDirectoryContents`) e per impostare le notifiche di modifica (utilizzando un `IChangeToken`).

`IFileInfo`fornisce metodi e proprietà sui singoli file o directory. Ha due proprietà booleane, `Exists` e `IsDirectory`, nonché le proprietà che descrivono il file `Name`, `Length` (in byte), e `LastModified` Data. È possibile leggere dal file utilizzando il relativo `CreateReadStream` metodo.

## <a name="file-provider-implementations"></a>Le implementazioni del Provider di file

Tre implementazioni di `IFileProvider` sono disponibili: incorporata, fisico e composito. Il provider fisico viene utilizzato per accedere ai file del sistema in uso. Il provider incorporato viene utilizzato per accedere ai file incorporati negli assembly. Il provider composito viene utilizzato per fornire accesso combinato al file e directory da uno o più provider.

### <a name="physicalfileprovider"></a>PhysicalFileProvider

Il `PhysicalFileProvider` fornisce l'accesso al file system fisico. Esegue il wrapping di `System.IO.File` tipo (per il provider fisico), dell'ambito di tutti i percorsi di una directory e i relativi elementi figlio. Questo ambito limita l'accesso a una directory e i relativi elementi figlio, impedendo l'accesso al file system di fuori di questo limite. Quando si crea questo provider, è necessario fornirlo con un percorso di directory che serve come percorso di base per tutte le richieste effettuate a questo provider (e che limita l'accesso all'esterno di questo percorso). In un'applicazione ASP.NET di base, è possibile creare un'istanza di un `PhysicalFileProvider` provider direttamente oppure è possibile richiedere un `IFileProvider` in un Controller o del servizio tramite [inserimento di dipendenze](dependency-injection.md). Il secondo approccio produrrà in genere una soluzione più flessibile e testabile.

Nell'esempio seguente viene illustrato come creare un `PhysicalFileProvider`.


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

È possibile scorrere il contenuto della directory o informazioni fornendo un sottopercorso del file specifico.

Per richiedere un provider da un controller, specificarlo nel costruttore del controller e la assegna a un campo locale. Utilizzare l'istanza locale di metodi di azione:

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

Quindi il provider di creare l'app `Startup` classe:

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

Nel *cshtml* fare scorrere il `IDirectoryContents` fornito:

[!code-html[Main](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

il risultato:

![File provider esempio applicazione elenco fisico file e cartelle](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a>EmbeddedFileProvider

Il `EmbeddedFileProvider` viene utilizzato per accedere ai file incorporati negli assembly. In .NET Core, incorporare file in un assembly con il `<EmbeddedResource>` elemento il *csproj* file:

[!code-json[Main](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

È possibile utilizzare [modelli il glob](#globbing-patterns) quando si specificano file da incorporare nell'assembly. Questi modelli possono essere utilizzati per la corrispondenza di uno o più file.

> [!NOTE]
> È improbabile che si desidera mai incorpora ogni file. js del progetto nell'assembly; l'esempio precedente è solo a scopo dimostrativo.

Quando si crea un `EmbeddedFileProvider`, passare l'assembly verrà visualizzato al costruttore.

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

Il frammento di codice sopra riportato di seguito viene illustrato come creare un `EmbeddedFileProvider` con accesso all'assembly attualmente in esecuzione.

Aggiornamento dell'app di esempio per utilizzare un `EmbeddedFileProvider` otterrà l'output seguente:

![Applicazione di esempio di provider file elenco di file incorporati](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> Directory non espongono le risorse incorporate. Invece, il percorso della risorsa (tramite lo spazio dei nomi) è incorporato nel nome di file mediante `.` separatori.

> [!TIP]
> Il `EmbeddedFileProvider` costruttore accetta un parametro facoltativo `baseNamespace` parametro. La definizione di questo ambito chiamate a `GetDirectoryContents` a tali risorse nello spazio dei nomi specificato.

### <a name="compositefileprovider"></a>CompositeFileProvider

Il `CompositeFileProvider` combina `IFileProvider` istanze, l'esposizione di una singola interfaccia per l'utilizzo di file da più provider. Quando si crea il `CompositeFileProvider`, si passa a uno o più `IFileProvider` istanze al costruttore:

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

Aggiornamento dell'app di esempio per utilizzare un `CompositeFileProvider` che include entrambi i provider, fisici e incorporati configurati in precedenza, viene generato il seguente output:

![Applicazione di esempio di provider file listato fisici file e cartelle e file incorporati](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a>Analizzare le modifiche apportate

Il `IFileProvider` `Watch` metodo fornisce un modo per controllare uno o più file o directory per le modifiche. Questo metodo accetta una stringa di percorso, che è possibile utilizzare [modelli il glob](#globbing-patterns) per specificare più file e restituisce un `IChangeToken`. Questo token espone un `HasChanged` proprietà che può essere controllato, e un `RegisterChangeCallback` metodo che viene chiamato quando vengono rilevate modifiche alla stringa di percorso specificata. Si noti che il token di ogni modifica chiama solo callback associato in risposta a una singola modifica. Per abilitare il monitoraggio costante, è possibile utilizzare un `TaskCompletionSource` come illustrato di seguito, oppure ricreare `IChangeToken` istanze in risposta alle modifiche.

Nell'esempio di questo articolo, un'applicazione console è configurata per visualizzare un messaggio ogni volta che viene modificato un file di testo:

[!code-csharp[Main](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Il risultato dopo il salvataggio del file più volte:

![Finestra di comando dopo l'esecuzione dotnet eseguire Mostra il monitoraggio del file quotes.txt per le modifiche dell'applicazione e che il file è stato modificato cinque volte.](file-providers/_static/watch-console.png)

> [!NOTE]
> Alcuni file System, ad esempio i contenitori di Docker e condivisioni di rete, non può inviare in modo affidabile le notifiche di modifica. Impostare il `DOTNET_USE_POLLINGFILEWATCHER` variabile di ambiente `1` o `true` per eseguire il polling del file system per modifiche ogni 4 secondi.

## <a name="globbing-patterns"></a>Modelli il glob

Percorsi del file system utilizzano i modelli jolly chiamati *modelli il glob*. Questi modelli semplici possono essere utilizzati per specificare i gruppi di file. I due caratteri jolly sono `*` e `**`.

**`*`**

   Corrisponde a qualsiasi livello di cartella corrente, o qualsiasi nome di file o qualsiasi estensione di file. Corrispondenze vengono terminate dal `/` e `.` caratteri nel percorso del file.

<strong><code>**</code></strong>

   Corrisponde a qualsiasi elemento in più livelli di directory. Può essere utilizzato in modo ricorsivo corrispondono molti file all'interno di una gerarchia di directory.

### <a name="globbing-pattern-examples"></a>Esempi di criteri il glob

**`directory/file.txt`**

   Consente di ricercare un file specifico in una directory specifica.

**<code>directory/*.txt</code>**

   Corrisponde a tutti i file con `.txt` estensione in una directory specifica.

**`directory/*/bower.json`**

   Corrisponde a tutti `bower.json` file nella directory esattamente un livello sotto il `directory` directory.

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   Corrisponde a tutti i file con `.txt` trovata in un punto qualsiasi in un'estensione di `directory` directory.

## <a name="file-provider-usage-in-aspnet-core"></a>File di utilizzo del Provider in ASP.NET Core

Parti diverse di ASP.NET Core utilizzano provider di file. `IHostingEnvironment`espone l'applicazione radice del contenuto e nella radice web come `IFileProvider` tipi. Il middleware di file statici Usa provider di file per individuare i file statici. Razor consente un uso massiccio di `IFileProvider` viste di individuazione. Dotnet pubblicare i provider di file Usa funzionalità e i pattern il glob per specificare i file che devono essere pubblicati.

## <a name="recommendations-for-use-in-apps"></a>Consigli per l'uso nelle App

Se l'applicazione ASP.NET di base richiede l'accesso al file system, è possibile richiedere un'istanza di `IFileProvider` tramite l'inserimento di dipendenze, quindi utilizzare i metodi per eseguire l'accesso, come illustrato in questo esempio. Ciò consente di configurare il provider di una volta, all'avvio, l'app e riduce il numero di tipi di implementazione che crea un'istanza dell'app.
