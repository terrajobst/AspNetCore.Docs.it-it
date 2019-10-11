---
title: Interfaccia utente Razor riutilizzabile nelle librerie di classi con ASP.NET Core
author: Rick-Anderson
description: Viene illustrato come creare un'interfaccia Razor riutilizzabili tramite le visualizzazioni parziali in una libreria di classi in ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 10/08/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: d656e924033f1b217cdd8c86f7d00411c5d71beb
ms.sourcegitcommit: 73a451e9a58ac7102f90b608d661d8c23dd9bbaf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/08/2019
ms.locfileid: "72037579"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="4adfe-103">Creare un'interfaccia utente riutilizzabile usando il progetto libreria di classi Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4adfe-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="4adfe-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4adfe-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4adfe-105">Le visualizzazioni Razor, le pagine, i controller, i modelli di pagina, i [componenti Razor](xref:blazor/class-libraries), i [componenti di visualizzazione](xref:mvc/views/view-components)e i modelli di dati possono essere incorporati in una libreria di classi Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="4adfe-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="4adfe-106">La libreria di classi Razor può essere inclusa nel pacchetto e usata nuovamente.</span><span class="sxs-lookup"><span data-stu-id="4adfe-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="4adfe-107">Le applicazioni possono includere la libreria di classi Razor ed eseguire l'override delle visualizzazioni e pagine in essa contenute.</span><span class="sxs-lookup"><span data-stu-id="4adfe-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="4adfe-108">Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="4adfe-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="4adfe-109">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4adfe-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="4adfe-110">Creare una libreria di classi contenente l'interfaccia utente Razor</span><span class="sxs-lookup"><span data-stu-id="4adfe-110">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4adfe-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4adfe-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4adfe-112">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="4adfe-112">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="4adfe-113">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="4adfe-113">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="4adfe-114">Assegnare un nome di libreria (ad esempio, "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="4adfe-114">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="4adfe-115">Per evitare un conflitto di nomi di file con la libreria di visualizzazione generata, verificare che il nome della libreria non finisca per `.Views`.</span><span class="sxs-lookup"><span data-stu-id="4adfe-115">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="4adfe-116">Verificare che sia selezionato **ASP.NET Core 3,0** o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="4adfe-116">Verify **ASP.NET Core 3.0** or later is selected.</span></span>
* <span data-ttu-id="4adfe-117">Selezionare la **libreria di classi Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="4adfe-117">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="4adfe-118">Per impostazione predefinita, il modello RCL (Razor Class Library) per impostazione predefinita è lo sviluppo di componenti Razor.</span><span class="sxs-lookup"><span data-stu-id="4adfe-118">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="4adfe-119">Un'opzione di modello in Visual Studio fornisce il supporto del modello per le pagine e le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="4adfe-119">A template option in Visual Studio provides template support for pages and views.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4adfe-120">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="4adfe-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4adfe-121">Eseguire `dotnet new razorclasslib` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="4adfe-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="4adfe-122">Esempio:</span><span class="sxs-lookup"><span data-stu-id="4adfe-122">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="4adfe-123">Per impostazione predefinita, il modello RCL (Razor Class Library) per impostazione predefinita è lo sviluppo di componenti Razor.</span><span class="sxs-lookup"><span data-stu-id="4adfe-123">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="4adfe-124">Passare l'opzione `--support-pages-and-views` (`dotnet new razorclasslib --support-pages-and-views`) per fornire il supporto per le pagine e le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="4adfe-124">Pass the `--support-pages-and-views` option (`dotnet new razorclasslib --support-pages-and-views`) to provide support for pages and views.</span></span>

