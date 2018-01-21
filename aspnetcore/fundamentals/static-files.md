---
title: Utilizzare i file statici in ASP.NET Core
author: rick-anderson
description: Informazioni su come gestire e proteggere i file statici e per configurare file statico che ospita i comportamenti di middleware in un'applicazione web ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: 60b205bf0a45e2965f9dab46f46956947ae513fd
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="work-with-static-files-in-aspnet-core"></a>Utilizzare i file statici in ASP.NET Core

Da [Rick Anderson](https://twitter.com/RickAndMSFT) e [Scott Addie](https://twitter.com/Scott_Addie)

File statici, ad esempio HTML, CSS, immagini e JavaScript, sono attività che viene utilizzata un'applicazione ASP.NET Core direttamente al client. Alcune attività di configurazione è necessario attivare la gestione di questi file.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="serve-static-files"></a>Gestire file statici

File statici vengono archiviati in directory di radice del progetto web. La directory predefinita è  *\<content_root > / wwwroot*, ma può essere modificata tramite il [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) metodo. Vedere [contenuto radice](xref:fundamentals/index#content-root) e [radice Web](xref:fundamentals/index#web-root) per ulteriori informazioni.

Dell'host dell'applicazione web deve essere a conoscenza della directory radice del contenuto.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Il `WebHost.CreateDefaultBuilder` metodo imposta la radice del contenuto nella directory corrente:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Imposta la radice del contenuto nella directory corrente richiamando [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) all'interno di `Program.Main`:

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

File statici sono accessibili tramite un percorso relativo alla radice web. Ad esempio, il **applicazione Web** modello di progetto contiene diverse cartelle all'interno di *wwwroot* cartella:

* **wwwroot**
  * **css**
  * **immagini**
  * **js**

Il formato dell'URI per accedere a un file di *immagini* sottocartella *http://\<server_address > /images/\<Nome_file_immagine >*. Ad esempio, *http://localhost:9189/images/banner3.svg*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Se la destinazione è .NET Framework, aggiungere il [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) pacchetto al progetto. Se la destinazione di .NET Core, il [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) include il pacchetto.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Aggiungere il [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) pacchetto al progetto.

---

Configurare il [middleware](xref:fundamentals/middleware) che consente la gestione dei file statici.

### <a name="serve-files-inside-of-web-root"></a>File radice web all'interno di

Richiamare il [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) metodo all'interno di `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

Senza parametri `UseStaticFiles` overload del metodo contrassegna i file nella radice web come servable. I seguenti riferimenti markup *wwwroot/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a>Gestire file esterni radice web

Considerare una gerarchia di directory in cui si trovano i file statici servito all'esterno nella radice web:

* **wwwroot**
  * **css**
  * **immagini**
  * **js**
* **MyStaticFiles**
  * **immagini**
      * *banner1.svg*

Accessibile a una richiesta di *banner1.svg* file configurando il middleware di file statici come indicato di seguito:

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

Nel codice precedente, il *MyStaticFiles* gerarchia di directory viene esposto pubblicamente tramite la *StaticFiles* segmento URI. Una richiesta di *http://\<server_address > /StaticFiles/images/banner1.svg* serve il *banner1.svg* file.

I seguenti riferimenti markup *MyStaticFiles/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a>Impostare le intestazioni di risposta HTTP

Oggetto [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) oggetto può essere utilizzato per impostare le intestazioni di risposta HTTP. Oltre a configurare la gestione di file statici dalla radice web, il codice seguente imposta il `Cache-Control` intestazione:

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

Il [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) metodo è presente il [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) pacchetto.

I file sono stati apportati pubblicamente memorizzabile nella cache per 10 minuti (600 secondi):

![Le intestazioni di risposta che mostra l'intestazione Cache-Control è stata aggiunta](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>Autorizzazione del file statico

Il middleware di file statici non fornisce controlli di autorizzazione. Qualsiasi file forniti, tra cui quelle contenute *wwwroot*, sono accessibili pubblicamente. Per gestire i file in base alle autorizzazioni:

* Archiviarli all'esterno di *wwwroot* e qualsiasi directory accessibile per il middleware di file statici **e**
* Esse forniscono tramite un metodo di azione a cui viene applicata l'autorizzazione. Restituire un [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) oggetto:

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a>Abilitare l'esplorazione directory

Esplorazione directory consente agli utenti dell'app web visualizzare un elenco di directory e i file all'interno di una directory specificata. Esplorazione directory è disabilitata per impostazione predefinita per motivi di sicurezza (vedere [considerazioni](#considerations)). Directory Attivazione esplorazione richiamando il [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) metodo `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

Aggiungere i servizi necessari richiamando il [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) metodo `Startup.ConfigureServices`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

Il codice precedente consente l'esplorazione delle directory per il *wwwroot/immagini* cartella utilizzando l'URL *http://\<server_address > / MyImages*, con collegamenti ai singoli file e cartelle:

![esplorazione directory](static-files/_static/dir-browse.png)

Vedere [considerazioni](#considerations) sui rischi di sicurezza quando si abilita l'esplorazione.

Tenere presente le due `UseStaticFiles` chiama nell'esempio seguente. La prima chiamata consente la gestione di file statici di *wwwroot* cartella. La seconda chiamata consente l'esplorazione delle directory per il *wwwroot/immagini* cartella utilizzando l'URL *http://\<server_address > / MyImages*:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a>Serva un documento predefinito

L'impostazione di una home page predefinita fornisce visitatori un punto di partenza logico quando si visita il sito. Per una pagina predefinita senza che l'utente in modo completo l'URI, chiamare il [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) metodo `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> `UseDefaultFiles`deve essere chiamato prima `UseStaticFiles` per servire il file predefinito. `UseDefaultFiles`è un rewriter URL che non viene effettivamente servire il file. Abilitare il middleware di file statici tramite `UseStaticFiles` per servire il file.

Con `UseDefaultFiles`, le richieste a una ricerca nella cartella per:

* *default.htm*
* *default.html*
* *index.htm*
* *index.html*

Il primo file trovato nell'elenco è disponibile come se la richiesta fosse l'URI completo. URL del browser continua in modo da riflettere l'URI richiesto.

Il codice seguente modifica il nome file predefinito per *mydefault.html*:

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a>UseFileServer

[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combina la funzionalità di `UseStaticFiles`, `UseDefaultFiles`, e `UseDirectoryBrowser`.

Il codice seguente consente la gestione di file statici e il file predefinito. Non è abilitata l'esplorazione directory.

```csharp
app.UseFileServer();
```

Il codice seguente si basa sull'overload senza parametri, consentendo l'esplorazione directory:

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

Si consideri la seguente gerarchia di directory:

* **wwwroot**
  * **css**
  * **immagini**
  * **js**
* **MyStaticFiles**
  * **immagini**
      * *banner1.svg*
  * *default.html*

Il codice seguente permette di file statici, i file predefiniti e esplorazione delle directory `MyStaticFiles`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

`AddDirectoryBrowser`deve essere chiamato quando il `EnableDirectoryBrowsing` valore della proprietà è `true`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

URL di codice usando la gerarchia di file e precedenti, risolvere come indicato di seguito:

| URI            |                             Risposta  |
| ------- | ------|
| *http://\<server_address>/StaticFiles/images/banner1.svg*    |      MyStaticFiles/images/banner1.svg |
| *http://\<server_address>/StaticFiles*             |     MyStaticFiles/default.html |

Se non esiste alcun file denominato predefinito nel *MyStaticFiles* directory *http://\<server_address > / StaticFiles* restituisce la visualizzazione con collegamenti selezionabili directory:

![Elenco di file statici](static-files/_static/db2.png)

> [!NOTE]
> `UseDefaultFiles`e `UseDirectoryBrowser` utilizzare l'URL *http://\<server_address > / StaticFiles* senza la barra finale per attivare un lato client si viene reindirizzati *http://\<server_address > / StaticFiles /*. Si noti l'aggiunta di una barra finale. URL relativo all'interno dei documenti sono considerati non validi senza una barra finale.

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

Il [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) classe contiene un `Mappings` proprietà che funge da un mapping di estensioni di file per i tipi di contenuto MIME. Nell'esempio seguente, diverse estensioni di file vengono registrate i tipi MIME noti. Il *RTF* estensione viene sostituita, e *MP4* viene rimosso.

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

Vedere [tipi di contenuto MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).

## <a name="non-standard-content-types"></a>Tipi di contenuto non standard

Il middleware di file statici riconosce i tipi di contenuto di file conosciuti quasi 400. Se l'utente richiede un file di un tipo di file sconosciuto, il middleware di file statici restituisce una risposta HTTP 404 (non trovato). Se è abilitata l'esplorazione directory, viene visualizzato un collegamento al file. L'URI restituisce un errore HTTP 404.

Il codice seguente consente serve tipi sconosciuti ed esegue il rendering di file sconosciute come un'immagine:

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

Con il codice precedente, una richiesta per un file con un tipo di contenuto sconosciuto viene restituita come un'immagine.

> [!WARNING]
> Abilitazione [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) costituisce un rischio di sicurezza. È disabilitato per impostazione predefinita e il relativo uso è sconsigliato. [FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) offre un'alternativa più sicura per servire i file con estensione non standard.

### <a name="considerations"></a>Considerazioni

> [!WARNING]
> `UseDirectoryBrowser`e `UseStaticFiles` possono verificarsi perdite di informazioni riservate. È consigliabile disabilitare directory esplorazione nell'ambiente di produzione. Leggere attentamente le directory vengono abilitate tramite `UseStaticFiles` o `UseDirectoryBrowser`. L'intera directory e relative sottodirectory diventano accessibili pubblicamente. Archivio file adatto per essere distribuito al pubblico in una directory dedicata, ad esempio  *\<content_root > / wwwroot*. Questi file devono essere distinti dalle visualizzazioni MVC, pagine Razor (2. x solo), i file di configurazione e così via.

* Gli URL per il contenuto esposti con `UseDirectoryBrowser` e `UseStaticFiles` sono soggetti a distinzione maiuscole/minuscole e restrizioni relative ai caratteri del file system sottostante. Ad esempio, Windows viene fatta distinzione tra maiuscole e minuscole&mdash;Mac e Linux non sono.

* Le applicazioni ASP.NET Core ospitate in IIS utilizza il [ASP.NET Core modulo (ANCM)](xref:fundamentals/servers/aspnet-core-module) per inoltrare tutte le richieste per l'app, incluse le richieste di file statici. Il gestore di file statici di IIS non verrà usato. Non dispone di alcuna possibilità di gestire le richieste prima di essi è gestiti dal ANCM.

* Completare i passaggi seguenti in Gestione IIS per rimuovere il gestore di file statici di IIS a livello di server o un sito Web:
    1. Passare il **moduli** funzionalità.
    1. Selezionare **StaticFileModule** nell'elenco.
    1. Fare clic su **rimuovere** nel **azioni** barra laterale.

> [!WARNING]
> Se il gestore di file statici di IIS è abilitato **e** il ANCM non è configurato correttamente, i file statici vengono forniti. Ciò si verifica, ad esempio, se il *Web. config* file non è stato distribuito.

* Inserire i file di codice (inclusi *cs* e *. cshtml*) di fuori della app radice del progetto web. Una separazione logica viene pertanto creata tra il contenuto sul lato client dell'app e il codice basato su server. Ciò impedisce che il codice lato server perdita.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Middleware](xref:fundamentals/middleware)

* [Introduzione a ASP.NET Core](xref:index)
