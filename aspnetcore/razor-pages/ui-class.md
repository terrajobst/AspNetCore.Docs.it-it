---
title: Interfaccia utente Razor riutilizzabile nelle librerie di classi con ASP.NET Core
author: Rick-Anderson
description: Viene illustrato come creare un'interfaccia Razor riutilizzabili tramite le visualizzazioni parziali in una libreria di classi in ASP.NET Core.
ms.author: riande
ms.date: 01/25/2020
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 8813244ea6d00b170d9f95d12743e9fee38bf810
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172654"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="a4f6d-103">Creare un'interfaccia utente riutilizzabile usando il progetto libreria di classi Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a4f6d-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="a4f6d-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a4f6d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a4f6d-105">Le visualizzazioni Razor, le pagine, i controller, i modelli di pagina, i [componenti Razor](xref:blazor/class-libraries), i [componenti di visualizzazione](xref:mvc/views/view-components)e i modelli di dati possono essere incorporati in una libreria di classi Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="a4f6d-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="a4f6d-106">La libreria di classi Razor può essere inclusa nel pacchetto e usata nuovamente.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="a4f6d-107">Le applicazioni possono includere la libreria di classi Razor ed eseguire l'override delle visualizzazioni e pagine in essa contenute.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="a4f6d-108">Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="a4f6d-109">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a4f6d-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="a4f6d-110">Creare una libreria di classi contenente l'interfaccia utente Razor</span><span class="sxs-lookup"><span data-stu-id="a4f6d-110">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4f6d-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4f6d-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a4f6d-112">In Visual Studio selezionare **Crea nuovo nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-112">From Visual Studio select **Create new a new project**.</span></span>
* <span data-ttu-id="a4f6d-113">Selezionare **libreria di classi Razor** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-113">Select **Razor Class Library** > **Next**.</span></span>
* <span data-ttu-id="a4f6d-114">Assegnare un nome alla libreria (ad esempio, "RazorClassLib") > **creare**.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-114">Name the library (for example, "RazorClassLib"), > **Create**.</span></span> <span data-ttu-id="a4f6d-115">Per evitare un conflitto di nomi di file con la libreria di visualizzazione generata, verificare che il nome della libreria non finisca per `.Views`.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-115">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="a4f6d-116">Se è necessario supportare le visualizzazioni, selezionare **pagine e visualizzazioni di supporto** .</span><span class="sxs-lookup"><span data-stu-id="a4f6d-116">Select **Support pages and views** if you need to support views.</span></span> <span data-ttu-id="a4f6d-117">Per impostazione predefinita, sono supportati solo Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-117">By default, only Razor Pages are supported.</span></span> <span data-ttu-id="a4f6d-118">Selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-118">Select **Create**.</span></span>

<span data-ttu-id="a4f6d-119">Per impostazione predefinita, il modello RCL (Razor Class Library) per impostazione predefinita è lo sviluppo di componenti Razor.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-119">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="a4f6d-120">L'opzione **pagine e visualizzazioni di supporto** supporta pagine e viste.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-120">The **Support pages and views** option supports pages and views.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a4f6d-121">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="a4f6d-121">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a4f6d-122">Eseguire `dotnet new razorclasslib` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-122">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="a4f6d-123">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="a4f6d-123">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="a4f6d-124">Per impostazione predefinita, il modello RCL (Razor Class Library) per impostazione predefinita è lo sviluppo di componenti Razor.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-124">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="a4f6d-125">Passare l'opzione `--support-pages-and-views` (`dotnet new razorclasslib --support-pages-and-views`) per fornire il supporto per le pagine e le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-125">Pass the `--support-pages-and-views` option (`dotnet new razorclasslib --support-pages-and-views`) to provide support for pages and views.</span></span>

