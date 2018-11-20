---
title: Provider di file in ASP.NET Core
author: guardrex
description: Informazioni su come ASP.NET Core astrae l'accesso al file system tramite l'utilizzo di provider di file.
ms.author: riande
ms.custom: mvc
ms.date: 08/01/2018
uid: fundamentals/file-providers
ms.openlocfilehash: 5d0d46ba82cd84e48e5a9b23d6d330d8888beb41
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2018
ms.locfileid: "51570100"
---
# <a name="file-providers-in-aspnet-core"></a>Provider di file in ASP.NET Core

Di [Steve Smith](https://ardalis.com/) e [Luke Latham](https://github.com/guardrex)

ASP.NET Core astrae l'accesso al file system tramite l'utilizzo di provider di file. I provider di file vengono usati in tutto il framework di ASP.NET Core:

* [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) espone la radice del contenuto dell'app e la radice Web come tipi `IFileProvider`.
* Il [middleware dei file statici](xref:fundamentals/static-files) usa i provider di file per trovare i file statici.
* [Razor](xref:mvc/views/razor) usa i provider di file per individuare pagine e viste.
* Gli strumenti .NET Core usano i provider di file e i criteri GLOB per specificare i file da pubblicare.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="file-provider-interfaces"></a>Interfacce del provider di file

L'interfaccia primaria è [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider). `IFileProvider` espone metodi per:

* Ottenere informazioni sui file ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).
* Ottenere informazioni sulla directory ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).
* Configurare le notifiche di modifica (usando un [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).

`IFileInfo` fornisce metodi e proprietà per l'uso di file:

* [Exists](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.exists)
* [IsDirectory](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.isdirectory)
* [Name](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.name)
* [Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (in byte)
* Data [LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified)

È possibile leggere dal file usando il metodo [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream).

L'app di esempio dimostra come configurare un provider di file in `Startup.ConfigureServices` da usare in tutta l'app tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection).

## <a name="file-provider-implementations"></a>Implementazioni dei provider di file

Sono disponibili tre implementazioni di `IFileProvider`.

::: moniker range=">= aspnetcore-2.0"

| Implementazione | Descrizione |
| -------------- | ----------- |
| [PhysicalFileProvider](#physicalfileprovider) | Il provider fisico viene usato per accedere ai file fisici del sistema. |
| [ManifestEmbeddedFileProvider](#manifestembeddedfileprovider) | Il provider incorporato nel manifesto viene usato per accedere ai file incorporati negli assembly. |
| [CompositeFileProvider](#compositefileprovider) | Il provider composito consente di offrire un accesso combinato a file e directory da uno o più provider. |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| Implementazione | Descrizione |
| -------------- | ----------- |
| [PhysicalFileProvider](#physicalfileprovider) | Il provider fisico viene usato per accedere ai file fisici del sistema. |
| [EmbeddedFileProvider](#embeddedfileprovider) | Il provider incorporato consente di accedere ai file incorporati negli assembly. |
| [CompositeFileProvider](#compositefileprovider) | Il provider composito consente di offrire un accesso combinato a file e directory da uno o più provider. |

::: moniker-end

### <a name="physicalfileprovider"></a>PhysicalFileProvider

[PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) consente di accedere al file system fisico. `PhysicalFileProvider` usa il tipo [System.IO.File](/dotnet/api/system.io.file) (per il provider fisico) e definisce una directory e i relativi elementi figlio come ambito per tutti i percorsi. La definizione dell'ambito impedisce l'accesso al file system al di fuori della directory specificata e dei relativi elementi figlio. Per la creazione di un'istanza di questo provider, è richiesto un percorso di directory che viene usato come percorso di base per tutte le richieste effettuate tramite il provider. È possibile creare direttamente un'istanza di un provider `PhysicalFileProvider` oppure richiedere un `IFileProvider` in un costruttore tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection).

**Tipi statici**

Il codice seguente illustra come creare un `PhysicalFileProvider` e usarlo per ottenere il contenuto della directory e informazioni sui file:

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

Tipi nell'esempio precedente:

* `provider` è `IFileProvider`.
* `contents` è `IDirectoryContents`.
* `fileInfo` è `IFileInfo`.

Il provider di file può essere usato per scorrere la directory specificata da `applicationRoot` oppure per chiamare `GetFileInfo` per ottenere informazioni su un file. Il provider di file non ha accesso all'esterno della directory `applicationRoot`.

L'app di esempio crea il provider nella classe `Startup.ConfigureServices` dell'app usando [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

**Ottenere i tipi di provider di file con inserimento delle dipendenze**

Inserire il provider in un costruttore di classe e assegnarlo a un campo locale. Usare il campo in tutti i metodi della classe per accedere ai file.

::: moniker range=">= aspnetcore-2.0"

Nell'app di esempio, la classe `IndexModel` riceve un'istanza di `IFileProvider` per ottenere il contenuto della directory per il percorso di base dell'app.

*Pages/Index.cshtml.cs*:

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml.cs?name=snippet1)]

Viene eseguita l'iterazione di `IDirectoryContents` nella pagina.

*Pages/Index.cshtml*:

[!code-cshtml[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Nell'app di esempio, la classe `HomeController` riceve un'istanza di `IFileProvider` per ottenere il contenuto della directory per il percorso di base dell'app.

*Controllers/HomeController.cs*:

[!code-csharp[](file-providers/samples/1.x/FileProviderSample/Controllers/HomeController.cs?name=snippet1)]

Viene eseguita l'iterazione di `IDirectoryContents` nella vista.

*Views/Home/Index.cshtml*:

[!code-cshtml[](file-providers/samples/1.x/FileProviderSample/Views/Home/Index.cshtml?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="manifestembeddedfileprovider"></a>ManifestEmbeddedFileProvider

[ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) viene usato per accedere ai file incorporati negli assembly. `ManifestEmbeddedFileProvider` usa un manifesto compilato nell'assembly per ricostruire i percorsi originali dei file incorporati.

> [!NOTE]
> `ManifestEmbeddedFileProvider` è disponibile in ASP.NET Core 2.1 o versioni successive. Per accedere ai file incorporati negli assembly di ASP.NET Core 2.0 o versioni precedenti, vedere la [versione per ASP.NET Core 1.x di questo argomento](/aspnet/core/fundamentals/file-providers?view=aspnetcore-1.1).

Per generare un manifesto dei file incorporati, impostare la proprietà `<GenerateEmbeddedFilesManifest>` su `true`. Specificare i file da incorporare con [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

Usare [modelli GLOB](#glob-patterns) per specificare uno o più file da incorporare nell'assembly.

L'app di esempio crea un `ManifestEmbeddedFileProvider` e passa l'assembly attualmente in esecuzione al rispettivo costruttore.

*Startup.cs*:

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

Overload aggiuntivi consentono di:

* Specificare un percorso di file relativo.
* Definire la data dell'ultima modifica come ambito dei file.
* Assegnare un nome alla risorsa incorporata contenente il manifesto dei file incorporati.

| Overload | Descrizione |
| -------- | ----------- |
| [ManifestEmbeddedFileProvider(Assembly, String)](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_) | Accetta un parametro di percorso relativo `root` facoltativo. Specificare `root` per definire come ambito delle chiamate a [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) le risorse incluse nel percorso specificato. |
| [ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_DateTimeOffset_) | Accetta un parametro di percorso relativo `root` facoltativo e un parametro di data `lastModified` ([DateTimeOffset](/dotnet/api/system.datetimeoffset)). La data `lastModified` definisce la data dell'ultima modifica come ambito per le istanze [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) restituite da [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider). |
| [ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_String_System_DateTimeOffset_) | Accetta un parametro di percorso relativo `root` facoltativo, un parametro di data `lastModified` e il parametro `manifestName`. `manifestName` rappresenta il nome della risorsa incorporata contenente il manifesto. |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

### <a name="embeddedfileprovider"></a>EmbeddedFileProvider

[EmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider) viene usato per accedere ai file incorporati negli assembly. Specificare i file da incorporare con la proprietà [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) nel file di progetto:

```xml
<ItemGroup>
  <EmbeddedResource Include="Resource.txt" />
</ItemGroup>
```

Usare [modelli GLOB](#glob-patterns) per specificare uno o più file da incorporare nell'assembly.

L'app di esempio crea un `EmbeddedFileProvider` e passa l'assembly attualmente in esecuzione al rispettivo costruttore.

*Startup.cs*:

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

Le risorse incorporate non espongono le directory. Il percorso della risorsa (tramite il relativo spazio dei nomi) viene invece incorporato nel nome file usando i separatori `.`. Nell'app di esempio, `baseNamespace` è `FileProviderSample.`.

Il costruttore [EmbeddedFileProvider(Assembly, String)](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_EmbeddedFileProvider__ctor_System_Reflection_Assembly_) accetta un parametro `baseNamespace` facoltativo. Specificare lo spazio dei nomi di base per definire come ambito delle chiamate a [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) le risorse incluse nello spazio dei nomi specificato.

::: moniker-end

### <a name="compositefileprovider"></a>CompositeFileProvider

[CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) combina le istanze di `IFileProvider` esponendo una singola interfaccia per l'utilizzo dei file da più provider. Quando si crea `CompositeFileProvider`, passare una o più istanze `IFileProvider` al relativo costruttore.

::: moniker range=">= aspnetcore-2.0"

Nell'app di esempio, un `PhysicalFileProvider` e un `ManifestEmbeddedFileProvider` forniscono i file a un `CompositeFileProvider` registrato nel contenitore dei servizi dell'app:

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Nell'app di esempio, un `PhysicalFileProvider` e un `EmbeddedFileProvider` forniscono i file a un `CompositeFileProvider` registrato nel contenitore dei servizi dell'app:

[!code-csharp[](file-providers/samples/1.x/FileProviderSample/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="watch-for-changes"></a>Controllo delle modifiche

Il metodo [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) consente di controllare uno o più file o directory per il rilevamento delle modifiche. `Watch` accetta una stringa di percorso in cui è possibile usare [criteri GLOB](#glob-patterns) per specificare più file. `Watch` restituisce un [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken). Il token di modifica espone:

* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): una proprietà che può essere ispezionata per determinare se è stata modificata.
* [RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): chiamato quando vengono rilevate modifiche alla stringa di percorso specificata. Ogni token di modifica chiama solo il relativo callback associato in risposta a una singola modifica. Per abilitare un monitoraggio continuo, usare [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) come illustrato di seguito oppure ricreare istanze di `IChangeToken` in risposta alle modifiche.

Nell'app di esempio, l'app console *WatchConsole* viene configurata per visualizzare un messaggio quando viene modificato un file di testo:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](file-providers/samples/1.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

::: moniker-end

È possibile che alcuni file system, ad esempio i contenitori Docker e le condivisioni di rete, non inviino sempre le notifiche di modifica. Impostare la variabile di ambiente `DOTNET_USE_POLLING_FILE_WATCHER` su `1` o `true` per eseguire il polling delle modifiche nel file system ogni quattro secondi (intervallo non configurabile).

## <a name="glob-patterns"></a>Modelli GLOB

Nei percorsi del file system vengono usati criteri con caratteri jolly chiamati *criteri GLOB (o globbing)*. Specificare gruppi di file con questi criteri. I due caratteri jolly sono `*` e `**`:

**`*`**  
Cerca qualsiasi elemento a livello della cartella corrente, qualsiasi nome di file o qualsiasi estensione di file. Le corrispondenze vengono terminate con i caratteri `/` e `.` nel percorso dei file.

**`**`**  
Cerca qualsiasi elemento in più livelli di directory. Può essere usato per cercare in modo ricorsivo numerosi file all'interno di una gerarchia di directory.

**Esempi di criteri GLOB**

**`directory/file.txt`**  
Cerca un file specifico in una directory specifica.

**`directory/*.txt`**  
Cerca tutti i file con estensione *txt* in una directory specifica.

**`directory/*/appsettings.json`**  
Cerca tutti i file `appsettings.json` nelle directory esattamente un livello sotto la cartella *directory*.

**`directory/**/*.txt`**  
Cerca tutti i file con estensione *txt* che si trovano in qualsiasi posizione nella cartella *directory*.
