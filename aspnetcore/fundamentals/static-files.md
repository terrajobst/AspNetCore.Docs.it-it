---
title: Utilizzo di file statici di ASP.NET Core
author: rick-anderson
description: Utilizzo di file statici
keywords: ASP.NET Core, file statici, asset statico, HTML, CSS, JavaScript
ms.author: riande
manager: wpickett
ms.date: 4/07/2017
ms.topic: article
ms.assetid: e32245c7-4eee-4831-bd2e-915dbf9f5f70
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/static-files
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 11457cb8684e98147447303ae4653b74414a11fb
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-working-with-static-files-in-aspnet-core"></a>Introduzione all'utilizzo dei file statici di ASP.NET Core

<a name=fundamentals-static-files></a>

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

File statici, ad esempio HTML, CSS, immagini e JavaScript, sono attività che un'applicazione ASP.NET di base può essere utilizzato direttamente al client.

[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample)

## <a name="serving-static-files"></a>Gestione dei file statici

File statici in genere si trovano nel `web root` (*\<contenuto radice > / wwwroot*) cartella. Vedere [contenuto radice](xref:fundamentals/index#content-root) e [radice Web](xref:fundamentals/index#web-root) per ulteriori informazioni. In genere imposta la radice del contenuto è la directory corrente in modo che il progetto `web root` verrà trovato mentre è in fase di sviluppo.

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]

File statici possono essere archiviati in qualsiasi cartella sotto la `web root` ed è accessibile tramite un percorso relativo a tale livello. Ad esempio, quando si crea un progetto di applicazione Web predefinita in Visual Studio, esistono diverse cartelle create all'interno di *wwwroot* cartella - *css*, *immagini*, e *js*. L'URI per accedere a un'immagine nel *immagini* sottocartella:

* `http://<app>/images/<imageFileName>`
* `http://localhost:9189/images/banner3.svg`

Per i file statici da gestire, è necessario configurare il [Middleware](middleware.md) per aggiungere file statici per la pipeline. Il middleware del file statico può essere configurato tramite l'aggiunta di una dipendenza sul *Microsoft.AspNetCore.StaticFiles* pacchetto al progetto e quindi chiamando il `UseStaticFiles` il metodo di estensione da `Startup.Configure`:

[!code-csharp[Main](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]

`app.UseStaticFiles();`rende i file in `web root` (*wwwroot* per impostazione predefinita) servable. In seguito verrà illustrato come rendere servable con altro contenuto della directory `UseStaticFiles`.

È necessario includere il pacchetto NuGet "Microsoft.AspNetCore.StaticFiles".

> [!NOTE]
> `web root`Per impostazione predefinita il *wwwroot* directory, ma è possibile impostare il `web root` directory con `UseWebRoot`.

Si supponga di avrà una gerarchia del progetto in cui i file statici che si desidera gestire sono di fuori di `web root`. Ad esempio:

* wwwroot
  * css
  * images
  * ...
* MyStaticFiles
  * test.PNG

Per una richiesta di accesso *test.png*, configurare il middleware di file statici come indicato di seguito:

[!code-csharp[Main](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]

Una richiesta di `http://<app>/StaticFiles/test.png` verrà utilizzato il *test.png* file.

`StaticFileOptions()`impostare le intestazioni di risposta. Ad esempio, il codice seguente imposta file statico che funge dal *wwwroot* cartella e imposta il `Cache-Control` intestazione per renderli pubblicamente memorizzabile nella cache per 10 minuti (600 secondi):

[!code-csharp[Main](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]

![Le intestazioni di risposta che mostra l'intestazione Cache-Control è stata aggiunta](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>Autorizzazione del file statico

Il modulo di file statici fornisce **non** controlli di autorizzazione. Qualsiasi file forniti, tra cui quelle contenute *wwwroot* sono disponibili pubblicamente. Per gestire i file in base alle autorizzazioni:

* Archiviarli all'esterno di *wwwroot* e qualsiasi directory accessibile per il middleware di file statici **e**

* Esse forniscono tramite un'azione del controller, restituendo un `FileResult` in cui viene applicata l'autorizzazione

## <a name="enabling-directory-browsing"></a>Abilitazione dell'esplorazione directory

Esplorazione directory consente all'utente dell'app web visualizzare un elenco di directory e file all'interno di una directory specificata. Esplorazione directory è disabilitata per impostazione predefinita per motivi di sicurezza (vedere [considerazioni](#considerations)). Per abilitare l'esplorazione directory, chiamare il `UseDirectoryBrowser` il metodo di estensione da `Startup.Configure`:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet1)]

E aggiungere i servizi necessari chiamando `AddDirectoryBrowser` il metodo di estensione da `Startup.ConfigureServices`:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet2)]

