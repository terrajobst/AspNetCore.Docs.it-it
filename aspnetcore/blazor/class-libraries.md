---
title: Librerie di classi dei componenti di ASP.NET Core Razor
author: guardrex
description: Scopri in che modo i componenti possono essere inclusi nelle app blazer da una libreria di componenti esterna.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/class-libraries
ms.openlocfilehash: 402b7b072554f63f85e7cf5e55336104d235a071
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2019
ms.locfileid: "68948441"
---
# <a name="aspnet-core-razor-components-class-libraries"></a><span data-ttu-id="b5020-103">Librerie di classi dei componenti di ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="b5020-103">ASP.NET Core Razor components class libraries</span></span>

<span data-ttu-id="b5020-104">Di [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="b5020-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="b5020-105">I componenti possono essere condivisi in una [libreria di classi Razor (RCL)](xref:razor-pages/ui-class) tra i progetti.</span><span class="sxs-lookup"><span data-stu-id="b5020-105">Components can be shared in a [Razor class library (RCL)](xref:razor-pages/ui-class) across projects.</span></span> <span data-ttu-id="b5020-106">È possibile includere una *libreria di classi di componenti Razor* da:</span><span class="sxs-lookup"><span data-stu-id="b5020-106">A *Razor components class library* can be included from:</span></span>

* <span data-ttu-id="b5020-107">Un altro progetto nella soluzione.</span><span class="sxs-lookup"><span data-stu-id="b5020-107">Another project in the solution.</span></span>
* <span data-ttu-id="b5020-108">Un pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="b5020-108">A NuGet package.</span></span>
* <span data-ttu-id="b5020-109">Libreria .NET A cui si fa riferimento.</span><span class="sxs-lookup"><span data-stu-id="b5020-109">A referenced .NET library.</span></span>

<span data-ttu-id="b5020-110">Così come i componenti sono tipi .NET normali, i componenti forniti da un RCL sono assembly .NET normali.</span><span class="sxs-lookup"><span data-stu-id="b5020-110">Just as components are regular .NET types, components provided by an RCL are normal .NET assemblies.</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="b5020-111">Creare un RCL</span><span class="sxs-lookup"><span data-stu-id="b5020-111">Create an RCL</span></span>

<span data-ttu-id="b5020-112">Per configurare l'ambiente per <xref:blazor/get-started> blazer, seguire le istruzioni riportate nell'articolo.</span><span class="sxs-lookup"><span data-stu-id="b5020-112">Follow the guidance in the <xref:blazor/get-started> article to configure your environment for Blazor.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b5020-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b5020-113">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="b5020-114">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="b5020-114">Create a new project.</span></span>
1. <span data-ttu-id="b5020-115">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="b5020-115">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="b5020-116">Selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="b5020-116">Select **Next**.</span></span>
1. <span data-ttu-id="b5020-117">Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="b5020-117">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="b5020-118">Gli esempi in questo argomento usano il nome `MyComponentLib1`del progetto.</span><span class="sxs-lookup"><span data-stu-id="b5020-118">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="b5020-119">Selezionare **Create**.</span><span class="sxs-lookup"><span data-stu-id="b5020-119">Select **Create**.</span></span>
1. <span data-ttu-id="b5020-120">Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core** verificare che siano selezionati **.NET Core** e **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="b5020-120">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="b5020-121">Selezionare il modello **libreria di classi Razor** .</span><span class="sxs-lookup"><span data-stu-id="b5020-121">Select the **Razor Class Library** template.</span></span> <span data-ttu-id="b5020-122">Selezionare **Create**.</span><span class="sxs-lookup"><span data-stu-id="b5020-122">Select **Create**.</span></span>
1. <span data-ttu-id="b5020-123">Aggiungere RCL a una soluzione:</span><span class="sxs-lookup"><span data-stu-id="b5020-123">Add the RCL to a solution:</span></span>
   1. <span data-ttu-id="b5020-124">Fare clic con il pulsante destro del mouse sulla soluzione.</span><span class="sxs-lookup"><span data-stu-id="b5020-124">Right-click the solution.</span></span> <span data-ttu-id="b5020-125">Selezionare **Aggiungi** > **progetto esistente**.</span><span class="sxs-lookup"><span data-stu-id="b5020-125">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="b5020-126">Passare al file di progetto di RCL.</span><span class="sxs-lookup"><span data-stu-id="b5020-126">Navigate to the RCL's project file.</span></span>
   1. <span data-ttu-id="b5020-127">Selezionare il file di progetto di RCL (con*estensione csproj*).</span><span class="sxs-lookup"><span data-stu-id="b5020-127">Select the RCL's project file (*.csproj*).</span></span>
1. <span data-ttu-id="b5020-128">Aggiungere un riferimento a RCL dall'app:</span><span class="sxs-lookup"><span data-stu-id="b5020-128">Add a reference the RCL from the app:</span></span>
   1. <span data-ttu-id="b5020-129">Fare clic con il pulsante destro del mouse sul progetto app.</span><span class="sxs-lookup"><span data-stu-id="b5020-129">Right-click the app project.</span></span> <span data-ttu-id="b5020-130">Selezionare **Aggiungi** > **riferimento**.</span><span class="sxs-lookup"><span data-stu-id="b5020-130">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="b5020-131">Selezionare il progetto RCL.</span><span class="sxs-lookup"><span data-stu-id="b5020-131">Select the RCL project.</span></span> <span data-ttu-id="b5020-132">Selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="b5020-132">Select **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b5020-133">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="b5020-133">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="b5020-134">Usare il modello di **libreria di classi Razor** (`razorclasslib`) con il comando [DotNet New](/dotnet/core/tools/dotnet-new) in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="b5020-134">Use the **Razor Class Library** template (`razorclasslib`) with the [dotnet new](/dotnet/core/tools/dotnet-new) command in a command shell.</span></span> <span data-ttu-id="b5020-135">Nell'esempio seguente viene creato un RCL denominato `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="b5020-135">In the following example, an RCL is created named `MyComponentLib1`.</span></span> <span data-ttu-id="b5020-136">La cartella che include `MyComponentLib1` viene creata automaticamente quando si esegue il comando:</span><span class="sxs-lookup"><span data-stu-id="b5020-136">The folder that holds `MyComponentLib1` is created automatically when the command is executed:</span></span>

   ```console
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. <span data-ttu-id="b5020-137">Per aggiungere la libreria a un progetto esistente, usare il comando [DotNet Add Reference](/dotnet/core/tools/dotnet-add-reference) in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="b5020-137">To add the library to an existing project, use the [dotnet add reference](/dotnet/core/tools/dotnet-add-reference) command in a command shell.</span></span> <span data-ttu-id="b5020-138">Nell'esempio seguente viene aggiunto RCL all'app.</span><span class="sxs-lookup"><span data-stu-id="b5020-138">In the following example, the RCL is added to the app.</span></span> <span data-ttu-id="b5020-139">Eseguire il comando seguente dalla cartella del progetto dell'app con il percorso della libreria:</span><span class="sxs-lookup"><span data-stu-id="b5020-139">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```console
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="rcls-not-supported-for-client-side-apps"></a><span data-ttu-id="b5020-140">RCL non supportato per le app sul lato client</span><span class="sxs-lookup"><span data-stu-id="b5020-140">RCLs not supported for client-side apps</span></span>

<span data-ttu-id="b5020-141">Nell'anteprima corrente di ASP.NET Core 3,0, le librerie di classi Razor non sono compatibili con le app lato client di Blazer.</span><span class="sxs-lookup"><span data-stu-id="b5020-141">In the current ASP.NET Core 3.0 Preview, Razor class libraries aren't compatible with Blazor client-side apps.</span></span> <span data-ttu-id="b5020-142">Per le `blazorlib` app Blazer sul lato client, usare una libreria di componenti Blazer creata dal modello in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="b5020-142">For Blazor client-side apps, use a Blazor component library created by the `blazorlib` template in a command shell:</span></span>

```console
dotnet new blazorlib -o MyComponentLib1
```

<span data-ttu-id="b5020-143">Le librerie dei componenti `blazorlib` che usano il modello possono includere file statici, ad esempio immagini, JavaScript e fogli di stile.</span><span class="sxs-lookup"><span data-stu-id="b5020-143">Component libraries using the `blazorlib` template can include static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="b5020-144">In fase di compilazione, i file statici vengono incorporati nel file di assembly compilato ( *. dll*), che consente l'utilizzo dei componenti senza doversi preoccupare di includere le risorse.</span><span class="sxs-lookup"><span data-stu-id="b5020-144">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="b5020-145">Tutti i file inclusi nella `content` directory sono contrassegnati come una risorsa incorporata.</span><span class="sxs-lookup"><span data-stu-id="b5020-145">Any files included in the `content` directory are marked as an embedded resource.</span></span>

## <a name="consume-a-library-component"></a><span data-ttu-id="b5020-146">Utilizzare un componente di libreria</span><span class="sxs-lookup"><span data-stu-id="b5020-146">Consume a library component</span></span>

<span data-ttu-id="b5020-147">Per utilizzare i componenti definiti in una raccolta in un altro progetto, utilizzare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="b5020-147">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="b5020-148">Usare il nome completo del tipo con lo spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="b5020-148">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="b5020-149">Usare la direttiva [ \@using](xref:mvc/views/razor#using) di Razor.</span><span class="sxs-lookup"><span data-stu-id="b5020-149">Use Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="b5020-150">I singoli componenti possono essere aggiunti in base al nome.</span><span class="sxs-lookup"><span data-stu-id="b5020-150">Individual components can be added by name.</span></span>

<span data-ttu-id="b5020-151">Negli esempi `MyComponentLib1` seguenti è una libreria di componenti contenente un `SalesReport` componente di.</span><span class="sxs-lookup"><span data-stu-id="b5020-151">In the following examples, `MyComponentLib1` is a component library containing a `SalesReport` component.</span></span>

<span data-ttu-id="b5020-152">È `SalesReport` possibile fare riferimento al componente usando il nome completo del tipo con lo spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="b5020-152">The `SalesReport` component can be referenced using its full type name with namespace:</span></span>

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="b5020-153">È possibile fare riferimento al componente anche se la libreria viene inserita nell'ambito con una `@using` direttiva:</span><span class="sxs-lookup"><span data-stu-id="b5020-153">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="b5020-154">Includere la `@using MyComponentLib1` direttiva nel file *_Import. Razor* di primo livello per rendere disponibili i componenti della libreria a un intero progetto.</span><span class="sxs-lookup"><span data-stu-id="b5020-154">Include the `@using MyComponentLib1` directive in the top-level *_Import.razor* file to make the library's components available to an entire project.</span></span> <span data-ttu-id="b5020-155">Aggiungere la direttiva a un file *_Import. Razor* a qualsiasi livello per applicare lo spazio dei nomi a una singola pagina o a un set di pagine all'interno di una cartella.</span><span class="sxs-lookup"><span data-stu-id="b5020-155">Add the directive to an *_Import.razor* file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="b5020-156">Compilare, comprimere e spedire a NuGet</span><span class="sxs-lookup"><span data-stu-id="b5020-156">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="b5020-157">Poiché le librerie dei componenti sono librerie .NET standard, la creazione di pacchetti e la spedizione a NuGet non è diversa dalla creazione di pacchetti e dalla distribuzione di qualsiasi libreria a NuGet.</span><span class="sxs-lookup"><span data-stu-id="b5020-157">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="b5020-158">La creazione di pacchetti viene eseguita usando il comando [DotNet Pack](/dotnet/core/tools/dotnet-pack) in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="b5020-158">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command in a command shell:</span></span>

```console
dotnet pack
```

<span data-ttu-id="b5020-159">Caricare il pacchetto in NuGet usando il comando [DotNet NuGet Publish](/dotnet/core/tools/dotnet-nuget-push) in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="b5020-159">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command in a command shell:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="b5020-160">Quando si usa `blazorlib` il modello, le risorse statiche sono incluse nel pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="b5020-160">When using the `blazorlib` template, static resources are included in the NuGet package.</span></span> <span data-ttu-id="b5020-161">I consumer di librerie ricevono automaticamente script e fogli di stile, pertanto non è necessario che gli utenti installino manualmente le risorse.</span><span class="sxs-lookup"><span data-stu-id="b5020-161">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>

## <a name="create-a-razor-components-class-library-with-static-assets"></a><span data-ttu-id="b5020-162">Creare una libreria di classi di componenti Razor con asset statici</span><span class="sxs-lookup"><span data-stu-id="b5020-162">Create a Razor components class library with static assets</span></span>

<span data-ttu-id="b5020-163">Un RCL può includere asset statici.</span><span class="sxs-lookup"><span data-stu-id="b5020-163">An RCL can include static assets.</span></span> <span data-ttu-id="b5020-164">Gli asset statici sono disponibili per qualsiasi app che usa la libreria.</span><span class="sxs-lookup"><span data-stu-id="b5020-164">The static assets are available to any app that consumes the library.</span></span> <span data-ttu-id="b5020-165">Per altre informazioni, vedere <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span><span class="sxs-lookup"><span data-stu-id="b5020-165">For more information, see <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b5020-166">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b5020-166">Additional resources</span></span>

* <xref:razor-pages/ui-class>