<span data-ttu-id="a4f6d-126">Per altre informazioni, vedere [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="a4f6d-126">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="a4f6d-127">Per evitare un conflitto di nomi di file con la libreria di visualizzazione generata, verificare che il nome della libreria non finisca per `.Views`.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-127">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="a4f6d-128">Aggiungere i file Razor alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-128">Add Razor files to the RCL.</span></span>

<span data-ttu-id="a4f6d-129">I modelli di ASP.NET Core presuppongono che il contenuto RCL si trovi nella cartella *aree* .</span><span class="sxs-lookup"><span data-stu-id="a4f6d-129">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="a4f6d-130">Vedere [layout di pagine RCL](#rcl-pages-layout) per creare un RCL che espone il contenuto in `~/Pages` anziché `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-130">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="a4f6d-131">Contenuto RCL di riferimento</span><span class="sxs-lookup"><span data-stu-id="a4f6d-131">Reference RCL content</span></span>

<span data-ttu-id="a4f6d-132">Il riferimento alla libreria di classi Razor può essere eseguito da:</span><span class="sxs-lookup"><span data-stu-id="a4f6d-132">The RCL can be referenced by:</span></span>

* <span data-ttu-id="a4f6d-133">Pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-133">NuGet package.</span></span> <span data-ttu-id="a4f6d-134">Vedere [Creazione di pacchetti NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) e [Creare e pubblicare un pacchetto NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="a4f6d-134">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="a4f6d-135">*{ProjectName}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-135">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="a4f6d-136">Vedere [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="a4f6d-136">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="a4f6d-137">Eseguire l'override di visualizzazioni, visualizzazioni parziali e pagine</span><span class="sxs-lookup"><span data-stu-id="a4f6d-137">Override views, partial views, and pages</span></span>

<span data-ttu-id="a4f6d-138">Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-138">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="a4f6d-139">Ad esempio, aggiungere *app Web 1/areas/la funzionalità/Pages/Pagina1. cshtml* a app Web 1 e Pagina1 in app Web 1 avrà la precedenza su PAGINA1 in RCL.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-139">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="a4f6d-140">Nel download di esempio, rinominare *WebApp1/aree/MyFeature2* in *WebApp1/aree/MyFeature* per testare la precedenza.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-140">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="a4f6d-141">Copiare la visualizzazione parziali *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* in *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-141">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="a4f6d-142">Aggiornare il markup per indicare la nuova posizione.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-142">Update the markup to indicate the new location.</span></span> <span data-ttu-id="a4f6d-143">Compilare ed eseguire l'app per verificare quale versione del parziale sia in uso.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-143">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="a4f6d-144">Layout delle pagine RCL</span><span class="sxs-lookup"><span data-stu-id="a4f6d-144">RCL Pages layout</span></span>

<span data-ttu-id="a4f6d-145">Per fare riferimento al contenuto RCL come se facesse parte della cartella *pagine* dell'app Web, creare il progetto RCL con la struttura di file seguente:</span><span class="sxs-lookup"><span data-stu-id="a4f6d-145">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="a4f6d-146">*RazorUIClassLib/pagine*</span><span class="sxs-lookup"><span data-stu-id="a4f6d-146">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="a4f6d-147">*RazorUIClassLib/pagine/condiviso*</span><span class="sxs-lookup"><span data-stu-id="a4f6d-147">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="a4f6d-148">Si supponga che *RazorUIClassLib/Pages/Shared* contenga due file parziali: *_Header. cshtml* e *_Footer. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-148">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="a4f6d-149">È possibile aggiungere i tag `<partial>` al file *_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="a4f6d-149">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="a4f6d-150">Creare un RCL con asset statici</span><span class="sxs-lookup"><span data-stu-id="a4f6d-150">Create an RCL with static assets</span></span>

<span data-ttu-id="a4f6d-151">Un RCL può richiedere risorse statiche complementari a cui è possibile fare riferimento tramite il RCL o l'app consumer di RCL.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-151">An RCL may require companion static assets that can be referenced by either the RCL or the consuming app of the RCL.</span></span> <span data-ttu-id="a4f6d-152">ASP.NET Core consente di creare RCL che includono risorse statiche disponibili per un'app di consumo.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-152">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="a4f6d-153">Per includere asset complementari come parte di un RCL, creare una cartella *wwwroot* nella libreria di classi e includere tutti i file necessari in tale cartella.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-153">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="a4f6d-154">Quando si esegue la compressione di un RCL, tutti gli asset complementari nella cartella *wwwroot* vengono inclusi automaticamente nel pacchetto.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-154">When packing an RCL, all companion assets in the *wwwroot* folder are automatically included in the package.</span></span>

### <a name="exclude-static-assets"></a><span data-ttu-id="a4f6d-155">Escludi asset statici</span><span class="sxs-lookup"><span data-stu-id="a4f6d-155">Exclude static assets</span></span>

<span data-ttu-id="a4f6d-156">Per escludere asset statici, aggiungere il percorso di esclusione desiderato al gruppo di proprietà `$(DefaultItemExcludes)` nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-156">To exclude static assets, add the desired exclusion path to the `$(DefaultItemExcludes)` property group in the project file.</span></span> <span data-ttu-id="a4f6d-157">Separare le voci con un punto e virgola (`;`).</span><span class="sxs-lookup"><span data-stu-id="a4f6d-157">Separate entries with a semicolon (`;`).</span></span>

<span data-ttu-id="a4f6d-158">Nell'esempio seguente, il foglio di stile *lib. CSS* nella cartella *wwwroot* non è considerato un asset statico e non è incluso nel RCL pubblicato:</span><span class="sxs-lookup"><span data-stu-id="a4f6d-158">In the following example, the *lib.css* stylesheet in the *wwwroot* folder isn't considered a static asset and isn't included in the published RCL:</span></span>

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a><span data-ttu-id="a4f6d-159">Integrazione typescript</span><span class="sxs-lookup"><span data-stu-id="a4f6d-159">Typescript integration</span></span>

<span data-ttu-id="a4f6d-160">Per includere i file TypeScript in un RCL:</span><span class="sxs-lookup"><span data-stu-id="a4f6d-160">To include TypeScript files in an RCL:</span></span>

1. <span data-ttu-id="a4f6d-161">Inserire i file TypeScript ( *. TS*) all'esterno della cartella *wwwroot*</span><span class="sxs-lookup"><span data-stu-id="a4f6d-161">Place the TypeScript files (*.ts*) outside of the *wwwroot* folder.</span></span> <span data-ttu-id="a4f6d-162">Inserire, ad esempio, i file in una cartella *client* .</span><span class="sxs-lookup"><span data-stu-id="a4f6d-162">For example, place the files in a *Client* folder.</span></span>

1. <span data-ttu-id="a4f6d-163">Configurare l'output di compilazione TypeScript per la cartella *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="a4f6d-163">Configure the TypeScript build output for the *wwwroot* folder.</span></span> <span data-ttu-id="a4f6d-164">Impostare la proprietà `TypescriptOutDir` all'interno di un `PropertyGroup` nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="a4f6d-164">Set the `TypescriptOutDir` property inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. <span data-ttu-id="a4f6d-165">Includere la destinazione TypeScript come dipendenza della destinazione `ResolveCurrentProjectStaticWebAssets` aggiungendo la destinazione seguente all'interno di un `PropertyGroup` nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="a4f6d-165">Include the TypeScript target as a dependency of the `ResolveCurrentProjectStaticWebAssets` target by adding the following target inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     CompileTypeScript;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="a4f6d-166">Utilizzare il contenuto da un RCL a cui si fa riferimento</span><span class="sxs-lookup"><span data-stu-id="a4f6d-166">Consume content from a referenced RCL</span></span>

<span data-ttu-id="a4f6d-167">I file inclusi nella cartella *wwwroot* di RCL vengono esposti a RCL o all'app consumer sotto il prefisso `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-167">The files included in the *wwwroot* folder of the RCL are exposed to either the RCL or the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="a4f6d-168">Ad esempio, una libreria denominata *Razor. Class. lib* genera un percorso del contenuto statico in `_content/Razor.Class.Lib/`.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-168">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span> <span data-ttu-id="a4f6d-169">Quando si produce un pacchetto NuGet e il nome dell'assembly non corrisponde all'ID del pacchetto, usare l'ID del pacchetto per `{LIBRARY NAME}`.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-169">When producing a NuGet package and the assembly name isn't the same as the package ID, use the package ID for `{LIBRARY NAME}`.</span></span>

<span data-ttu-id="a4f6d-170">L'app consumer fa riferimento a risorse statiche fornite dalla libreria con `<script>`, `<style>`, `<img>`e altri tag HTML.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-170">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="a4f6d-171">Per l'app consumer deve essere abilitato il [supporto dei file statici](xref:fundamentals/static-files) in `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="a4f6d-171">The consuming app must have [static file support](xref:fundamentals/static-files) enabled in `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="a4f6d-172">Quando si esegue l'app consumer dall'output di compilazione (`dotnet run`), le risorse Web statiche sono abilitate per impostazione predefinita nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-172">When running the consuming app from build output (`dotnet run`), static web assets are enabled by default in the Development environment.</span></span> <span data-ttu-id="a4f6d-173">Per supportare asset in altri ambienti durante l'esecuzione dall'output di compilazione, chiamare `UseStaticWebAssets` nel generatore host in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="a4f6d-173">To support assets in other environments when running from build output, call `UseStaticWebAssets` on the host builder in *Program.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStaticWebAssets();
                webBuilder.UseStartup<Startup>();
            });
}
```

<span data-ttu-id="a4f6d-174">Quando si esegue un'app dall'output pubblicato (`dotnet publish`), non è necessario chiamare `UseStaticWebAssets`.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-174">Calling `UseStaticWebAssets` isn't required when running an app from published output (`dotnet publish`).</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="a4f6d-175">Flusso di sviluppo per più progetti</span><span class="sxs-lookup"><span data-stu-id="a4f6d-175">Multi-project development flow</span></span>

<span data-ttu-id="a4f6d-176">Quando viene eseguita l'app consumer:</span><span class="sxs-lookup"><span data-stu-id="a4f6d-176">When the consuming app runs:</span></span>

* <span data-ttu-id="a4f6d-177">Gli asset nel RCL rimanere nelle cartelle originali.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-177">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="a4f6d-178">Gli asset non vengono spostati nell'app consumer.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-178">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="a4f6d-179">Tutte le modifiche apportate all'interno della cartella *wwwroot* di RCL vengono riflesse nell'app consumer dopo che il RCL viene ricompilato e senza ricompilare l'app consumer.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-179">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="a4f6d-180">Quando viene compilato il RCL, viene generato un manifesto che descrive i percorsi di asset Web statici.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-180">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="a4f6d-181">L'app consumer legge il manifesto in fase di esecuzione per usare gli asset dei progetti e dei pacchetti a cui si fa riferimento.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-181">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="a4f6d-182">Quando si aggiunge un nuovo asset a un RCL, è necessario ricompilare il RCL per aggiornare il manifesto prima che un'app che utilizza possa accedere al nuovo asset.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-182">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="a4f6d-183">Pubblica</span><span class="sxs-lookup"><span data-stu-id="a4f6d-183">Publish</span></span>

<span data-ttu-id="a4f6d-184">Quando viene pubblicata l'app, gli asset complementari di tutti i progetti e i pacchetti a cui viene fatto riferimento vengono copiati nella cartella *wwwroot* dell'app pubblicata in `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-184">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a4f6d-185">Le visualizzazioni Razor, le pagine, i controller, i modelli di pagina, i [componenti Razor](xref:blazor/class-libraries), i [componenti di visualizzazione](xref:mvc/views/view-components)e i modelli di dati possono essere incorporati in una libreria di classi Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="a4f6d-185">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="a4f6d-186">La libreria di classi Razor può essere inclusa nel pacchetto e usata nuovamente.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-186">The RCL can be packaged and reused.</span></span> <span data-ttu-id="a4f6d-187">Le applicazioni possono includere la libreria di classi Razor ed eseguire l'override delle visualizzazioni e pagine in essa contenute.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-187">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="a4f6d-188">Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-188">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="a4f6d-189">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a4f6d-189">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="a4f6d-190">Creare una libreria di classi contenente l'interfaccia utente Razor</span><span class="sxs-lookup"><span data-stu-id="a4f6d-190">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4f6d-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4f6d-191">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a4f6d-192">Scegliere **nuovo** > **progetto**dal menu **file** di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-192">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="a4f6d-193">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-193">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="a4f6d-194">Assegnare un nome di libreria (ad esempio, "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-194">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="a4f6d-195">Per evitare un conflitto di nomi di file con la libreria di visualizzazione generata, verificare che il nome della libreria non finisca per `.Views`.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-195">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="a4f6d-196">Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-196">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="a4f6d-197">Selezionare la **libreria di classi Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-197">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="a4f6d-198">Un RCL ha il seguente file di progetto:</span><span class="sxs-lookup"><span data-stu-id="a4f6d-198">An RCL has the following project file:</span></span>

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a4f6d-199">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="a4f6d-199">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a4f6d-200">Eseguire `dotnet new razorclasslib` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-200">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="a4f6d-201">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="a4f6d-201">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="a4f6d-202">Per altre informazioni, vedere [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="a4f6d-202">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="a4f6d-203">Per evitare un conflitto di nomi di file con la libreria di visualizzazione generata, verificare che il nome della libreria non finisca per `.Views`.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-203">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="a4f6d-204">Aggiungere i file Razor alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-204">Add Razor files to the RCL.</span></span>

