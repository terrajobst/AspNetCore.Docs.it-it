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
ms.openlocfilehash: ea6c180332dd5ab3a7238dcd73a4a1c8534c6243
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-working-with-static-files-in-aspnet-core"></a><span data-ttu-id="008f6-104">Introduzione all'utilizzo dei file statici di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="008f6-104">Introduction to working with static files in ASP.NET Core</span></span>

<a name=fundamentals-static-files></a>

<span data-ttu-id="008f6-105">Da [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="008f6-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="008f6-106">File statici, ad esempio HTML, CSS, immagini e JavaScript, sono attività che un'applicazione ASP.NET di base può essere utilizzato direttamente al client.</span><span class="sxs-lookup"><span data-stu-id="008f6-106">Static files, such as HTML, CSS, image, and JavaScript, are assets that an ASP.NET Core app can serve directly to clients.</span></span>

[<span data-ttu-id="008f6-107">Visualizzare o scaricare codice di esempio</span><span class="sxs-lookup"><span data-stu-id="008f6-107">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample)

## <a name="serving-static-files"></a><span data-ttu-id="008f6-108">Gestione dei file statici</span><span class="sxs-lookup"><span data-stu-id="008f6-108">Serving static files</span></span>

<span data-ttu-id="008f6-109">File statici in genere si trovano nel `web root` (*\<contenuto radice > / wwwroot*) cartella.</span><span class="sxs-lookup"><span data-stu-id="008f6-109">Static files are typically located in the `web root` (*\<content-root>/wwwroot*) folder.</span></span> <span data-ttu-id="008f6-110">Vedere [contenuto radice](xref:fundamentals/index#content-root) e [radice Web](xref:fundamentals/index#web-root) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="008f6-110">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span> <span data-ttu-id="008f6-111">In genere imposta la radice del contenuto è la directory corrente in modo che il progetto `web root` verrà trovato mentre è in fase di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="008f6-111">You generally set the content root to be the current directory so that your project's `web root` will be found while in development.</span></span>

<span data-ttu-id="008f6-112">[!code-csharp[Principale](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]</span><span class="sxs-lookup"><span data-stu-id="008f6-112">[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]</span></span>

<span data-ttu-id="008f6-113">File statici possono essere archiviati in qualsiasi cartella sotto la `web root` ed è accessibile tramite un percorso relativo a tale livello.</span><span class="sxs-lookup"><span data-stu-id="008f6-113">Static files can be stored in any folder under the `web root` and accessed with a relative path to that root.</span></span> <span data-ttu-id="008f6-114">Ad esempio, quando si crea un progetto di applicazione Web predefinita in Visual Studio, esistono diverse cartelle create all'interno di *wwwroot* cartella - *css*, *immagini*, e *js*.</span><span class="sxs-lookup"><span data-stu-id="008f6-114">For example, when you create a default Web application project using Visual Studio, there are several folders created within the *wwwroot*  folder - *css*, *images*, and *js*.</span></span> <span data-ttu-id="008f6-115">L'URI per accedere a un'immagine nel *immagini* sottocartella:</span><span class="sxs-lookup"><span data-stu-id="008f6-115">The URI to access an image in the *images* subfolder:</span></span>

* `http://<app>/images/<imageFileName>`
* `http://localhost:9189/images/banner3.svg`

<span data-ttu-id="008f6-116">Per i file statici da gestire, è necessario configurare il [Middleware](middleware.md) per aggiungere file statici per la pipeline.</span><span class="sxs-lookup"><span data-stu-id="008f6-116">In order for static files to be served, you must configure the [Middleware](middleware.md) to add static files to the pipeline.</span></span> <span data-ttu-id="008f6-117">Il middleware del file statico può essere configurato tramite l'aggiunta di una dipendenza sul *Microsoft.AspNetCore.StaticFiles* pacchetto al progetto e quindi chiamando il `UseStaticFiles` il metodo di estensione da `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="008f6-117">The static file middleware can be configured by adding a dependency on the *Microsoft.AspNetCore.StaticFiles* package to your project and then calling the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

<span data-ttu-id="008f6-118">[!code-csharp[Principale](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="008f6-118">[!code-csharp[Main](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]</span></span>

<span data-ttu-id="008f6-119">`app.UseStaticFiles();`rende i file in `web root` (*wwwroot* per impostazione predefinita) servable.</span><span class="sxs-lookup"><span data-stu-id="008f6-119">`app.UseStaticFiles();` makes the files in `web root` (*wwwroot* by default) servable.</span></span> <span data-ttu-id="008f6-120">In seguito verrà illustrato come rendere servable con altro contenuto della directory `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="008f6-120">Later I'll show how to make other directory contents servable with `UseStaticFiles`.</span></span>

<span data-ttu-id="008f6-121">È necessario includere il pacchetto NuGet "Microsoft.AspNetCore.StaticFiles".</span><span class="sxs-lookup"><span data-stu-id="008f6-121">You must include the NuGet package "Microsoft.AspNetCore.StaticFiles".</span></span>

> [!NOTE]
> <span data-ttu-id="008f6-122">`web root`Per impostazione predefinita il *wwwroot* directory, ma è possibile impostare il `web root` directory con `UseWebRoot`.</span><span class="sxs-lookup"><span data-stu-id="008f6-122">`web root` defaults to the *wwwroot* directory, but you can set the `web root` directory with `UseWebRoot`.</span></span>

<span data-ttu-id="008f6-123">Si supponga di avrà una gerarchia del progetto in cui i file statici che si desidera gestire sono di fuori di `web root`.</span><span class="sxs-lookup"><span data-stu-id="008f6-123">Suppose you have a project hierarchy where the static files you wish to serve are outside the `web root`.</span></span> <span data-ttu-id="008f6-124">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="008f6-124">For example:</span></span>

* <span data-ttu-id="008f6-125">Wwwroot</span><span class="sxs-lookup"><span data-stu-id="008f6-125">wwwroot</span></span>
  * <span data-ttu-id="008f6-126">css</span><span class="sxs-lookup"><span data-stu-id="008f6-126">css</span></span>
  * <span data-ttu-id="008f6-127">images</span><span class="sxs-lookup"><span data-stu-id="008f6-127">images</span></span>
  * <span data-ttu-id="008f6-128">...</span><span class="sxs-lookup"><span data-stu-id="008f6-128">...</span></span>
* <span data-ttu-id="008f6-129">MyStaticFiles</span><span class="sxs-lookup"><span data-stu-id="008f6-129">MyStaticFiles</span></span>
  * <span data-ttu-id="008f6-130">test.PNG</span><span class="sxs-lookup"><span data-stu-id="008f6-130">test.png</span></span>

<span data-ttu-id="008f6-131">Per una richiesta di accesso *test.png*, configurare il middleware di file statici come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="008f6-131">For a request to access *test.png*, configure the static files middleware as follows:</span></span>

<span data-ttu-id="008f6-132">[!code-csharp[Principale](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="008f6-132">[!code-csharp[Main](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]</span></span>

<span data-ttu-id="008f6-133">Una richiesta di `http://<app>/StaticFiles/test.png` verrà utilizzato il *test.png* file.</span><span class="sxs-lookup"><span data-stu-id="008f6-133">A request to `http://<app>/StaticFiles/test.png` will serve the *test.png* file.</span></span>

<span data-ttu-id="008f6-134">`StaticFileOptions()`impostare le intestazioni di risposta.</span><span class="sxs-lookup"><span data-stu-id="008f6-134">`StaticFileOptions()` can set response headers.</span></span> <span data-ttu-id="008f6-135">Ad esempio, il codice seguente imposta file statico che funge dal *wwwroot* cartella e imposta il `Cache-Control` intestazione per renderli pubblicamente memorizzabile nella cache per 10 minuti (600 secondi):</span><span class="sxs-lookup"><span data-stu-id="008f6-135">For example, the code below sets up static file serving from the *wwwroot* folder and sets the `Cache-Control` header to make them publicly cacheable for 10 minutes (600 seconds):</span></span>

<span data-ttu-id="008f6-136">[!code-csharp[Principale](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="008f6-136">[!code-csharp[Main](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]</span></span>

![Le intestazioni di risposta che mostra l'intestazione Cache-Control è stata aggiunta](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="008f6-138">Autorizzazione del file statico</span><span class="sxs-lookup"><span data-stu-id="008f6-138">Static file authorization</span></span>

<span data-ttu-id="008f6-139">Il modulo di file statici fornisce **non** controlli di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="008f6-139">The static file module provides **no** authorization checks.</span></span> <span data-ttu-id="008f6-140">Qualsiasi file forniti, tra cui quelle contenute *wwwroot* sono disponibili pubblicamente.</span><span class="sxs-lookup"><span data-stu-id="008f6-140">Any files served by it, including those under *wwwroot* are publicly available.</span></span> <span data-ttu-id="008f6-141">Per gestire i file in base alle autorizzazioni:</span><span class="sxs-lookup"><span data-stu-id="008f6-141">To serve files based on authorization:</span></span>

* <span data-ttu-id="008f6-142">Archiviarli all'esterno di *wwwroot* e qualsiasi directory accessibile per il middleware di file statici **e**</span><span class="sxs-lookup"><span data-stu-id="008f6-142">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>

* <span data-ttu-id="008f6-143">Esse forniscono tramite un'azione del controller, restituendo un `FileResult` in cui viene applicata l'autorizzazione</span><span class="sxs-lookup"><span data-stu-id="008f6-143">Serve them through a controller action, returning a `FileResult` where authorization is applied</span></span>

## <a name="enabling-directory-browsing"></a><span data-ttu-id="008f6-144">Abilitazione dell'esplorazione directory</span><span class="sxs-lookup"><span data-stu-id="008f6-144">Enabling directory browsing</span></span>

<span data-ttu-id="008f6-145">Esplorazione directory consente all'utente dell'app web visualizzare un elenco di directory e file all'interno di una directory specificata.</span><span class="sxs-lookup"><span data-stu-id="008f6-145">Directory browsing allows the user of your web app to see a list of directories and files within a specified directory.</span></span> <span data-ttu-id="008f6-146">Esplorazione directory è disabilitata per impostazione predefinita per motivi di sicurezza (vedere [considerazioni](#considerations)).</span><span class="sxs-lookup"><span data-stu-id="008f6-146">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="008f6-147">Per abilitare l'esplorazione directory, chiamare il `UseDirectoryBrowser` il metodo di estensione da `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="008f6-147">To enable directory browsing, call the `UseDirectoryBrowser` extension method from  `Startup.Configure`:</span></span>

<span data-ttu-id="008f6-148">[!code-csharp[Principale](static-files/sample/StartupBrowse.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="008f6-148">[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet1)]</span></span>

<span data-ttu-id="008f6-149">E aggiungere i servizi necessari chiamando `AddDirectoryBrowser` il metodo di estensione da `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="008f6-149">And add required services by calling `AddDirectoryBrowser` extension method from `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="008f6-150">[!code-csharp[Principale](static-files/sample/StartupBrowse.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="008f6-150">[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet2)]</span></span>

<span data-ttu-id="008f6-151">Il codice precedente consente l'esplorazione delle directory per il *wwwroot/immagini* cartella utilizzando l'URL http://\<app > / MyImages, con collegamenti ai singoli file e cartelle:</span><span class="sxs-lookup"><span data-stu-id="008f6-151">The code above allows directory browsing of the *wwwroot/images* folder using the URL http://\<app>/MyImages, with links to each file and folder:</span></span>

![esplorazione directory](static-files/_static/dir-browse.png)

<span data-ttu-id="008f6-153">Vedere [considerazioni](#considerations) sui rischi di sicurezza quando si abilita l'esplorazione.</span><span class="sxs-lookup"><span data-stu-id="008f6-153">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="008f6-154">Tenere presente le due `app.UseStaticFiles` chiamate.</span><span class="sxs-lookup"><span data-stu-id="008f6-154">Note the two `app.UseStaticFiles` calls.</span></span> <span data-ttu-id="008f6-155">Il primo è richiesto per gestire il CSS, immagini e JavaScript nel *wwwroot* cartella e la seconda chiamata per l'esplorazione delle directory per il *wwwroot/immagini* cartella utilizzando l'URL http://\<app > / MyImages:</span><span class="sxs-lookup"><span data-stu-id="008f6-155">The first one is required to serve the CSS, images and JavaScript in the *wwwroot* folder, and the second call for directory browsing of the *wwwroot/images* folder using the URL http://\<app>/MyImages:</span></span>

<span data-ttu-id="008f6-156">[!code-csharp[Principale](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="008f6-156">[!code-csharp[Main](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]</span></span>

## <a name="serving-a-default-document"></a><span data-ttu-id="008f6-157">Serve un documento predefinito</span><span class="sxs-lookup"><span data-stu-id="008f6-157">Serving a default document</span></span>

<span data-ttu-id="008f6-158">L'impostazione di una home page predefinita offre un punto di partenza quando si visita il sito visitatori del sito.</span><span class="sxs-lookup"><span data-stu-id="008f6-158">Setting a default home page gives site visitors a place to start when visiting your site.</span></span> <span data-ttu-id="008f6-159">Affinché l'app Web gestire una pagina predefinita senza che sia necessario qualificare completamente l'URI, chiamare il `UseDefaultFiles` il metodo di estensione da `Startup.Configure` come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="008f6-159">In order for your Web app to serve a default page without the user having to fully qualify the URI, call the `UseDefaultFiles` extension method from `Startup.Configure` as follows.</span></span>

<span data-ttu-id="008f6-160">[!code-csharp[Principale](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="008f6-160">[!code-csharp[Main](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]</span></span>

> [!NOTE]
> <span data-ttu-id="008f6-161">`UseDefaultFiles`deve essere chiamato prima `UseStaticFiles` per servire il file predefinito.</span><span class="sxs-lookup"><span data-stu-id="008f6-161">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="008f6-162">`UseDefaultFiles`è un URL processo di scrittura che non viene effettivamente servire il file.</span><span class="sxs-lookup"><span data-stu-id="008f6-162">`UseDefaultFiles` is a URL re-writer that doesn't actually serve the file.</span></span> <span data-ttu-id="008f6-163">È necessario abilitare il middleware di file statici (`UseStaticFiles`) per servire il file.</span><span class="sxs-lookup"><span data-stu-id="008f6-163">You must enable the static file middleware (`UseStaticFiles`) to serve the file.</span></span>

<span data-ttu-id="008f6-164">Con `UseDefaultFiles`, cercheranno le richieste a una cartella:</span><span class="sxs-lookup"><span data-stu-id="008f6-164">With `UseDefaultFiles`, requests to a folder will search for:</span></span>

* <span data-ttu-id="008f6-165">default.htm</span><span class="sxs-lookup"><span data-stu-id="008f6-165">default.htm</span></span>
* <span data-ttu-id="008f6-166">default.html</span><span class="sxs-lookup"><span data-stu-id="008f6-166">default.html</span></span>
* <span data-ttu-id="008f6-167">htm</span><span class="sxs-lookup"><span data-stu-id="008f6-167">index.htm</span></span>
* <span data-ttu-id="008f6-168">index.html</span><span class="sxs-lookup"><span data-stu-id="008f6-168">index.html</span></span>

<span data-ttu-id="008f6-169">Il primo file trovato nell'elenco verrà utilizzato come se la richiesta è l'URI completo (anche se l'URL del browser continuerà a visualizzare l'URI richiesto).</span><span class="sxs-lookup"><span data-stu-id="008f6-169">The first file found from the list will be served as if the request was the fully qualified URI (although the browser URL will continue to show the URI requested).</span></span>

<span data-ttu-id="008f6-170">Il codice seguente viene illustrato come modificare il nome file predefinito per *mydefault.html*.</span><span class="sxs-lookup"><span data-stu-id="008f6-170">The following code shows how to change the default file name to *mydefault.html*.</span></span>

<span data-ttu-id="008f6-171">[!code-csharp[Principale](static-files/sample/StartupDefault.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="008f6-171">[!code-csharp[Main](static-files/sample/StartupDefault.cs?name=snippet1)]</span></span>

## <a name="usefileserver"></a><span data-ttu-id="008f6-172">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="008f6-172">UseFileServer</span></span>

<span data-ttu-id="008f6-173">`UseFileServer`combina la funzionalità di `UseStaticFiles`, `UseDefaultFiles`, e `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="008f6-173">`UseFileServer` combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="008f6-174">Il codice seguente permette di file statici e il file predefinito per essere gestiti, ma non consente l'esplorazione directory:</span><span class="sxs-lookup"><span data-stu-id="008f6-174">The following code enables static files and the default file to be served, but does not allow directory browsing:</span></span>

```csharp
app.UseFileServer();
   ```

<span data-ttu-id="008f6-175">Il codice seguente permette di file statici, i file predefiniti e esplorazione directory:</span><span class="sxs-lookup"><span data-stu-id="008f6-175">The following code enables static files, default files and  directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
   ```

<span data-ttu-id="008f6-176">Vedere [considerazioni](#considerations) sui rischi di sicurezza quando si abilita l'esplorazione.</span><span class="sxs-lookup"><span data-stu-id="008f6-176">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span> <span data-ttu-id="008f6-177">Come con `UseStaticFiles`, `UseDefaultFiles`, e `UseDirectoryBrowser`, se si desidera gestire i file che esistono di fuori di `web root`, si crea un'istanza e si configura un `FileServerOptions` oggetto passato come parametro per `UseFileServer`.</span><span class="sxs-lookup"><span data-stu-id="008f6-177">As with `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`, if you wish to serve files that exist outside the `web root`, you instantiate and configure an `FileServerOptions` object that you pass as a parameter to `UseFileServer`.</span></span> <span data-ttu-id="008f6-178">Nell'app Web, ad esempio, data la seguente gerarchia di directory:</span><span class="sxs-lookup"><span data-stu-id="008f6-178">For example, given the following directory hierarchy in your Web app:</span></span>

* <span data-ttu-id="008f6-179">Wwwroot</span><span class="sxs-lookup"><span data-stu-id="008f6-179">wwwroot</span></span>

  * <span data-ttu-id="008f6-180">css</span><span class="sxs-lookup"><span data-stu-id="008f6-180">css</span></span>

  * <span data-ttu-id="008f6-181">images</span><span class="sxs-lookup"><span data-stu-id="008f6-181">images</span></span>

  * <span data-ttu-id="008f6-182">...</span><span class="sxs-lookup"><span data-stu-id="008f6-182">...</span></span>

* <span data-ttu-id="008f6-183">MyStaticFiles</span><span class="sxs-lookup"><span data-stu-id="008f6-183">MyStaticFiles</span></span>

  * <span data-ttu-id="008f6-184">test.PNG</span><span class="sxs-lookup"><span data-stu-id="008f6-184">test.png</span></span>

  * <span data-ttu-id="008f6-185">default.html</span><span class="sxs-lookup"><span data-stu-id="008f6-185">default.html</span></span>

<span data-ttu-id="008f6-186">Utilizzando l'esempio di gerarchia precedente, è opportuno abilitare file statici, i file predefiniti e cercare il `MyStaticFiles` directory.</span><span class="sxs-lookup"><span data-stu-id="008f6-186">Using the hierarchy example above, you might want to enable static files, default files, and browsing for the `MyStaticFiles` directory.</span></span> <span data-ttu-id="008f6-187">Nel frammento di codice seguente, che viene eseguita con una singola chiamata a `FileServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="008f6-187">In the following code snippet, that is accomplished with a single call to `FileServerOptions`.</span></span>

<span data-ttu-id="008f6-188">[!code-csharp[Principale](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="008f6-188">[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]</span></span>

<span data-ttu-id="008f6-189">Se `enableDirectoryBrowsing` è impostato su `true` è necessario chiamare `AddDirectoryBrowser` il metodo di estensione da `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="008f6-189">If `enableDirectoryBrowsing` is set to `true` you are required to call `AddDirectoryBrowser` extension method from  `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="008f6-190">[!code-csharp[Principale](static-files/sample/StartupUseFileServer.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="008f6-190">[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?name=snippet2)]</span></span>

<span data-ttu-id="008f6-191">Utilizzando la gerarchia di file e il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="008f6-191">Using the file hierarchy and code above:</span></span>

| <span data-ttu-id="008f6-192">URI</span><span class="sxs-lookup"><span data-stu-id="008f6-192">URI</span></span>            |                             <span data-ttu-id="008f6-193">Risposta</span><span class="sxs-lookup"><span data-stu-id="008f6-193">Response</span></span>  |
| ------- | ------|
| `http://<app>/StaticFiles/test.png`    |      <span data-ttu-id="008f6-194">MyStaticFiles/test.png</span><span class="sxs-lookup"><span data-stu-id="008f6-194">MyStaticFiles/test.png</span></span> |
| `http://<app>/StaticFiles`              |     <span data-ttu-id="008f6-195">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="008f6-195">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="008f6-196">Se si trovano in alcun valore predefinito denominato file di *MyStaticFiles* directory, http://\<app > / StaticFiles restituisce la visualizzazione con collegamenti selezionabili directory:</span><span class="sxs-lookup"><span data-stu-id="008f6-196">If no default named files are in the *MyStaticFiles* directory, http://\<app>/StaticFiles returns the directory listing with clickable links:</span></span>

![Elenco di file statici](static-files/_static/db2.PNG)

> [!NOTE]
> <span data-ttu-id="008f6-198">`UseDefaultFiles`e `UseDirectoryBrowser` richiederà l'url http://\<app > / StaticFiles senza la barra finale e causa un lato client si viene reindirizzati http://\<app > /StaticFiles/ (aggiunta la barra finale).</span><span class="sxs-lookup"><span data-stu-id="008f6-198">`UseDefaultFiles` and `UseDirectoryBrowser` will take the url http://\<app>/StaticFiles without the trailing slash and cause a client side redirect to http://\<app>/StaticFiles/ (adding the trailing slash).</span></span> <span data-ttu-id="008f6-199">Senza gli URL relativi barra finale all'interno dei documenti non saranno corretti.</span><span class="sxs-lookup"><span data-stu-id="008f6-199">Without the trailing slash relative URLs within the documents would be incorrect.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="008f6-200">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="008f6-200">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="008f6-201">La `FileExtensionContentTypeProvider` classe contiene una raccolta di estensioni di file viene eseguito il mapping a tipi di contenuto MIME.</span><span class="sxs-lookup"><span data-stu-id="008f6-201">The `FileExtensionContentTypeProvider` class contains a  collection that maps file extensions to MIME content types.</span></span> <span data-ttu-id="008f6-202">Nell'esempio seguente, diverse estensioni di file registrate per i tipi MIME noti, viene sostituito "RTF" e "MP4" viene rimosso.</span><span class="sxs-lookup"><span data-stu-id="008f6-202">In the following sample, several file extensions are registered to known MIME types, the ".rtf" is replaced, and ".mp4" is removed.</span></span>

<span data-ttu-id="008f6-203">[!code-csharp[Principale](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="008f6-203">[!code-csharp[Main](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]</span></span>

<span data-ttu-id="008f6-204">Vedere [tipi di contenuto MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).</span><span class="sxs-lookup"><span data-stu-id="008f6-204">See   [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="008f6-205">Tipi di contenuto non standard</span><span class="sxs-lookup"><span data-stu-id="008f6-205">Non-standard content types</span></span>

<span data-ttu-id="008f6-206">Il middleware di file statici ASP.NET riconosce i tipi di contenuto di file conosciuti quasi 400.</span><span class="sxs-lookup"><span data-stu-id="008f6-206">The ASP.NET static file middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="008f6-207">Se l'utente richiede un file di un tipo di file sconosciuto, il middleware di file statici restituisce una risposta HTTP 404 (non trovato).</span><span class="sxs-lookup"><span data-stu-id="008f6-207">If the user requests a file of an unknown file type, the static file middleware returns a HTTP 404 (Not found) response.</span></span> <span data-ttu-id="008f6-208">Se è abilitata l'esplorazione directory, verrà visualizzato un collegamento al file, ma l'URI verrà restituito un errore HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="008f6-208">If directory browsing is enabled, a link to the file will be displayed, but the URI will return an HTTP 404 error.</span></span>

<span data-ttu-id="008f6-209">Il codice seguente consente serve tipi sconosciuti e verrà eseguito il rendering di file sconosciute come immagine.</span><span class="sxs-lookup"><span data-stu-id="008f6-209">The following code enables serving unknown types and will render the unknown file as an image.</span></span>

<span data-ttu-id="008f6-210">[!code-csharp[Principale](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="008f6-210">[!code-csharp[Main](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]</span></span>

<span data-ttu-id="008f6-211">Con il codice precedente, una richiesta per un file con un tipo di contenuto sconosciuto verrà restituita come un'immagine.</span><span class="sxs-lookup"><span data-stu-id="008f6-211">With the code above, a request for a file with an unknown content type will be returned as an image.</span></span>

>[!WARNING]
> <span data-ttu-id="008f6-212">Abilitazione `ServeUnknownFileTypes` è un rischio per la sicurezza e l'utilizzo è sconsigliato.</span><span class="sxs-lookup"><span data-stu-id="008f6-212">Enabling `ServeUnknownFileTypes` is a security risk and using it is discouraged.</span></span>  <span data-ttu-id="008f6-213">`FileExtensionContentTypeProvider`(come spiegato in precedenza) offre un'alternativa più sicura per servire i file con estensione non standard.</span><span class="sxs-lookup"><span data-stu-id="008f6-213">`FileExtensionContentTypeProvider`  (explained above) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="008f6-214">Considerazioni</span><span class="sxs-lookup"><span data-stu-id="008f6-214">Considerations</span></span>

>[!WARNING]
> <span data-ttu-id="008f6-215">`UseDirectoryBrowser`e `UseStaticFiles` possono verificarsi perdite di informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="008f6-215">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="008f6-216">È consigliabile che si **non** directory Attivazione esplorazione nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="008f6-216">We recommend that you **not** enable directory browsing in production.</span></span> <span data-ttu-id="008f6-217">Prestare attenzione sulle directory di cui si abilita con `UseStaticFiles` o `UseDirectoryBrowser` come l'intera directory e tutte le sottodirectory saranno accessibili.</span><span class="sxs-lookup"><span data-stu-id="008f6-217">Be careful about which directories you enable with `UseStaticFiles` or `UseDirectoryBrowser` as the entire directory and all sub-directories will be accessible.</span></span> <span data-ttu-id="008f6-218">Si consiglia di mantenere i contenuti pubblici nella propria directory, ad esempio  *\<contenuto radice > / wwwroot*, lontano da visualizzazioni applicazione, i file di configurazione e così via.</span><span class="sxs-lookup"><span data-stu-id="008f6-218">We recommend keeping public content in its own directory such as *\<content root>/wwwroot*, away from application views, configuration files, etc.</span></span>

* <span data-ttu-id="008f6-219">Gli URL per il contenuto esposti con `UseDirectoryBrowser` e `UseStaticFiles` sono soggetti a distinzione maiuscole/minuscole e le restrizioni del carattere del loro file system sottostante.</span><span class="sxs-lookup"><span data-stu-id="008f6-219">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of their underlying file system.</span></span> <span data-ttu-id="008f6-220">Ad esempio, Windows viene fatta distinzione tra maiuscole e minuscole, ma Mac e Linux non sono.</span><span class="sxs-lookup"><span data-stu-id="008f6-220">For example, Windows is case insensitive, but Mac and Linux are not.</span></span>

* <span data-ttu-id="008f6-221">Le applicazioni ASP.NET Core ospitate in IIS utilizzano il modulo di base di ASP.NET per inoltrare tutte le richieste all'applicazione, incluse le richieste per i file statici.</span><span class="sxs-lookup"><span data-stu-id="008f6-221">ASP.NET Core applications hosted in IIS use the ASP.NET Core Module to forward all requests to the application including requests for static files.</span></span> <span data-ttu-id="008f6-222">Il gestore di file statici di IIS non viene utilizzato perché non è di ottenere l'opportunità di gestire le richieste prima che vengano gestiti dal modulo di base di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="008f6-222">The IIS static file handler is not used because it doesn't get a chance to handle requests before they are handled by the ASP.NET Core Module.</span></span>

* <span data-ttu-id="008f6-223">Per rimuovere il gestore di file statici di IIS (a livello di sito Web o server):</span><span class="sxs-lookup"><span data-stu-id="008f6-223">To remove the IIS static file handler (at the server or website level):</span></span>

     * <span data-ttu-id="008f6-224">Passare il **moduli** funzionalità</span><span class="sxs-lookup"><span data-stu-id="008f6-224">Navigate to the **Modules** feature</span></span>

     * <span data-ttu-id="008f6-225">Selezionare **StaticFileModule** nell'elenco</span><span class="sxs-lookup"><span data-stu-id="008f6-225">Select **StaticFileModule** in the list</span></span>

     * <span data-ttu-id="008f6-226">Toccare **rimuovere** nel **azioni** barra laterale</span><span class="sxs-lookup"><span data-stu-id="008f6-226">Tap **Remove** in the **Actions** sidebar</span></span>

>[!WARNING]
> <span data-ttu-id="008f6-227">Se il gestore di file statici di IIS è abilitato **e** il modulo di base di ASP.NET (ANCM) non è configurata correttamente (ad esempio se *Web. config* non è stata distribuita), file statici verranno serviti.</span><span class="sxs-lookup"><span data-stu-id="008f6-227">If the IIS static file handler is enabled **and** the ASP.NET Core Module (ANCM) is not correctly configured (for example if *web.config* was not deployed), static files will be served.</span></span>

* <span data-ttu-id="008f6-228">File di codice (c# e Razor inclusi) devono essere posizionati all'esterno del progetto di app `web root` (*wwwroot* per impostazione predefinita).</span><span class="sxs-lookup"><span data-stu-id="008f6-228">Code files (including c# and Razor) should be placed outside of the app project's `web root` (*wwwroot* by default).</span></span> <span data-ttu-id="008f6-229">Crea una netta separazione tra il contenuto di lato client dell'app e codice sorgente sul lato server, che impedisce la divulgazione di codice lato server.</span><span class="sxs-lookup"><span data-stu-id="008f6-229">This creates a clean separation between your app's client side content and server side source code, which prevents server side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="008f6-230">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="008f6-230">Additional Resources</span></span>

* [<span data-ttu-id="008f6-231">Middleware</span><span class="sxs-lookup"><span data-stu-id="008f6-231">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="008f6-232">Introduzione a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="008f6-232">Introduction to ASP.NET Core</span></span>](../index.md)