Il codice precedente consente l'esplorazione delle directory per il *wwwroot/immagini* cartella utilizzando l'URL http://\<app > / MyImages, con collegamenti ai singoli file e cartelle:

![esplorazione directory](static-files/_static/dir-browse.png)

Vedere [considerazioni](#considerations) sui rischi di sicurezza quando si abilita l'esplorazione.

Tenere presente le due `app.UseStaticFiles` chiamate. Il primo è richiesto per gestire il CSS, immagini e JavaScript nel *wwwroot* cartella e la seconda chiamata per l'esplorazione delle directory per il *wwwroot/immagini* cartella utilizzando l'URL http://\<app > / MyImages:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]

## <a name="serving-a-default-document"></a>Serve un documento predefinito

L'impostazione di una home page predefinita offre un punto di partenza quando si visita il sito visitatori del sito. Affinché l'app Web gestire una pagina predefinita senza che sia necessario qualificare completamente l'URI, chiamare il `UseDefaultFiles` il metodo di estensione da `Startup.Configure` come indicato di seguito.

[!code-csharp[Main](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]

> [!NOTE]
> `UseDefaultFiles`deve essere chiamato prima `UseStaticFiles` per servire il file predefinito. `UseDefaultFiles`è un URL processo di scrittura che non viene effettivamente servire il file. È necessario abilitare il middleware di file statici (`UseStaticFiles`) per servire il file.

Con `UseDefaultFiles`, cercheranno le richieste a una cartella:

* default.htm
* default.html
* htm
* index.html

Il primo file trovato nell'elenco verrà utilizzato come se la richiesta è l'URI completo (anche se l'URL del browser continuerà a visualizzare l'URI richiesto).

Il codice seguente viene illustrato come modificare il nome file predefinito per *mydefault.html*.

[!code-csharp[Main](static-files/sample/StartupDefault.cs?name=snippet1)]

## <a name="usefileserver"></a>UseFileServer

`UseFileServer`combina la funzionalità di `UseStaticFiles`, `UseDefaultFiles`, e `UseDirectoryBrowser`.

Il codice seguente permette di file statici e il file predefinito per essere gestiti, ma non consente l'esplorazione directory:

```csharp
app.UseFileServer();
   ```

Il codice seguente permette di file statici, i file predefiniti e esplorazione directory:

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
   ```

Vedere [considerazioni](#considerations) sui rischi di sicurezza quando si abilita l'esplorazione. Come con `UseStaticFiles`, `UseDefaultFiles`, e `UseDirectoryBrowser`, se si desidera gestire i file che esistono di fuori di `web root`, si crea un'istanza e si configura un `FileServerOptions` oggetto passato come parametro per `UseFileServer`. Nell'app Web, ad esempio, data la seguente gerarchia di directory:

* wwwroot

  * css

  * images

  * ...

* MyStaticFiles

  * test.PNG

  * default.html

Utilizzando l'esempio di gerarchia precedente, è opportuno abilitare file statici, i file predefiniti e cercare il `MyStaticFiles` directory. Nel frammento di codice seguente, che viene eseguita con una singola chiamata a `FileServerOptions`.

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]

Se `enableDirectoryBrowsing` è impostato su `true` è necessario chiamare `AddDirectoryBrowser` il metodo di estensione da `Startup.ConfigureServices`:

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?name=snippet2)]

Utilizzando la gerarchia di file e il codice precedente:

| URI            |                             Risposta  |
| ------- | ------|
| `http://<app>/StaticFiles/test.png`    |      MyStaticFiles/test.png |
| `http://<app>/StaticFiles`              |     MyStaticFiles/default.html |

Se si trovano in alcun valore predefinito denominato file di *MyStaticFiles* directory, http://\<app > / StaticFiles restituisce la visualizzazione con collegamenti selezionabili directory:

![Elenco di file statici](static-files/_static/db2.PNG)

> [!NOTE]
> `UseDefaultFiles`e `UseDirectoryBrowser` richiederà l'url http://\<app > / StaticFiles senza la barra finale e causa un lato client si viene reindirizzati http://\<app > /StaticFiles/ (aggiunta la barra finale). Senza gli URL relativi barra finale all'interno dei documenti non saranno corretti.

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

La `FileExtensionContentTypeProvider` classe contiene una raccolta di estensioni di file viene eseguito il mapping a tipi di contenuto MIME. Nell'esempio seguente, diverse estensioni di file registrate per i tipi MIME noti, viene sostituito "RTF" e "MP4" viene rimosso.

[!code-csharp[Main](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]

Vedere [tipi di contenuto MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).

## <a name="non-standard-content-types"></a>Tipi di contenuto non standard

Il middleware di file statici ASP.NET riconosce i tipi di contenuto di file conosciuti quasi 400. Se l'utente richiede un file di un tipo di file sconosciuto, il middleware di file statici restituisce una risposta HTTP 404 (non trovato). Se è abilitata l'esplorazione directory, verrà visualizzato un collegamento al file, ma l'URI verrà restituito un errore HTTP 404.

Il codice seguente consente serve tipi sconosciuti e verrà eseguito il rendering di file sconosciute come immagine.

[!code-csharp[Main](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]

Con il codice precedente, una richiesta per un file con un tipo di contenuto sconosciuto verrà restituita come un'immagine.

>[!WARNING]
> Abilitazione `ServeUnknownFileTypes` è un rischio per la sicurezza e l'utilizzo è sconsigliato.  `FileExtensionContentTypeProvider`(come spiegato in precedenza) offre un'alternativa più sicura per servire i file con estensione non standard.

### <a name="considerations"></a>Considerazioni

>[!WARNING]
> `UseDirectoryBrowser`e `UseStaticFiles` possono verificarsi perdite di informazioni riservate. È consigliabile che si **non** directory Attivazione esplorazione nell'ambiente di produzione. Prestare attenzione sulle directory di cui si abilita con `UseStaticFiles` o `UseDirectoryBrowser` come l'intera directory e tutte le sottodirectory saranno accessibili. Si consiglia di mantenere i contenuti pubblici nella propria directory, ad esempio * \<contenuto radice > / wwwroot*, lontano da visualizzazioni applicazione, i file di configurazione e così via.

* Gli URL per il contenuto esposti con `UseDirectoryBrowser` e `UseStaticFiles` sono soggetti a distinzione maiuscole/minuscole e le restrizioni del carattere del loro file system sottostante. Ad esempio, Windows viene fatta distinzione tra maiuscole e minuscole, ma Mac e Linux non sono.

* Le applicazioni ASP.NET Core ospitate in IIS utilizzano il modulo di base di ASP.NET per inoltrare tutte le richieste all'applicazione, incluse le richieste per i file statici. Il gestore di file statici di IIS non viene utilizzato perché non è di ottenere l'opportunità di gestire le richieste prima che vengano gestiti dal modulo di base di ASP.NET.

* Per rimuovere il gestore di file statici di IIS (a livello di sito Web o server):

     * Passare il **moduli** funzionalità

     * Selezionare **StaticFileModule** nell'elenco

     * Toccare **rimuovere** nel **azioni** barra laterale

>[!WARNING]
> Se il gestore di file statici di IIS è abilitato **e** il modulo di base di ASP.NET (ANCM) non è configurata correttamente (ad esempio se *Web. config* non è stata distribuita), file statici verranno serviti.

* File di codice (c# e Razor inclusi) devono essere posizionati all'esterno del progetto di app `web root` (*wwwroot* per impostazione predefinita). Crea una netta separazione tra il contenuto di lato client dell'app e codice sorgente sul lato server, che impedisce la divulgazione di codice lato server.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Middleware](middleware.md)

* [Introduzione a ASP.NET Core](../index.md)