<span data-ttu-id="a4f6d-205">I modelli di ASP.NET Core presuppongono che il contenuto RCL si trovi nella cartella *aree* .</span><span class="sxs-lookup"><span data-stu-id="a4f6d-205">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="a4f6d-206">Vedere [layout di pagine RCL](#rcl-pages-layout) per creare un RCL che espone il contenuto in `~/Pages` anziché `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-206">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="a4f6d-207">Contenuto RCL di riferimento</span><span class="sxs-lookup"><span data-stu-id="a4f6d-207">Reference RCL content</span></span>

<span data-ttu-id="a4f6d-208">Il riferimento alla libreria di classi Razor può essere eseguito da:</span><span class="sxs-lookup"><span data-stu-id="a4f6d-208">The RCL can be referenced by:</span></span>

* <span data-ttu-id="a4f6d-209">Pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-209">NuGet package.</span></span> <span data-ttu-id="a4f6d-210">Vedere [Creazione di pacchetti NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) e [Creare e pubblicare un pacchetto NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="a4f6d-210">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="a4f6d-211">*{ProjectName}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-211">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="a4f6d-212">Vedere [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="a4f6d-212">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="a4f6d-213">Procedura dettagliata: creare un progetto RCL e usarlo da un progetto Razor Pages</span><span class="sxs-lookup"><span data-stu-id="a4f6d-213">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="a4f6d-214">È possibile scaricare il [progetto completo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) e testarlo anziché crearlo.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-214">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="a4f6d-215">Il download di esempio contiene codice aggiuntivo e collegamenti che rendono più semplice testare il progetto.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-215">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="a4f6d-216">È possibile lasciare commenti e suggerimenti in [questa discussione su GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6098) con i commenti sui download di esempio rispetto alle istruzioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-216">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="a4f6d-217">Testare l'app di download</span><span class="sxs-lookup"><span data-stu-id="a4f6d-217">Test the download app</span></span>

<span data-ttu-id="a4f6d-218">Se non è stata scaricata l'app completa e si vuole invece creare il progetto della procedura dettagliata, passare alla [prossima sezione](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="a4f6d-218">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4f6d-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4f6d-219">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a4f6d-220">Aprire il file con estensione *sln* in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-220">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="a4f6d-221">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-221">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a4f6d-222">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="a4f6d-222">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a4f6d-223">Al prompt dei comandi nella directory *cli*, compilare la libreria di classi Razor e l'app Web.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-223">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="a4f6d-224">Passare alla directory *WebApp1* ed eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="a4f6d-224">Move to the *WebApp1* directory and run the app:</span></span>

```dotnetcli
dotnet run
```

---

<span data-ttu-id="a4f6d-225">Seguire le istruzioni indicate in [Testare WebApp1](#test-webapp1)</span><span class="sxs-lookup"><span data-stu-id="a4f6d-225">Follow the instructions in [Test WebApp1](#test-webapp1)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="a4f6d-226">Creare un RCL</span><span class="sxs-lookup"><span data-stu-id="a4f6d-226">Create an RCL</span></span>

<span data-ttu-id="a4f6d-227">In questa sezione viene creato un RCL.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-227">In this section, an RCL is created.</span></span> <span data-ttu-id="a4f6d-228">I file Razor vengono aggiunti alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-228">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4f6d-229">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4f6d-229">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a4f6d-230">Creare il progetto di libreria di classi Razor:</span><span class="sxs-lookup"><span data-stu-id="a4f6d-230">Create the RCL project:</span></span>

* <span data-ttu-id="a4f6d-231">Scegliere **nuovo** > **progetto**dal menu **file** di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-231">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="a4f6d-232">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-232">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="a4f6d-233">Denominare l'app **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-233">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="a4f6d-234">Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-234">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="a4f6d-235">Selezionare la **libreria di classi Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-235">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="a4f6d-236">Aggiungere un file di visualizzazione parziale Razor denominato *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-236">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a4f6d-237">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="a4f6d-237">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a4f6d-238">Eseguire il comando seguente dalla riga di comando:</span><span class="sxs-lookup"><span data-stu-id="a4f6d-238">From the command line, run the following:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="a4f6d-239">I comandi precedenti:</span><span class="sxs-lookup"><span data-stu-id="a4f6d-239">The preceding commands:</span></span>

* <span data-ttu-id="a4f6d-240">Crea l'`RazorUIClassLib` RCL.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-240">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="a4f6d-241">Crea una pagina Razor _Message e la aggiunge alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-241">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="a4f6d-242">Il parametro `-np` crea la pagina senza un `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-242">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="a4f6d-243">Crea un file [_ViewStart. cshtml](xref:mvc/views/layout#running-code-before-each-view) e lo aggiunge al RCL.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-243">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="a4f6d-244">Il file *_ViewStart. cshtml* è necessario per usare il layout del progetto Razor Pages (che viene aggiunto nella sezione successiva).</span><span class="sxs-lookup"><span data-stu-id="a4f6d-244">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="a4f6d-245">Aggiungere i file Razor e cartelle per il progetto</span><span class="sxs-lookup"><span data-stu-id="a4f6d-245">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="a4f6d-246">Sostituire il markup *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a4f6d-246">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="a4f6d-247">Sostituire il markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a4f6d-247">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  <span data-ttu-id="a4f6d-248">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` è necessario per usare la visualizzazione parziale (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="a4f6d-248">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="a4f6d-249">Anziché includere la direttiva `@addTagHelper`, è possibile aggiungere un file *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-249">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="a4f6d-250">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="a4f6d-250">For example:</span></span>

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  <span data-ttu-id="a4f6d-251">Per altre informazioni su *_ViewImports. cshtml*, vedere [importazione di direttive condivise](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="a4f6d-251">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="a4f6d-252">Compilare la libreria di classi per verificare che non siano presenti errori del compilatore:</span><span class="sxs-lookup"><span data-stu-id="a4f6d-252">Build the class library to verify there are no compiler errors:</span></span>

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

<span data-ttu-id="a4f6d-253">L'output di compilazione contiene *RazorUIClassLib.dll* e *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-253">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="a4f6d-254">*RazorUIClassLib.Views.dll* contiene il contenuto Razor compilato.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-254">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="a4f6d-255">Usare la libreria dell'interfaccia utente Razor da un progetto Razor Pages</span><span class="sxs-lookup"><span data-stu-id="a4f6d-255">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4f6d-256">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4f6d-256">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a4f6d-257">Creare l'app Web di Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="a4f6d-257">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="a4f6d-258">Da **Esplora soluzioni**, fare clic con il pulsante destro del mouse sulla soluzione > **Aggiungi** >**nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-258">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="a4f6d-259">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-259">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="a4f6d-260">Denominare l'app **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-260">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="a4f6d-261">Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-261">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="a4f6d-262">Selezionare **applicazione Web** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-262">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="a4f6d-263">In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **WebApp1** e selezionare **Imposta come progetto di avvio**.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-263">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="a4f6d-264">Da **Esplora soluzioni**, fare clic con il pulsante destro del mouse su **app Web 1** e scegliere **dipendenze di compilazione** > **Dipendenze progetto**.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-264">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="a4f6d-265">Selezionare **RazorUIClassLib** come una dipendenza di **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-265">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="a4f6d-266">Da **Esplora soluzioni**, fare clic con il pulsante destro del mouse su **app Web 1** e scegliere **Aggiungi** > **riferimento**.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-266">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="a4f6d-267">Nella finestra di dialogo **Gestione riferimenti** selezionare **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-267">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="a4f6d-268">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-268">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a4f6d-269">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="a4f6d-269">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a4f6d-270">Creare un'app Web Razor Pages e un file di soluzione contenente l'app Razor Pages e il RCL:</span><span class="sxs-lookup"><span data-stu-id="a4f6d-270">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="a4f6d-271">Compilare ed eseguire l'app Web:</span><span class="sxs-lookup"><span data-stu-id="a4f6d-271">Build and run the web app:</span></span>

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a><span data-ttu-id="a4f6d-272">Testare WebApp1</span><span class="sxs-lookup"><span data-stu-id="a4f6d-272">Test WebApp1</span></span>

<span data-ttu-id="a4f6d-273">Passare a `/MyFeature/Page1` per verificare che sia in uso la libreria di classi dell'interfaccia utente Razor.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-273">Browse to `/MyFeature/Page1` to verify that the Razor UI class library is in use.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="a4f6d-274">Eseguire l'override di visualizzazioni, visualizzazioni parziali e pagine</span><span class="sxs-lookup"><span data-stu-id="a4f6d-274">Override views, partial views, and pages</span></span>

<span data-ttu-id="a4f6d-275">Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-275">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="a4f6d-276">Ad esempio, aggiungere *app Web 1/areas/la funzionalità/Pages/Pagina1. cshtml* a app Web 1 e Pagina1 in app Web 1 avrà la precedenza su PAGINA1 in RCL.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-276">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="a4f6d-277">Nel download di esempio, rinominare *WebApp1/aree/MyFeature2* in *WebApp1/aree/MyFeature* per testare la precedenza.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-277">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="a4f6d-278">Copiare la visualizzazione parziali *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* in *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-278">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="a4f6d-279">Aggiornare il markup per indicare la nuova posizione.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-279">Update the markup to indicate the new location.</span></span> <span data-ttu-id="a4f6d-280">Compilare ed eseguire l'app per verificare quale versione del parziale sia in uso.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-280">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="a4f6d-281">Layout delle pagine RCL</span><span class="sxs-lookup"><span data-stu-id="a4f6d-281">RCL Pages layout</span></span>

<span data-ttu-id="a4f6d-282">Per fare riferimento al contenuto RCL come se facesse parte della cartella *pagine* dell'app Web, creare il progetto RCL con la struttura di file seguente:</span><span class="sxs-lookup"><span data-stu-id="a4f6d-282">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="a4f6d-283">*RazorUIClassLib/pagine*</span><span class="sxs-lookup"><span data-stu-id="a4f6d-283">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="a4f6d-284">*RazorUIClassLib/pagine/condiviso*</span><span class="sxs-lookup"><span data-stu-id="a4f6d-284">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="a4f6d-285">Si supponga che *RazorUIClassLib/Pages/Shared* contenga due file parziali: *_Header. cshtml* e *_Footer. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a4f6d-285">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="a4f6d-286">È possibile aggiungere i tag `<partial>` al file *_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="a4f6d-286">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="a4f6d-287">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="a4f6d-287">Additional resources</span></span>

* <xref:blazor/class-libraries>
