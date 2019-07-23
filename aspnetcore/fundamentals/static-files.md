---
title: File statici in ASP.NET Core
author: rick-anderson
description: Informazioni sull'uso, sulla protezione di file statici e sulla configurazione dei comportamenti del middleware che ospita i file statici nell'app Web ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/8/2019
uid: fundamentals/static-files
ms.openlocfilehash: 1c665d1206e984fe41e9f57bb5356839c354dde2
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308191"
---
# <a name="static-files-in-aspnet-core"></a>File statici in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Steve Smith](https://twitter.com/Scott_Addie)

I file statici, ad esempio HTML, CSS, immagini e JavaScript, sono asset che un'app ASP.NET Core rende direttamente disponibili nei client. Per abilitare l'uso di questi file, sono necessarie alcune configurazioni.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="serve-static-files"></a>Usare i file statici

I file statici vengono archiviati nella directory radice Web del progetto. La directory predefinita è *\<radice_contenuto > / wwwroot*, ma può essere modificata tramite il metodo [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_). Vedere [Radice del contenuto](xref:fundamentals/index#content-root) e [Radice Web](xref:fundamentals/index#web-root) per altre informazioni.

L'host Web dell'app deve conoscere la directory radice del contenuto.

::: moniker range=">= aspnetcore-2.0"

Il metodo `WebHost.CreateDefaultBuilder` imposta la radice del contenuto nella directory corrente:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Impostare la radice del contenuto nella directory corrente richiamando [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) all'interno di `Program.Main`:

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

::: moniker-end

I file statici sono accessibili tramite un percorso relativo alla radice Web. Ad esempio, il modello di progetto **Applicazione Web** contiene varie cartelle all'interno della cartella *wwwroot*:

* **wwwroot**
  * **css**
  * **images**
  * **js**

Il formato dell'URI per accedere a un file nella sottocartella *images* (immagini) è *http://\<indirizzo_server > /images/\<nome_file_immagine >* . Ad esempio: *http://localhost:9189/images/banner3.svg* .

::: moniker range=">= aspnetcore-2.1"

Se la destinazione è .NET Framework, aggiungere il pacchetto [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) al progetto. Se la destinazione è .NET Core, questo pacchetto è incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Se la destinazione è .NET Framework, aggiungere il pacchetto [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) al progetto. Se la destinazione è .NET Core, questo pacchetto è incluso in [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Aggiungere il pacchetto [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) al progetto.

::: moniker-end

Configurare il [middleware](xref:fundamentals/middleware/index) che consente l'uso di file statici.

### <a name="serve-files-inside-of-web-root"></a>Usare i file all'interno della radice Web

Richiamare il metodo [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) all'interno di `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

L'overload del metodo `UseStaticFiles` senza parametri contrassegna i file nella radice Web come utilizzabili. Il markup seguente si riferisce a *wwwroot/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

Nel codice precedente, il carattere tilde `~/` indica una radice Web. Per altre informazioni, vedere [Web root](xref:fundamentals/index#web-root) (Radice Web).

### <a name="serve-files-outside-of-web-root"></a>Usare i file all'esterno della radice Web

Considerare una gerarchia di directory in cui si trovano i file statici da usare all'esterno della radice Web:

* **wwwroot**
  * **css**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
    * *banner1.svg*

Una richiesta può accedere al file *banner1.svg* configurando il middleware dei file statici come indicato di seguito:

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

Nel codice precedente la gerarchia di directory *MyStaticFiles* viene esposta pubblicamente tramite il segmento dell'URI *StaticFiles*. Una richiesta a *http://\<indirizzo_server > /StaticFiles/images/banner1.svg* usa il file *banner1.svg*.

Il markup seguente si riferisce a *MyStaticFiles/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a>Impostare le intestazioni della risposta HTTP

Per impostare le intestazioni della risposta HTTP, è possibile usare un oggetto [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions). Oltre a configurare l'uso del file statico della radice Web, il codice seguente imposta l'intestazione `Cache-Control`:

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

Il metodo [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) è disponibile nel pacchetto [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/).

I file sono stati resi pubblicamente inseribili nella cache per 10 minuti (600 secondi) nell'ambiente di sviluppo:

![Sono state aggiunte le intestazioni della risposta che visualizzano l'intestazione Cache-Control](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>Autorizzazione dei file statici

Il middleware dei file statici non offre controlli di autorizzazione. I file usati dal middleware, inclusi i file in *wwwroot*, sono disponibili pubblicamente. Per usare i file in base alle autorizzazioni:

* Archiviare i file all'esterno di *wwwroot* e di qualsiasi directory alla quale può accedere il middleware dei file statici.
* Usarli tramite un metodo di azione al quale è applicata l'autorizzazione. Restituire un oggetto [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult):

  [!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a>Abilitare l'esplorazione directory

L'esplorazione directory consente agli utenti dell'app Web di visualizzare un elenco di directory e i file all'interno di una directory specifica. Per motivi di sicurezza, l'esplorazione directory è disabilitata per impostazione predefinita (vedere [Considerazioni](#considerations)). Abilitare l'esplorazione directory richiamando il metodo [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) in `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

Aggiungere i servizi necessari richiamando il metodo [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) da `Startup.ConfigureServices`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

Il codice precedente consente l'esplorazione directory della cartella *wwwroot/images* tramite l'URL *http://\<indirizzo_server >/MyImages*, con collegamenti ai singoli file e cartelle:

![esplorazione directory](static-files/_static/dir-browse.png)

Vedere la sezione [Considerazioni](#considerations) per conoscere i rischi per la sicurezza quando l'esplorazione viene abilitata.

Si notino le due chiamate `UseStaticFiles` nell'esempio seguente. La prima chiamata consente l'uso di file statici nella cartella *wwwroot*. La seconda chiamata consente l'esplorazione directory nella cartella *wwwroot/images* tramite l'URL *http://\<indirizzo_server >/MyImages*:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a>Usare un documento predefinito

L'impostazione di una home page predefinita offre ai visitatori un punto di partenza logico per la visita del sito. Per usare una pagina predefinita senza che l'utente debba specificare in modo completo l'URI, chiamare il metodo [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) da `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> `UseDefaultFiles` deve essere chiamato prima di `UseStaticFiles` per usare il file predefinito. `UseDefaultFiles` è un URL rewriter che effettivamente non usa il file. Abilitare il middleware dei file statici tramite `UseStaticFiles` per usare il file.

Con `UseDefaultFiles`, si richiede la ricerca nella cartella degli elementi seguenti:

* *default.htm*
* *default.html*
* *index.htm*
* *index.html*

Il primo file trovato nell'elenco viene usato come se la richiesta fosse l'URI completo. L'URL del browser continua a riflettere l'URI richiesto.

Il codice seguente modifica il nome file predefinito in *mydefault.html*:

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a>UseFileServer

[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combina la funzionalità di `UseStaticFiles`, `UseDefaultFiles` e `UseDirectoryBrowser`.

Il codice seguente consente di usare i file statici e il file predefinito. L'esplorazione directory non è abilitata.

```csharp
app.UseFileServer();
```

Il codice seguente si basa sull'overload senza parametri, consentendo l'esplorazione directory:

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

Considerare la gerarchia di directory seguente:

* **wwwroot**
  * **css**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
    * *banner1.svg*
  * *default.html*

Il codice seguente abilita i file statici, i file predefiniti e l'esplorazione directory di `MyStaticFiles`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

Chiamare il metodo `AddDirectoryBrowser` quando il valore della proprietà `EnableDirectoryBrowsing` è `true`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

Dall'uso della gerarchia di file e del codice precedente ne deriva quanto segue:

| URI            |                             Risposta  |
| ------- | ------|
| *http://\<indirizzo_server>/StaticFiles/images/banner1.svg*    |      MyStaticFiles/images/banner1.svg |
| *http://\<indirizzo_server>/StaticFiles*             |     MyStaticFiles/default.html |

Se non esiste un file predefinito nella directory *MyStaticFiles*, *http://\<indirizzo_server > / StaticFiles* restituisce l'elenco di directory con i collegamenti su cui è possibile fare clic:

![Elenco di file statici](static-files/_static/db2.png)

> [!NOTE]
> <xref:Microsoft.AspNetCore.Builder.DefaultFilesExtensions.UseDefaultFiles*> e <xref:Microsoft.AspNetCore.Builder.DirectoryBrowserExtensions.UseDirectoryBrowser*> eseguono un reindirizzamento sul lato client da `http://{SERVER ADDRESS}/StaticFiles` (senza barra rovesciata) a `http://{SERVER ADDRESS}/StaticFiles/` (con barra rovesciata). Gli URL relativi all'interno della directory *StaticFiles* non sono validi senza barra finale.

## <a name="fileextensioncontenttypeprovider"></a>Classe FileExtensionContentTypeProvider

La classe [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) contiene una proprietà `Mappings` che può essere usata come mapping di estensioni di file nei tipi di contenuto MIME. Nell'esempio seguente varie estensioni di file vengono registrate in tipi MIME noti. L'estensione *rtf* viene sostituita e l'estensione *mp4* viene rimossa.

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

Vedere [Tipi di contenuto MIME](https://www.iana.org/assignments/media-types/media-types.xhtml).

## <a name="non-standard-content-types"></a>Tipi di contenuto non standard

Il middleware dei file statici riconosce almeno 400 tipi di contenuto file noti. Se viene richiesto un file con un tipo di file non noto, il middleware dei file statici passa la richiesta al middleware successivo nella pipeline. Se nessun middleware gestisce la richiesta, viene restituita la risposta *404 Non trovato*. Se è abilitata l'esplorazione directory, viene visualizzato un collegamento al file nella visualizzazione directory.

Il codice seguente consente di usare tipi sconosciuti ed esegue il rendering del file sconosciuto come immagine:

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

Con il codice precedente, una richiesta per un file con un tipo di contenuto sconosciuto viene restituita come immagine.

> [!WARNING]
> L'abilitazione di [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) costituisce un rischio per la sicurezza. È disabilitato per impostazione predefinita e ne è sconsigliato l'uso. La classe [FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) offre un'alternativa più sicura per usare i file con estensioni non standard.

### <a name="considerations"></a>Considerazioni

> [!WARNING]
> L'uso di `UseDirectoryBrowser` e `UseStaticFiles` può comportare la perdita di informazioni riservate. È consigliabile disabilitare l'esplorazione directory nell'ambiente di produzione. Controllare attentamente quali sono le directory abilitate tramite `UseStaticFiles` o `UseDirectoryBrowser`. L'intera directory e le relative sottodirectory diventano pubblicamente accessibili. Archiviare i file pubblicamente utilizzabili in una directory dedicata, ad esempio  *\<radice_contenuto > / wwwroot*. Tenere questi file separati da visualizzazioni MVC, Razor Pages (solo versione 2.x), file di configurazione e così via.

* Gli URL per il contenuto esposto con `UseDirectoryBrowser` e `UseStaticFiles` sono soggetti a distinzione tra maiuscole e minuscole e a limitazione di caratteri del file system sottostante. Ad esempio, in Windows viene fatta distinzione tra maiuscole e minuscole, al contrario di quanto avviene in macOS e Linux.

* Le app ASP.NET Core ospitate in IIS usano il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) per inoltrare tutte le richieste all'app, incluse le richieste di file statici. Non viene usato il gestore di file statici di IIS. Non è possibile gestire le richieste se queste non sono state prima gestite dal modulo.

* Completare la procedura seguente in Gestione IIS per rimuovere il gestore di file statici di IIS a livello di server o di sito Web:
    1. Passare alla funzionalità **Moduli**.
    1. Selezionare **StaticFileModule** nell'elenco.
    1. Fare clic su **Rimuovi** nell'intestazione laterale **Azioni**.

> [!WARNING]
> Se il gestore di file statici di IIS è abilitato **e** il modulo ASP.NET Core non è configurato correttamente, vengono usati i file statici. Tale scenario si verifica ad esempio se il file *web.config* non è stato distribuito.

* Inserire i file di codice (inclusi i file con estensione *cs* e *cshtml*) all'esterno della radice Web del progetto dell'app. Si crea quindi un separazione logica tra il contenuto sul lato client dell'app e il codice basato su server. In questo modo si impedisce la perdita del codice sul lato server.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Middleware](xref:fundamentals/middleware/index)
* [Introduzione a ASP.NET Core](xref:index)
