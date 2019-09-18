---
title: Librerie di classi dei componenti di ASP.NET Core Razor
author: guardrex
description: Scopri in che modo i componenti possono essere inclusi nelle app blazer da una libreria di componenti esterna.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/class-libraries
ms.openlocfilehash: 91c4ffb29649c162fc15e5cb70dc60144b53afea
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080659"
---
# <a name="aspnet-core-razor-components-class-libraries"></a><span data-ttu-id="c39c4-103">Librerie di classi dei componenti di ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="c39c4-103">ASP.NET Core Razor components class libraries</span></span>

<span data-ttu-id="c39c4-104">Di [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="c39c4-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="c39c4-105">I componenti possono essere condivisi in una [libreria di classi Razor (RCL)](xref:razor-pages/ui-class) tra i progetti.</span><span class="sxs-lookup"><span data-stu-id="c39c4-105">Components can be shared in a [Razor class library (RCL)](xref:razor-pages/ui-class) across projects.</span></span> <span data-ttu-id="c39c4-106">È possibile includere una *libreria di classi di componenti Razor* da:</span><span class="sxs-lookup"><span data-stu-id="c39c4-106">A *Razor components class library* can be included from:</span></span>

* <span data-ttu-id="c39c4-107">Un altro progetto nella soluzione.</span><span class="sxs-lookup"><span data-stu-id="c39c4-107">Another project in the solution.</span></span>
* <span data-ttu-id="c39c4-108">Un pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="c39c4-108">A NuGet package.</span></span>
* <span data-ttu-id="c39c4-109">Libreria .NET A cui si fa riferimento.</span><span class="sxs-lookup"><span data-stu-id="c39c4-109">A referenced .NET library.</span></span>

<span data-ttu-id="c39c4-110">Così come i componenti sono tipi .NET normali, i componenti forniti da un RCL sono assembly .NET normali.</span><span class="sxs-lookup"><span data-stu-id="c39c4-110">Just as components are regular .NET types, components provided by an RCL are normal .NET assemblies.</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="c39c4-111">Creare un RCL</span><span class="sxs-lookup"><span data-stu-id="c39c4-111">Create an RCL</span></span>

<span data-ttu-id="c39c4-112">Per configurare l'ambiente per <xref:blazor/get-started> blazer, seguire le istruzioni riportate nell'articolo.</span><span class="sxs-lookup"><span data-stu-id="c39c4-112">Follow the guidance in the <xref:blazor/get-started> article to configure your environment for Blazor.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c39c4-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c39c4-113">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="c39c4-114">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="c39c4-114">Create a new project.</span></span>
1. <span data-ttu-id="c39c4-115">Selezionare **libreria di classi Razor**.</span><span class="sxs-lookup"><span data-stu-id="c39c4-115">Select **Razor Class Library**.</span></span> <span data-ttu-id="c39c4-116">Selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="c39c4-116">Select **Next**.</span></span>
1. <span data-ttu-id="c39c4-117">Nella finestra di dialogo **Crea una nuova libreria di classi Razor** selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="c39c4-117">In the **Create a new Razor class library** dialog, select **Create**.</span></span>
1. <span data-ttu-id="c39c4-118">Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="c39c4-118">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="c39c4-119">Gli esempi in questo argomento usano il nome `MyComponentLib1`del progetto.</span><span class="sxs-lookup"><span data-stu-id="c39c4-119">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="c39c4-120">Selezionare **Create**.</span><span class="sxs-lookup"><span data-stu-id="c39c4-120">Select **Create**.</span></span>
1. <span data-ttu-id="c39c4-121">Aggiungere RCL a una soluzione:</span><span class="sxs-lookup"><span data-stu-id="c39c4-121">Add the RCL to a solution:</span></span>
   1. <span data-ttu-id="c39c4-122">Fare clic con il pulsante destro del mouse sulla soluzione.</span><span class="sxs-lookup"><span data-stu-id="c39c4-122">Right-click the solution.</span></span> <span data-ttu-id="c39c4-123">Selezionare **Aggiungi** > **progetto esistente**.</span><span class="sxs-lookup"><span data-stu-id="c39c4-123">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="c39c4-124">Passare al file di progetto di RCL.</span><span class="sxs-lookup"><span data-stu-id="c39c4-124">Navigate to the RCL's project file.</span></span>
   1. <span data-ttu-id="c39c4-125">Selezionare il file di progetto di RCL (con*estensione csproj*).</span><span class="sxs-lookup"><span data-stu-id="c39c4-125">Select the RCL's project file (*.csproj*).</span></span>
1. <span data-ttu-id="c39c4-126">Aggiungere un riferimento a RCL dall'app:</span><span class="sxs-lookup"><span data-stu-id="c39c4-126">Add a reference the RCL from the app:</span></span>
   1. <span data-ttu-id="c39c4-127">Fare clic con il pulsante destro del mouse sul progetto app.</span><span class="sxs-lookup"><span data-stu-id="c39c4-127">Right-click the app project.</span></span> <span data-ttu-id="c39c4-128">Selezionare **Aggiungi** > **riferimento**.</span><span class="sxs-lookup"><span data-stu-id="c39c4-128">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="c39c4-129">Selezionare il progetto RCL.</span><span class="sxs-lookup"><span data-stu-id="c39c4-129">Select the RCL project.</span></span> <span data-ttu-id="c39c4-130">Selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="c39c4-130">Select **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c39c4-131">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="c39c4-131">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="c39c4-132">Usare il modello di **libreria di classi Razor** (`razorclasslib`) con il comando [DotNet New](/dotnet/core/tools/dotnet-new) in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="c39c4-132">Use the **Razor Class Library** template (`razorclasslib`) with the [dotnet new](/dotnet/core/tools/dotnet-new) command in a command shell.</span></span> <span data-ttu-id="c39c4-133">Nell'esempio seguente viene creato un RCL denominato `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="c39c4-133">In the following example, an RCL is created named `MyComponentLib1`.</span></span> <span data-ttu-id="c39c4-134">La cartella che include `MyComponentLib1` viene creata automaticamente quando si esegue il comando:</span><span class="sxs-lookup"><span data-stu-id="c39c4-134">The folder that holds `MyComponentLib1` is created automatically when the command is executed:</span></span>

   ```dotnetcli
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. <span data-ttu-id="c39c4-135">Per aggiungere la libreria a un progetto esistente, usare il comando [DotNet Add Reference](/dotnet/core/tools/dotnet-add-reference) in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="c39c4-135">To add the library to an existing project, use the [dotnet add reference](/dotnet/core/tools/dotnet-add-reference) command in a command shell.</span></span> <span data-ttu-id="c39c4-136">Nell'esempio seguente viene aggiunto RCL all'app.</span><span class="sxs-lookup"><span data-stu-id="c39c4-136">In the following example, the RCL is added to the app.</span></span> <span data-ttu-id="c39c4-137">Eseguire il comando seguente dalla cartella del progetto dell'app con il percorso della libreria:</span><span class="sxs-lookup"><span data-stu-id="c39c4-137">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```dotnetcli
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="consume-a-library-component"></a><span data-ttu-id="c39c4-138">Utilizzare un componente di libreria</span><span class="sxs-lookup"><span data-stu-id="c39c4-138">Consume a library component</span></span>

<span data-ttu-id="c39c4-139">Per utilizzare i componenti definiti in una raccolta in un altro progetto, utilizzare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="c39c4-139">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="c39c4-140">Usare il nome completo del tipo con lo spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="c39c4-140">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="c39c4-141">Usare la direttiva [ \@using](xref:mvc/views/razor#using) di Razor.</span><span class="sxs-lookup"><span data-stu-id="c39c4-141">Use Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="c39c4-142">I singoli componenti possono essere aggiunti in base al nome.</span><span class="sxs-lookup"><span data-stu-id="c39c4-142">Individual components can be added by name.</span></span>

<span data-ttu-id="c39c4-143">Negli esempi `MyComponentLib1` seguenti è una libreria di componenti contenente un `SalesReport` componente di.</span><span class="sxs-lookup"><span data-stu-id="c39c4-143">In the following examples, `MyComponentLib1` is a component library containing a `SalesReport` component.</span></span>

<span data-ttu-id="c39c4-144">È `SalesReport` possibile fare riferimento al componente usando il nome completo del tipo con lo spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="c39c4-144">The `SalesReport` component can be referenced using its full type name with namespace:</span></span>

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="c39c4-145">È possibile fare riferimento al componente anche se la libreria viene inserita nell'ambito con una `@using` direttiva:</span><span class="sxs-lookup"><span data-stu-id="c39c4-145">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="c39c4-146">Includere la `@using MyComponentLib1` direttiva nel file *_Import. Razor* di primo livello per rendere disponibili i componenti della libreria a un intero progetto.</span><span class="sxs-lookup"><span data-stu-id="c39c4-146">Include the `@using MyComponentLib1` directive in the top-level *_Import.razor* file to make the library's components available to an entire project.</span></span> <span data-ttu-id="c39c4-147">Aggiungere la direttiva a un file *_Import. Razor* a qualsiasi livello per applicare lo spazio dei nomi a una singola pagina o a un set di pagine all'interno di una cartella.</span><span class="sxs-lookup"><span data-stu-id="c39c4-147">Add the directive to an *_Import.razor* file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="c39c4-148">Compilare, comprimere e spedire a NuGet</span><span class="sxs-lookup"><span data-stu-id="c39c4-148">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="c39c4-149">Poiché le librerie dei componenti sono librerie .NET standard, la creazione di pacchetti e la spedizione a NuGet non è diversa dalla creazione di pacchetti e dalla distribuzione di qualsiasi libreria a NuGet.</span><span class="sxs-lookup"><span data-stu-id="c39c4-149">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="c39c4-150">La creazione di pacchetti viene eseguita usando il comando [DotNet Pack](/dotnet/core/tools/dotnet-pack) in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="c39c4-150">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command in a command shell:</span></span>

```dotnetcli
dotnet pack
```

<span data-ttu-id="c39c4-151">Caricare il pacchetto in NuGet usando il comando [DotNet NuGet Publish](/dotnet/core/tools/dotnet-nuget-push) in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="c39c4-151">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command in a command shell:</span></span>

```dotnetcli
dotnet nuget publish
```

## <a name="create-a-razor-components-class-library-with-static-assets"></a><span data-ttu-id="c39c4-152">Creare una libreria di classi di componenti Razor con asset statici</span><span class="sxs-lookup"><span data-stu-id="c39c4-152">Create a Razor components class library with static assets</span></span>

<span data-ttu-id="c39c4-153">Un RCL può includere asset statici.</span><span class="sxs-lookup"><span data-stu-id="c39c4-153">An RCL can include static assets.</span></span> <span data-ttu-id="c39c4-154">Gli asset statici sono disponibili per qualsiasi app che usa la libreria.</span><span class="sxs-lookup"><span data-stu-id="c39c4-154">The static assets are available to any app that consumes the library.</span></span> <span data-ttu-id="c39c4-155">Per altre informazioni, vedere <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span><span class="sxs-lookup"><span data-stu-id="c39c4-155">For more information, see <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c39c4-156">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c39c4-156">Additional resources</span></span>

* <xref:razor-pages/ui-class>
