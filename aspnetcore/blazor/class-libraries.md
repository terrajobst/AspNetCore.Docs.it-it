---
title: Librerie di classi dei componenti di ASP.NET Core Razor
author: guardrex
description: Scopri in che modo i componenti possono essere inclusi nelle app blazer da una libreria di componenti esterna.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/class-libraries
ms.openlocfilehash: 6e93d48bbc684845952c3db8935ccc8b190044b7
ms.sourcegitcommit: f5f0ff65d4e2a961939762fb00e654491a2c772a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030339"
---
# <a name="aspnet-core-razor-components-class-libraries"></a><span data-ttu-id="8e62e-103">Librerie di classi dei componenti di ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="8e62e-103">ASP.NET Core Razor components class libraries</span></span>

<span data-ttu-id="8e62e-104">Di [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="8e62e-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="8e62e-105">I componenti possono essere condivisi in una [libreria di classi Razor (RCL)](xref:razor-pages/ui-class) tra i progetti.</span><span class="sxs-lookup"><span data-stu-id="8e62e-105">Components can be shared in a [Razor class library (RCL)](xref:razor-pages/ui-class) across projects.</span></span> <span data-ttu-id="8e62e-106">È possibile includere una *libreria di classi di componenti Razor* da:</span><span class="sxs-lookup"><span data-stu-id="8e62e-106">A *Razor components class library* can be included from:</span></span>

* <span data-ttu-id="8e62e-107">Un altro progetto nella soluzione.</span><span class="sxs-lookup"><span data-stu-id="8e62e-107">Another project in the solution.</span></span>
* <span data-ttu-id="8e62e-108">Un pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="8e62e-108">A NuGet package.</span></span>
* <span data-ttu-id="8e62e-109">Libreria .NET A cui si fa riferimento.</span><span class="sxs-lookup"><span data-stu-id="8e62e-109">A referenced .NET library.</span></span>

<span data-ttu-id="8e62e-110">Così come i componenti sono tipi .NET normali, i componenti forniti da un RCL sono assembly .NET normali.</span><span class="sxs-lookup"><span data-stu-id="8e62e-110">Just as components are regular .NET types, components provided by an RCL are normal .NET assemblies.</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="8e62e-111">Creare un RCL</span><span class="sxs-lookup"><span data-stu-id="8e62e-111">Create an RCL</span></span>

<span data-ttu-id="8e62e-112">Per configurare l'ambiente per <xref:blazor/get-started> blazer, seguire le istruzioni riportate nell'articolo.</span><span class="sxs-lookup"><span data-stu-id="8e62e-112">Follow the guidance in the <xref:blazor/get-started> article to configure your environment for Blazor.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8e62e-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e62e-113">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="8e62e-114">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="8e62e-114">Create a new project.</span></span>
1. <span data-ttu-id="8e62e-115">Selezionare **libreria di classi Razor**.</span><span class="sxs-lookup"><span data-stu-id="8e62e-115">Select **Razor Class Library**.</span></span> <span data-ttu-id="8e62e-116">Selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="8e62e-116">Select **Next**.</span></span>
1. <span data-ttu-id="8e62e-117">Nella finestra di dialogo **Crea una nuova libreria di classi Razor** selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="8e62e-117">In the **Create a new Razor class library** dialog, select **Create**.</span></span>
1. <span data-ttu-id="8e62e-118">Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="8e62e-118">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="8e62e-119">Gli esempi in questo argomento usano il nome `MyComponentLib1`del progetto.</span><span class="sxs-lookup"><span data-stu-id="8e62e-119">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="8e62e-120">Selezionare **Create**.</span><span class="sxs-lookup"><span data-stu-id="8e62e-120">Select **Create**.</span></span>
1. <span data-ttu-id="8e62e-121">Aggiungere RCL a una soluzione:</span><span class="sxs-lookup"><span data-stu-id="8e62e-121">Add the RCL to a solution:</span></span>
   1. <span data-ttu-id="8e62e-122">Fare clic con il pulsante destro del mouse sulla soluzione.</span><span class="sxs-lookup"><span data-stu-id="8e62e-122">Right-click the solution.</span></span> <span data-ttu-id="8e62e-123">Selezionare **Aggiungi** > **progetto esistente**.</span><span class="sxs-lookup"><span data-stu-id="8e62e-123">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="8e62e-124">Passare al file di progetto di RCL.</span><span class="sxs-lookup"><span data-stu-id="8e62e-124">Navigate to the RCL's project file.</span></span>
   1. <span data-ttu-id="8e62e-125">Selezionare il file di progetto di RCL (con*estensione csproj*).</span><span class="sxs-lookup"><span data-stu-id="8e62e-125">Select the RCL's project file (*.csproj*).</span></span>
1. <span data-ttu-id="8e62e-126">Aggiungere un riferimento a RCL dall'app:</span><span class="sxs-lookup"><span data-stu-id="8e62e-126">Add a reference the RCL from the app:</span></span>
   1. <span data-ttu-id="8e62e-127">Fare clic con il pulsante destro del mouse sul progetto app.</span><span class="sxs-lookup"><span data-stu-id="8e62e-127">Right-click the app project.</span></span> <span data-ttu-id="8e62e-128">Selezionare **Aggiungi** > **riferimento**.</span><span class="sxs-lookup"><span data-stu-id="8e62e-128">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="8e62e-129">Selezionare il progetto RCL.</span><span class="sxs-lookup"><span data-stu-id="8e62e-129">Select the RCL project.</span></span> <span data-ttu-id="8e62e-130">Selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="8e62e-130">Select **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8e62e-131">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="8e62e-131">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="8e62e-132">Usare il modello di **libreria di classi Razor** (`razorclasslib`) con il comando [DotNet New](/dotnet/core/tools/dotnet-new) in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="8e62e-132">Use the **Razor Class Library** template (`razorclasslib`) with the [dotnet new](/dotnet/core/tools/dotnet-new) command in a command shell.</span></span> <span data-ttu-id="8e62e-133">Nell'esempio seguente viene creato un RCL denominato `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="8e62e-133">In the following example, an RCL is created named `MyComponentLib1`.</span></span> <span data-ttu-id="8e62e-134">La cartella che include `MyComponentLib1` viene creata automaticamente quando si esegue il comando:</span><span class="sxs-lookup"><span data-stu-id="8e62e-134">The folder that holds `MyComponentLib1` is created automatically when the command is executed:</span></span>

   ```console
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. <span data-ttu-id="8e62e-135">Per aggiungere la libreria a un progetto esistente, usare il comando [DotNet Add Reference](/dotnet/core/tools/dotnet-add-reference) in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="8e62e-135">To add the library to an existing project, use the [dotnet add reference](/dotnet/core/tools/dotnet-add-reference) command in a command shell.</span></span> <span data-ttu-id="8e62e-136">Nell'esempio seguente viene aggiunto RCL all'app.</span><span class="sxs-lookup"><span data-stu-id="8e62e-136">In the following example, the RCL is added to the app.</span></span> <span data-ttu-id="8e62e-137">Eseguire il comando seguente dalla cartella del progetto dell'app con il percorso della libreria:</span><span class="sxs-lookup"><span data-stu-id="8e62e-137">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```console
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="rcls-not-supported-for-client-side-apps"></a><span data-ttu-id="8e62e-138">RCL non supportato per le app sul lato client</span><span class="sxs-lookup"><span data-stu-id="8e62e-138">RCLs not supported for client-side apps</span></span>

<span data-ttu-id="8e62e-139">Nell'anteprima corrente di ASP.NET Core 3,0, le librerie di classi Razor non sono compatibili con le app lato client di Blazer.</span><span class="sxs-lookup"><span data-stu-id="8e62e-139">In the current ASP.NET Core 3.0 Preview, Razor class libraries aren't compatible with Blazor client-side apps.</span></span> <span data-ttu-id="8e62e-140">Per le `blazorlib` app Blazer sul lato client, usare una libreria di componenti Blazer creata dal modello in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="8e62e-140">For Blazor client-side apps, use a Blazor component library created by the `blazorlib` template in a command shell:</span></span>

```console
dotnet new blazorlib -o MyComponentLib1
```

<span data-ttu-id="8e62e-141">Le librerie dei componenti `blazorlib` che usano il modello possono includere file statici, ad esempio immagini, JavaScript e fogli di stile.</span><span class="sxs-lookup"><span data-stu-id="8e62e-141">Component libraries using the `blazorlib` template can include static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="8e62e-142">In fase di compilazione, i file statici vengono incorporati nel file di assembly compilato ( *. dll*), che consente l'utilizzo dei componenti senza doversi preoccupare di includere le risorse.</span><span class="sxs-lookup"><span data-stu-id="8e62e-142">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="8e62e-143">Tutti i file inclusi nella `content` directory sono contrassegnati come una risorsa incorporata.</span><span class="sxs-lookup"><span data-stu-id="8e62e-143">Any files included in the `content` directory are marked as an embedded resource.</span></span>

## <a name="consume-a-library-component"></a><span data-ttu-id="8e62e-144">Utilizzare un componente di libreria</span><span class="sxs-lookup"><span data-stu-id="8e62e-144">Consume a library component</span></span>

<span data-ttu-id="8e62e-145">Per utilizzare i componenti definiti in una raccolta in un altro progetto, utilizzare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="8e62e-145">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="8e62e-146">Usare il nome completo del tipo con lo spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="8e62e-146">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="8e62e-147">Usare la direttiva [ \@using](xref:mvc/views/razor#using) di Razor.</span><span class="sxs-lookup"><span data-stu-id="8e62e-147">Use Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="8e62e-148">I singoli componenti possono essere aggiunti in base al nome.</span><span class="sxs-lookup"><span data-stu-id="8e62e-148">Individual components can be added by name.</span></span>

<span data-ttu-id="8e62e-149">Negli esempi `MyComponentLib1` seguenti è una libreria di componenti contenente un `SalesReport` componente di.</span><span class="sxs-lookup"><span data-stu-id="8e62e-149">In the following examples, `MyComponentLib1` is a component library containing a `SalesReport` component.</span></span>

<span data-ttu-id="8e62e-150">È `SalesReport` possibile fare riferimento al componente usando il nome completo del tipo con lo spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="8e62e-150">The `SalesReport` component can be referenced using its full type name with namespace:</span></span>

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="8e62e-151">È possibile fare riferimento al componente anche se la libreria viene inserita nell'ambito con una `@using` direttiva:</span><span class="sxs-lookup"><span data-stu-id="8e62e-151">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="8e62e-152">Includere la `@using MyComponentLib1` direttiva nel file *_Import. Razor* di primo livello per rendere disponibili i componenti della libreria a un intero progetto.</span><span class="sxs-lookup"><span data-stu-id="8e62e-152">Include the `@using MyComponentLib1` directive in the top-level *_Import.razor* file to make the library's components available to an entire project.</span></span> <span data-ttu-id="8e62e-153">Aggiungere la direttiva a un file *_Import. Razor* a qualsiasi livello per applicare lo spazio dei nomi a una singola pagina o a un set di pagine all'interno di una cartella.</span><span class="sxs-lookup"><span data-stu-id="8e62e-153">Add the directive to an *_Import.razor* file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="8e62e-154">Compilare, comprimere e spedire a NuGet</span><span class="sxs-lookup"><span data-stu-id="8e62e-154">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="8e62e-155">Poiché le librerie dei componenti sono librerie .NET standard, la creazione di pacchetti e la spedizione a NuGet non è diversa dalla creazione di pacchetti e dalla distribuzione di qualsiasi libreria a NuGet.</span><span class="sxs-lookup"><span data-stu-id="8e62e-155">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="8e62e-156">La creazione di pacchetti viene eseguita usando il comando [DotNet Pack](/dotnet/core/tools/dotnet-pack) in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="8e62e-156">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command in a command shell:</span></span>

```console
dotnet pack
```

<span data-ttu-id="8e62e-157">Caricare il pacchetto in NuGet usando il comando [DotNet NuGet Publish](/dotnet/core/tools/dotnet-nuget-push) in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="8e62e-157">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command in a command shell:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="8e62e-158">Quando si usa `blazorlib` il modello, le risorse statiche sono incluse nel pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="8e62e-158">When using the `blazorlib` template, static resources are included in the NuGet package.</span></span> <span data-ttu-id="8e62e-159">I consumer di librerie ricevono automaticamente script e fogli di stile, pertanto non è necessario che gli utenti installino manualmente le risorse.</span><span class="sxs-lookup"><span data-stu-id="8e62e-159">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>

## <a name="create-a-razor-components-class-library-with-static-assets"></a><span data-ttu-id="8e62e-160">Creare una libreria di classi di componenti Razor con asset statici</span><span class="sxs-lookup"><span data-stu-id="8e62e-160">Create a Razor components class library with static assets</span></span>

<span data-ttu-id="8e62e-161">Un RCL può includere asset statici.</span><span class="sxs-lookup"><span data-stu-id="8e62e-161">An RCL can include static assets.</span></span> <span data-ttu-id="8e62e-162">Gli asset statici sono disponibili per qualsiasi app che usa la libreria.</span><span class="sxs-lookup"><span data-stu-id="8e62e-162">The static assets are available to any app that consumes the library.</span></span> <span data-ttu-id="8e62e-163">Per altre informazioni, vedere <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span><span class="sxs-lookup"><span data-stu-id="8e62e-163">For more information, see <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8e62e-164">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="8e62e-164">Additional resources</span></span>

* <xref:razor-pages/ui-class>