<span data-ttu-id="4adfe-125">Per altre informazioni, vedere [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="4adfe-125">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="4adfe-126">Per evitare un conflitto di nomi di file con la libreria di visualizzazione generata, verificare che il nome della libreria non finisca per `.Views`.</span><span class="sxs-lookup"><span data-stu-id="4adfe-126">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="4adfe-127">Aggiungere i file Razor alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="4adfe-127">Add Razor files to the RCL.</span></span>

<span data-ttu-id="4adfe-128">I modelli ASP.NET Core presuppongono il contenuto RCL sia impostato il *aree* cartella.</span><span class="sxs-lookup"><span data-stu-id="4adfe-128">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="4adfe-129">Vedere [layout di pagine RCL](#rcl-pages-layout) per creare un RCL che espone il contenuto in `~/Pages` anziché `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="4adfe-129">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="4adfe-130">Contenuto RCL di riferimento</span><span class="sxs-lookup"><span data-stu-id="4adfe-130">Reference RCL content</span></span>

<span data-ttu-id="4adfe-131">Il riferimento alla libreria di classi Razor può essere eseguito da:</span><span class="sxs-lookup"><span data-stu-id="4adfe-131">The RCL can be referenced by:</span></span>

* <span data-ttu-id="4adfe-132">Pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="4adfe-132">NuGet package.</span></span> <span data-ttu-id="4adfe-133">Vedere [Creazione di pacchetti NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) e [Creare e pubblicare un pacchetto NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="4adfe-133">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="4adfe-134">*{ProjectName}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="4adfe-134">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="4adfe-135">Vedere [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="4adfe-135">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="4adfe-136">Eseguire l'override di visualizzazioni, visualizzazioni parziali e pagine</span><span class="sxs-lookup"><span data-stu-id="4adfe-136">Override views, partial views, and pages</span></span>

<span data-ttu-id="4adfe-137">Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="4adfe-137">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="4adfe-138">Ad esempio, aggiungere *app Web 1/areas/la funzionalità/Pages/Pagina1. cshtml* a app Web 1 e Pagina1 in app Web 1 avrà la precedenza su PAGINA1 in RCL.</span><span class="sxs-lookup"><span data-stu-id="4adfe-138">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="4adfe-139">Nel download di esempio, rinominare *WebApp1/aree/MyFeature2* in *WebApp1/aree/MyFeature* per testare la precedenza.</span><span class="sxs-lookup"><span data-stu-id="4adfe-139">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="4adfe-140">Copiare la visualizzazione parziali *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* in *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4adfe-140">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="4adfe-141">Aggiornare il markup per indicare la nuova posizione.</span><span class="sxs-lookup"><span data-stu-id="4adfe-141">Update the markup to indicate the new location.</span></span> <span data-ttu-id="4adfe-142">Compilare ed eseguire l'app per verificare quale versione del parziale sia in uso.</span><span class="sxs-lookup"><span data-stu-id="4adfe-142">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="4adfe-143">Layout delle pagine RCL</span><span class="sxs-lookup"><span data-stu-id="4adfe-143">RCL Pages layout</span></span>

<span data-ttu-id="4adfe-144">Per riferimento come se facesse parte dell'app web di contenuto RCL *pagine* cartella, creare il progetto RCL con la struttura file seguente:</span><span class="sxs-lookup"><span data-stu-id="4adfe-144">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="4adfe-145">*RazorUIClassLib o delle pagine*</span><span class="sxs-lookup"><span data-stu-id="4adfe-145">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="4adfe-146">*RazorUIClassLib/pagine/Shared*</span><span class="sxs-lookup"><span data-stu-id="4adfe-146">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="4adfe-147">Si supponga *RazorUIClassLib/pagine/Shared* contiene due file parziali: *_Header.cshtml* e *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4adfe-147">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="4adfe-148">Il `<partial>` tag può essere aggiunto al *layout. cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="4adfe-148">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="4adfe-149">Creare un RCL con asset statici</span><span class="sxs-lookup"><span data-stu-id="4adfe-149">Create an RCL with static assets</span></span>

<span data-ttu-id="4adfe-150">Un RCL può richiedere risorse statiche complementari a cui è possibile fare riferimento dall'app consumer di RCL.</span><span class="sxs-lookup"><span data-stu-id="4adfe-150">An RCL may require companion static assets that can be referenced by the consuming app of the RCL.</span></span> <span data-ttu-id="4adfe-151">ASP.NET Core consente di creare RCL che includono risorse statiche disponibili per un'app di consumo.</span><span class="sxs-lookup"><span data-stu-id="4adfe-151">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="4adfe-152">Per includere asset complementari come parte di un RCL, creare una cartella *wwwroot* nella libreria di classi e includere tutti i file necessari in tale cartella.</span><span class="sxs-lookup"><span data-stu-id="4adfe-152">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="4adfe-153">Quando si esegue la compressione di un RCL, tutti gli asset complementari nella cartella *wwwroot* vengono inclusi automaticamente nel pacchetto.</span><span class="sxs-lookup"><span data-stu-id="4adfe-153">When packing an RCL, all companion assets in the *wwwroot* folder are automatically included in the package.</span></span>

### <a name="exclude-static-assets"></a><span data-ttu-id="4adfe-154">Escludi asset statici</span><span class="sxs-lookup"><span data-stu-id="4adfe-154">Exclude static assets</span></span>

<span data-ttu-id="4adfe-155">Per escludere asset statici, aggiungere il percorso di esclusione desiderato al gruppo di proprietà `$(DefaultItemExcludes)` nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="4adfe-155">To exclude static assets, add the desired exclusion path to the `$(DefaultItemExcludes)` property group in the project file.</span></span> <span data-ttu-id="4adfe-156">Separa le voci con un punto e virgola (`;`).</span><span class="sxs-lookup"><span data-stu-id="4adfe-156">Separate entries with a semicolon (`;`).</span></span>

<span data-ttu-id="4adfe-157">Nell'esempio seguente, il foglio di stile *lib. CSS* nella cartella *wwwroot* non è considerato un asset statico e non è incluso nel RCL pubblicato:</span><span class="sxs-lookup"><span data-stu-id="4adfe-157">In the following example, the *lib.css* stylesheet in the *wwwroot* folder isn't considered a static asset and isn't included in the published RCL:</span></span>

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a><span data-ttu-id="4adfe-158">Integrazione typescript</span><span class="sxs-lookup"><span data-stu-id="4adfe-158">Typescript integration</span></span>

<span data-ttu-id="4adfe-159">Per includere i file TypeScript in un RCL:</span><span class="sxs-lookup"><span data-stu-id="4adfe-159">To include TypeScript files in an RCL:</span></span>

1. <span data-ttu-id="4adfe-160">Inserire i file TypeScript ( *. TS*) all'esterno della cartella *wwwroot*</span><span class="sxs-lookup"><span data-stu-id="4adfe-160">Place the TypeScript files (*.ts*) outside of the *wwwroot* folder.</span></span> <span data-ttu-id="4adfe-161">Inserire, ad esempio, i file in una cartella *client* .</span><span class="sxs-lookup"><span data-stu-id="4adfe-161">For example, place the files in a *Client* folder.</span></span>

1. <span data-ttu-id="4adfe-162">Configurare l'output di compilazione TypeScript per la cartella *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="4adfe-162">Configure the TypeScript build output for the *wwwroot* folder.</span></span> <span data-ttu-id="4adfe-163">Impostare la proprietà `TypescriptOutDir` all'interno di un `PropertyGroup` nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="4adfe-163">Set the `TypescriptOutDir` property inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. <span data-ttu-id="4adfe-164">Includere la destinazione TypeScript come dipendenza della destinazione `ResolveCurrentProjectStaticWebAssets` aggiungendo la destinazione seguente all'interno di un `PropertyGroup` nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="4adfe-164">Include the TypeScript target as a dependency of the `ResolveCurrentProjectStaticWebAssets` target by adding the following target inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     TypeScriptCompile;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="4adfe-165">Utilizzare il contenuto da un RCL a cui si fa riferimento</span><span class="sxs-lookup"><span data-stu-id="4adfe-165">Consume content from a referenced RCL</span></span>

<span data-ttu-id="4adfe-166">I file inclusi nella cartella *wwwroot* di RCL vengono esposti all'app consumer nel prefisso `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="4adfe-166">The files included in the *wwwroot* folder of the RCL are exposed to the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="4adfe-167">Ad esempio, una libreria denominata *Razor. Class. lib* genera un percorso del contenuto statico in `_content/Razor.Class.Lib/`.</span><span class="sxs-lookup"><span data-stu-id="4adfe-167">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span>

<span data-ttu-id="4adfe-168">L'app consumer fa riferimento a risorse statiche fornite dalla libreria con `<script>`, `<style>`, `<img>` e altri tag HTML.</span><span class="sxs-lookup"><span data-stu-id="4adfe-168">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="4adfe-169">Per l'app consumer deve essere abilitato il [supporto dei file statici](xref:fundamentals/static-files) in `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="4adfe-169">The consuming app must have [static file support](xref:fundamentals/static-files) enabled in `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="4adfe-170">Quando si esegue l'app consumer dall'output di compilazione (`dotnet run`), gli asset Web statici sono abilitati per impostazione predefinita nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="4adfe-170">When running the consuming app from build output (`dotnet run`), static web assets are enabled by default in the Development environment.</span></span> <span data-ttu-id="4adfe-171">Per supportare asset in altri ambienti durante l'esecuzione dall'output di compilazione, chiamare `UseStaticWebAssets` nel generatore host in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="4adfe-171">To support assets in other environments when running from build output, call `UseStaticWebAssets` on the host builder in *Program.cs*:</span></span>

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

<span data-ttu-id="4adfe-172">Quando si esegue un'app dall'output pubblicato (`dotnet publish`), non è necessario chiamare `UseStaticWebAssets`.</span><span class="sxs-lookup"><span data-stu-id="4adfe-172">Calling `UseStaticWebAssets` isn't required when running an app from published output (`dotnet publish`).</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="4adfe-173">Flusso di sviluppo per più progetti</span><span class="sxs-lookup"><span data-stu-id="4adfe-173">Multi-project development flow</span></span>

<span data-ttu-id="4adfe-174">Quando viene eseguita l'app consumer:</span><span class="sxs-lookup"><span data-stu-id="4adfe-174">When the consuming app runs:</span></span>

* <span data-ttu-id="4adfe-175">Gli asset nel RCL rimanere nelle cartelle originali.</span><span class="sxs-lookup"><span data-stu-id="4adfe-175">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="4adfe-176">Gli asset non vengono spostati nell'app consumer.</span><span class="sxs-lookup"><span data-stu-id="4adfe-176">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="4adfe-177">Tutte le modifiche apportate all'interno della cartella *wwwroot* di RCL vengono riflesse nell'app consumer dopo che il RCL viene ricompilato e senza ricompilare l'app consumer.</span><span class="sxs-lookup"><span data-stu-id="4adfe-177">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="4adfe-178">Quando viene compilato il RCL, viene generato un manifesto che descrive i percorsi di asset Web statici.</span><span class="sxs-lookup"><span data-stu-id="4adfe-178">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="4adfe-179">L'app consumer legge il manifesto in fase di esecuzione per usare gli asset dei progetti e dei pacchetti a cui si fa riferimento.</span><span class="sxs-lookup"><span data-stu-id="4adfe-179">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="4adfe-180">Quando si aggiunge un nuovo asset a un RCL, è necessario ricompilare il RCL per aggiornare il manifesto prima che un'app che utilizza possa accedere al nuovo asset.</span><span class="sxs-lookup"><span data-stu-id="4adfe-180">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="4adfe-181">Pubblica</span><span class="sxs-lookup"><span data-stu-id="4adfe-181">Publish</span></span>

<span data-ttu-id="4adfe-182">Quando viene pubblicata l'app, gli asset complementari di tutti i progetti e i pacchetti a cui viene fatto riferimento vengono copiati nella cartella *wwwroot* dell'app pubblicata in `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="4adfe-182">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="4adfe-183">Le visualizzazioni Razor, le pagine, i controller, i modelli di pagina, i [componenti Razor](xref:blazor/class-libraries), i [componenti di visualizzazione](xref:mvc/views/view-components)e i modelli di dati possono essere incorporati in una libreria di classi Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="4adfe-183">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="4adfe-184">La libreria di classi Razor può essere inclusa nel pacchetto e usata nuovamente.</span><span class="sxs-lookup"><span data-stu-id="4adfe-184">The RCL can be packaged and reused.</span></span> <span data-ttu-id="4adfe-185">Le applicazioni possono includere la libreria di classi Razor ed eseguire l'override delle visualizzazioni e pagine in essa contenute.</span><span class="sxs-lookup"><span data-stu-id="4adfe-185">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="4adfe-186">Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="4adfe-186">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="4adfe-187">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4adfe-187">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="4adfe-188">Creare una libreria di classi contenente l'interfaccia utente Razor</span><span class="sxs-lookup"><span data-stu-id="4adfe-188">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4adfe-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4adfe-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4adfe-190">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="4adfe-190">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="4adfe-191">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="4adfe-191">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="4adfe-192">Assegnare un nome di libreria (ad esempio, "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="4adfe-192">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="4adfe-193">Per evitare un conflitto di nomi di file con la libreria di visualizzazione generata, verificare che il nome della libreria non finisca per `.Views`.</span><span class="sxs-lookup"><span data-stu-id="4adfe-193">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="4adfe-194">Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="4adfe-194">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="4adfe-195">Selezionare la **libreria di classi Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="4adfe-195">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="4adfe-196">Un RCL ha il seguente file di progetto:</span><span class="sxs-lookup"><span data-stu-id="4adfe-196">An RCL has the following project file:</span></span>

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4adfe-197">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="4adfe-197">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4adfe-198">Eseguire `dotnet new razorclasslib` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="4adfe-198">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="4adfe-199">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4adfe-199">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="4adfe-200">Per altre informazioni, vedere [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="4adfe-200">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="4adfe-201">Per evitare un conflitto di nomi di file con la libreria di visualizzazione generata, verificare che il nome della libreria non finisca per `.Views`.</span><span class="sxs-lookup"><span data-stu-id="4adfe-201">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="4adfe-202">Aggiungere i file Razor alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="4adfe-202">Add Razor files to the RCL.</span></span>

<span data-ttu-id="4adfe-203">I modelli ASP.NET Core presuppongono il contenuto RCL sia impostato il *aree* cartella.</span><span class="sxs-lookup"><span data-stu-id="4adfe-203">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="4adfe-204">Vedere [layout di pagine RCL](#rcl-pages-layout) per creare un RCL che espone il contenuto in `~/Pages` anziché `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="4adfe-204">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="4adfe-205">Contenuto RCL di riferimento</span><span class="sxs-lookup"><span data-stu-id="4adfe-205">Reference RCL content</span></span>

<span data-ttu-id="4adfe-206">Il riferimento alla libreria di classi Razor può essere eseguito da:</span><span class="sxs-lookup"><span data-stu-id="4adfe-206">The RCL can be referenced by:</span></span>

* <span data-ttu-id="4adfe-207">Pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="4adfe-207">NuGet package.</span></span> <span data-ttu-id="4adfe-208">Vedere [Creazione di pacchetti NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) e [Creare e pubblicare un pacchetto NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="4adfe-208">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="4adfe-209">*{ProjectName}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="4adfe-209">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="4adfe-210">Vedere [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="4adfe-210">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="4adfe-211">Procedura dettagliata: Creare un progetto RCL e usarlo da un progetto Razor Pages</span><span class="sxs-lookup"><span data-stu-id="4adfe-211">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="4adfe-212">È possibile scaricare il [progetto completo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) e testarlo anziché crearlo.</span><span class="sxs-lookup"><span data-stu-id="4adfe-212">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="4adfe-213">Il download di esempio contiene codice aggiuntivo e collegamenti che rendono più semplice testare il progetto.</span><span class="sxs-lookup"><span data-stu-id="4adfe-213">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="4adfe-214">È possibile lasciare commenti e suggerimenti in [questa discussione su GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6098) con i commenti sui download di esempio rispetto alle istruzioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="4adfe-214">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="4adfe-215">Testare l'app di download</span><span class="sxs-lookup"><span data-stu-id="4adfe-215">Test the download app</span></span>

<span data-ttu-id="4adfe-216">Se non è stata scaricata l'app completa e si vuole invece creare il progetto della procedura dettagliata, passare alla [prossima sezione](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="4adfe-216">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4adfe-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4adfe-217">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4adfe-218">Aprire il file con estensione *sln* in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4adfe-218">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="4adfe-219">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="4adfe-219">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4adfe-220">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="4adfe-220">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4adfe-221">Al prompt dei comandi nella directory *cli*, compilare la libreria di classi Razor e l'app Web.</span><span class="sxs-lookup"><span data-stu-id="4adfe-221">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="4adfe-222">Passare alla directory *WebApp1* ed eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="4adfe-222">Move to the *WebApp1* directory and run the app:</span></span>

```dotnetcli
dotnet run
```

---

<span data-ttu-id="4adfe-223">Seguire le istruzioni indicate in [Testare WebApp1](#test-webapp1)</span><span class="sxs-lookup"><span data-stu-id="4adfe-223">Follow the instructions in [Test WebApp1](#test-webapp1)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="4adfe-224">Creare un RCL</span><span class="sxs-lookup"><span data-stu-id="4adfe-224">Create an RCL</span></span>

<span data-ttu-id="4adfe-225">In questa sezione viene creato un RCL.</span><span class="sxs-lookup"><span data-stu-id="4adfe-225">In this section, an RCL is created.</span></span> <span data-ttu-id="4adfe-226">I file Razor vengono aggiunti alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="4adfe-226">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4adfe-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4adfe-227">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4adfe-228">Creare il progetto di libreria di classi Razor:</span><span class="sxs-lookup"><span data-stu-id="4adfe-228">Create the RCL project:</span></span>

* <span data-ttu-id="4adfe-229">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="4adfe-229">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="4adfe-230">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="4adfe-230">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="4adfe-231">Denominare l'app **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="4adfe-231">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="4adfe-232">Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="4adfe-232">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="4adfe-233">Selezionare la **libreria di classi Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="4adfe-233">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="4adfe-234">Aggiungere un file di visualizzazione parziale Razor denominato *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4adfe-234">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4adfe-235">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="4adfe-235">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4adfe-236">Eseguire il comando seguente dalla riga di comando:</span><span class="sxs-lookup"><span data-stu-id="4adfe-236">From the command line, run the following:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="4adfe-237">I comandi precedenti:</span><span class="sxs-lookup"><span data-stu-id="4adfe-237">The preceding commands:</span></span>

* <span data-ttu-id="4adfe-238">Crea il RCL `RazorUIClassLib`.</span><span class="sxs-lookup"><span data-stu-id="4adfe-238">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="4adfe-239">Crea una pagina Razor _Message e la aggiunge alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="4adfe-239">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="4adfe-240">Il parametro `-np` crea la pagina senza un `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="4adfe-240">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="4adfe-241">Crea una [viewstart. cshtml](xref:mvc/views/layout#running-code-before-each-view) file e lo aggiunge al RCL.</span><span class="sxs-lookup"><span data-stu-id="4adfe-241">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="4adfe-242">Il *viewstart. cshtml* file è necessario per usare il layout del progetto Razor Pages (che viene aggiunto nella sezione successiva).</span><span class="sxs-lookup"><span data-stu-id="4adfe-242">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="4adfe-243">Aggiungere i file Razor e cartelle per il progetto</span><span class="sxs-lookup"><span data-stu-id="4adfe-243">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="4adfe-244">Sostituire il markup *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4adfe-244">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="4adfe-245">Sostituire il markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4adfe-245">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  <span data-ttu-id="4adfe-246">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` è necessario per usare la visualizzazione parziale (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="4adfe-246">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="4adfe-247">Anziché includere la direttiva `@addTagHelper`, è possibile aggiungere un file *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4adfe-247">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="4adfe-248">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4adfe-248">For example:</span></span>

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  <span data-ttu-id="4adfe-249">Per ulteriori informazioni sul *viewimports. cshtml*, vedere [importazione di direttive condivise](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="4adfe-249">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="4adfe-250">Compilare la libreria di classi per verificare che non siano presenti errori del compilatore:</span><span class="sxs-lookup"><span data-stu-id="4adfe-250">Build the class library to verify there are no compiler errors:</span></span>

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

<span data-ttu-id="4adfe-251">L'output di compilazione contiene *RazorUIClassLib.dll* e *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="4adfe-251">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="4adfe-252">*RazorUIClassLib.Views.dll* contiene il contenuto Razor compilato.</span><span class="sxs-lookup"><span data-stu-id="4adfe-252">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="4adfe-253">Usare la libreria dell'interfaccia utente Razor da un progetto Razor Pages</span><span class="sxs-lookup"><span data-stu-id="4adfe-253">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4adfe-254">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4adfe-254">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4adfe-255">Creare l'app Web di Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="4adfe-255">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="4adfe-256">Da **Esplora soluzioni**, fare clic con il pulsante destro del mouse sulla soluzione > **aggiungere** >  **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="4adfe-256">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="4adfe-257">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="4adfe-257">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="4adfe-258">Denominare l'app **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="4adfe-258">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="4adfe-259">Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="4adfe-259">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="4adfe-260">Selezionare **applicazione Web** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="4adfe-260">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="4adfe-261">In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **WebApp1** e selezionare **Imposta come progetto di avvio**.</span><span class="sxs-lookup"><span data-stu-id="4adfe-261">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="4adfe-262">Da **Esplora soluzioni**, fare clic con il pulsante destro del mouse su **app Web 1** e scegliere dipendenze di **compilazione** > **Dipendenze progetto**.</span><span class="sxs-lookup"><span data-stu-id="4adfe-262">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="4adfe-263">Selezionare **RazorUIClassLib** come una dipendenza di **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="4adfe-263">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="4adfe-264">Da **Esplora soluzioni**, fare clic con il pulsante destro del mouse su **app Web 1** e scegliere **Aggiungi** > **riferimento**.</span><span class="sxs-lookup"><span data-stu-id="4adfe-264">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="4adfe-265">Nella finestra di dialogo **Gestione riferimenti** selezionare **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="4adfe-265">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="4adfe-266">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="4adfe-266">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4adfe-267">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="4adfe-267">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4adfe-268">Creare un'app Web Razor Pages e un file di soluzione contenente l'app Razor Pages e il RCL:</span><span class="sxs-lookup"><span data-stu-id="4adfe-268">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="4adfe-269">Compilare ed eseguire l'app Web:</span><span class="sxs-lookup"><span data-stu-id="4adfe-269">Build and run the web app:</span></span>

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a><span data-ttu-id="4adfe-270">Testare WebApp1</span><span class="sxs-lookup"><span data-stu-id="4adfe-270">Test WebApp1</span></span>

<span data-ttu-id="4adfe-271">Passare a `/MyFeature/Page1` per verificare che sia in uso la libreria di classi dell'interfaccia utente Razor.</span><span class="sxs-lookup"><span data-stu-id="4adfe-271">Browse to `/MyFeature/Page1` to verify that the Razor UI class library is in use.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="4adfe-272">Eseguire l'override di visualizzazioni, visualizzazioni parziali e pagine</span><span class="sxs-lookup"><span data-stu-id="4adfe-272">Override views, partial views, and pages</span></span>

<span data-ttu-id="4adfe-273">Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="4adfe-273">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="4adfe-274">Ad esempio, aggiungere *app Web 1/areas/la funzionalità/Pages/Pagina1. cshtml* a app Web 1 e Pagina1 in app Web 1 avrà la precedenza su PAGINA1 in RCL.</span><span class="sxs-lookup"><span data-stu-id="4adfe-274">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="4adfe-275">Nel download di esempio, rinominare *WebApp1/aree/MyFeature2* in *WebApp1/aree/MyFeature* per testare la precedenza.</span><span class="sxs-lookup"><span data-stu-id="4adfe-275">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="4adfe-276">Copiare la visualizzazione parziali *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* in *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4adfe-276">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="4adfe-277">Aggiornare il markup per indicare la nuova posizione.</span><span class="sxs-lookup"><span data-stu-id="4adfe-277">Update the markup to indicate the new location.</span></span> <span data-ttu-id="4adfe-278">Compilare ed eseguire l'app per verificare quale versione del parziale sia in uso.</span><span class="sxs-lookup"><span data-stu-id="4adfe-278">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="4adfe-279">Layout delle pagine RCL</span><span class="sxs-lookup"><span data-stu-id="4adfe-279">RCL Pages layout</span></span>

<span data-ttu-id="4adfe-280">Per riferimento come se facesse parte dell'app web di contenuto RCL *pagine* cartella, creare il progetto RCL con la struttura file seguente:</span><span class="sxs-lookup"><span data-stu-id="4adfe-280">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="4adfe-281">*RazorUIClassLib o delle pagine*</span><span class="sxs-lookup"><span data-stu-id="4adfe-281">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="4adfe-282">*RazorUIClassLib/pagine/Shared*</span><span class="sxs-lookup"><span data-stu-id="4adfe-282">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="4adfe-283">Si supponga *RazorUIClassLib/pagine/Shared* contiene due file parziali: *_Header.cshtml* e *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4adfe-283">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="4adfe-284">Il `<partial>` tag può essere aggiunto al *layout. cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="4adfe-284">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end
