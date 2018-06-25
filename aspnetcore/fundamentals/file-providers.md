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
# <a name="file-providers-in-aspnet-core"></a>Provider di file in ASP.NET Core

[Steve Smith](https://ardalis.com/)

ASP.NET Core astrae l'accesso al file system tramite l'utilizzo di provider di file.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="file-provider-abstractions"></a>Astrazioni dei provider di file

I provider di file sono astrazioni dei file system. L'interfaccia principale è `IFileProvider`. `IFileProvider` espone metodi per ottenere informazioni sui file (`IFileInfo`) e informazioni sulle directory (`IDirectoryContents`) e per impostare le notifiche di modifica (usando `IChangeToken`).

`IFileInfo` offre metodi e proprietà relative a singoli file o directory. Ha due proprietà booleane, `Exists` e `IsDirectory`, e proprietà che descrivono `Name`, `Length` (in byte) e data `LastModified` del file. È possibile leggere dal file usando il relativo metodo `CreateReadStream`.

## <a name="file-provider-implementations"></a>Implementazioni dei provider di file

Sono disponibili tre implementazioni di `IFileProvider`: fisica, incorporata e composita. Il provider fisico consente di accedere ai file del sistema. Il provider incorporato consente di accedere ai file incorporati negli assembly. Il provider composito consente di offrire un accesso combinato a file e directory da uno o più provider.

### <a name="physicalfileprovider"></a>PhysicalFileProvider

`PhysicalFileProvider` consente di accedere al file system fisico. Esegue il wrapping del tipo `System.IO.File` (per il provider fisico), definendo l'ambito di tutti i percorsi in una directory e nei relativi elementi figlio. La definizione dell'ambito limita l'accesso a una determinata directory e ai relativi elementi figlio impedendo l'accesso al file system che non rientra nel limite. Quando si crea un'istanza di questo provider, è necessario specificare il percorso di una directory che viene usato come percorso di base per tutte le richieste effettuate dal provider (e che impedisce l'accesso all'esterno del percorso). In un'app ASP.NET Core è possibile creare direttamente un'istanza di un provider `PhysicalFileProvider` oppure richiedere `IFileProvider` in un controller o un costruttore del servizio tramite l'[inserimento di dipendenze](dependency-injection.md). Il secondo approccio offre in genere una soluzione più flessibile e verificabile.

L'esempio seguente illustra come creare un provider `PhysicalFileProvider`.


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

È possibile scorrere i contenuti della directory oppure ottenere le informazioni su un file specifico specificando un sottopercorso.

Per richiedere un provider da un controller, specificarlo nel costruttore del controller e assegnarlo a un campo locale. Usare l'istanza locale dai metodi di azione:

[!code-csharp[](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

Creare quindi il provider nella classe `Startup` dell'app:

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

Nella visualizzazione *Index.cshtml* scorrere `IDirectoryContents`:

[!code-html[](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

Il risultato sarà:

![Applicazione di esempio di provider di file con elenco dei file fisici e delle cartelle](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a>EmbeddedFileProvider

`EmbeddedFileProvider` consente di accedere ai file incorporati negli assembly. In .NET Core i file vengono incorporati in un assembly con l'elemento `<EmbeddedResource>` nel file *.csproj*:

[!code-json[](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

Durante la specifica dei file da incorporare nell'assembly è possibile usare [criteri GLOB](#globbing-patterns). Questi criteri possono essere usati per cercare uno o più file.

> [!NOTE]
> È improbabile che si voglia incorporare ogni file con estensione js nel progetto nel relativo assembly; l'esempio precedente è solo a scopo dimostrativo.

Quando si crea un provider `EmbeddedFileProvider`, passare l'assembly che verrà letto al relativo costruttore.

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

Il frammento di codice precedente illustra come creare un provider `EmbeddedFileProvider` con accesso all'assembly attualmente in esecuzione.

L'aggiornamento dell'app di esempio per l'uso di un provider `EmbeddedFileProvider` produce l'output seguente:

![Applicazione di esempio di provider di file con elenco dei file incorporati](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> Le risorse incorporate non espongono le directory. Il percorso della risorsa (tramite il relativo spazio dei nomi) viene invece incorporato nel nome file usando i separatori `.`.

> [!TIP]
> Il costruttore `EmbeddedFileProvider` accetta un parametro `baseNamespace` facoltativo. Questa specifica determinerà la definizione di un ambito delle chiamate a `GetDirectoryContents` corrispondente alle risorse appartenenti allo spazio dei nomi specificato.

### <a name="compositefileprovider"></a>CompositeFileProvider

`CompositeFileProvider` combina le istanze `IFileProvider` esponendo una sola interfaccia per l'uso dei file di più provider. Quando si crea il provider `CompositeFileProvider`, si passano una o più istanze `IFileProvider` al relativo costruttore:

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

L'aggiornamento dell'app di esempio per l'uso di un provider `CompositeFileProvider` che include i provider fisici e i provider incorporati definiti in precedenza produce l'output seguente:

![Applicazione di esempio di provider di file con elenco dei file fisici, delle cartelle e dei file incorporati](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a>Controllo per la ricerca di modifiche

Il metodo `Watch` di `IFileProvider` consente di controllare uno o più file o directory per la ricerca di modifiche. Questo metodo accetta una stringa di percorso in cui è possibile usare [criteri GLOB](#globbing-patterns) per specificare più file e restituisce `IChangeToken`. Il token espone una proprietà `HasChanged` in cui è possibile eseguire il controllo e un metodo `RegisterChangeCallback` che viene chiamato quando vengono rilevate modifiche nella stringa di percorso specificata. Si noti che ogni token di modifica chiama solo il relativo callback associato in risposta a una singola modifica. Per abilitare un monitoraggio continuo, è possibile usare `TaskCompletionSource` come illustrato di seguito oppure ricreare istanze `IChangeToken` in risposta alle modifiche.

Nell'esempio di questo articolo viene configurata un'applicazione console in modo che visualizzi un messaggio quando viene modificato un file di testo:

[!code-csharp[](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Dopo aver salvato il file più volte, il risultato è il seguente:

![Finestra di comando dopo l'esecuzione di dotnet run che mostra il controllo del file quotes.txt da parte dell'applicazione e l'individuazione di cinque modifiche.](file-providers/_static/watch-console.png)

> [!NOTE]
> È possibile che alcuni file system, ad esempio i contenitori Docker e le condivisioni di rete, non inviino sempre le notifiche di modifica. Impostare la variabile di ambiente `DOTNET_USE_POLLINGFILEWATCHER` su `1` o `true` per cercare le modifiche nel file system ogni 4 secondi.

## <a name="globbing-patterns"></a>Criteri GLOB

Nei percorsi dei file vengono usati criteri di caratteri jolly chiamati *criteri GLOB*. Questi criteri semplici possono essere usati per specificare gruppi di file. I due caratteri jolly sono `*` e `**`.

**`*`**

   Ricerca qualsiasi elemento a livello della cartella corrente o qualsiasi nome file o estensione di file. Le corrispondenze vengono terminate con i caratteri `/` e `.` nel percorso dei file.

<strong><code>**</code></strong>

   Cerca qualsiasi elemento in più livelli di directory. Può essere usato per cercare in modo ricorsivo numerosi file all'interno di una gerarchia di directory.

### <a name="globbing-pattern-examples"></a>Esempi di criteri GLOB

**`directory/file.txt`**

   Cerca un file specifico in una directory specifica.

**<code>directory/*.txt</code>**

   Cerca tutti i file con estensione `.txt` in una directory specifica.

**`directory/*/bower.json`**

   Cerca tutti i file `bower.json` nelle directory esattamente un livello sotto la directory `directory`.

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   Cerca tutti i file con estensione `.txt` che si trovano in qualsiasi posizione nella directory `directory`.

## <a name="file-provider-usage-in-aspnet-core"></a>Utilizzo dei provider di file in ASP.NET Core

Diverse parti di ASP.NET Core utilizzano i provider di file. `IHostingEnvironment` espone la radice del contenuto dell'app e la radice Web come tipi `IFileProvider`. Il middleware dei file statici usa i provider di file per individuare i file statici. Razor usa di frequente `IFileProvider` nella ricerca delle visualizzazioni. La funzionalità di pubblicazione di Dotnet usa i provider di file e i criteri GLOB per specificare i file da pubblicare.

## <a name="recommendations-for-use-in-apps"></a>Suggerimenti per l'uso nelle app

Se l'app ASP.NET Core richiede l'accesso al file system, è possibile richiedere un'istanza di `IFileProvider` tramite l'inserimento di dipendenze e quindi usarne i metodi per eseguire l'accesso, come illustrato nell'esempio. In questo modo è possibile configurare il provider una sola volta, ovvero all'avvio dell'app, e ridurre il numero di tipi di implementazioni di cui l'app crea un'istanza.
