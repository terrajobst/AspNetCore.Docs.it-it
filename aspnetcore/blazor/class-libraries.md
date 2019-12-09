---
title: Librerie di classi dei componenti di ASP.NET Core Razor
author: guardrex
description: Scopri in che modo è possibile includere i componenti nelle app Blazor da una libreria di componenti esterna.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/class-libraries
ms.openlocfilehash: 6bac007e3e1d046d6b16a3a0be6dc5976b99b766
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2019
ms.locfileid: "74943875"
---
# <a name="aspnet-core-razor-components-class-libraries"></a><span data-ttu-id="15784-103">Librerie di classi dei componenti di ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="15784-103">ASP.NET Core Razor components class libraries</span></span>

<span data-ttu-id="15784-104">Di [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="15784-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="15784-105">I componenti possono essere condivisi in una [libreria di classi Razor (RCL)](xref:razor-pages/ui-class) tra i progetti.</span><span class="sxs-lookup"><span data-stu-id="15784-105">Components can be shared in a [Razor class library (RCL)](xref:razor-pages/ui-class) across projects.</span></span> <span data-ttu-id="15784-106">È possibile includere una *libreria di classi di componenti Razor* da:</span><span class="sxs-lookup"><span data-stu-id="15784-106">A *Razor components class library* can be included from:</span></span>

* <span data-ttu-id="15784-107">Un altro progetto nella soluzione.</span><span class="sxs-lookup"><span data-stu-id="15784-107">Another project in the solution.</span></span>
* <span data-ttu-id="15784-108">Un pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="15784-108">A NuGet package.</span></span>
* <span data-ttu-id="15784-109">Libreria .NET A cui si fa riferimento.</span><span class="sxs-lookup"><span data-stu-id="15784-109">A referenced .NET library.</span></span>

<span data-ttu-id="15784-110">Così come i componenti sono tipi .NET normali, i componenti forniti da un RCL sono assembly .NET normali.</span><span class="sxs-lookup"><span data-stu-id="15784-110">Just as components are regular .NET types, components provided by an RCL are normal .NET assemblies.</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="15784-111">Creare un RCL</span><span class="sxs-lookup"><span data-stu-id="15784-111">Create an RCL</span></span>

<span data-ttu-id="15784-112">Per configurare l'ambiente per Blazor, seguire le istruzioni riportate nell'articolo <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="15784-112">Follow the guidance in the <xref:blazor/get-started> article to configure your environment for Blazor.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="15784-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="15784-113">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="15784-114">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="15784-114">Create a new project.</span></span>
1. <span data-ttu-id="15784-115">Selezionare **libreria di classi Razor**.</span><span class="sxs-lookup"><span data-stu-id="15784-115">Select **Razor Class Library**.</span></span> <span data-ttu-id="15784-116">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="15784-116">Select **Next**.</span></span>
1. <span data-ttu-id="15784-117">Nella finestra di dialogo **Crea una nuova libreria di classi Razor** selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="15784-117">In the **Create a new Razor class library** dialog, select **Create**.</span></span>
1. <span data-ttu-id="15784-118">Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="15784-118">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="15784-119">Gli esempi in questo argomento usano il nome del progetto `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="15784-119">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="15784-120">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="15784-120">Select **Create**.</span></span>
1. <span data-ttu-id="15784-121">Aggiungere RCL a una soluzione:</span><span class="sxs-lookup"><span data-stu-id="15784-121">Add the RCL to a solution:</span></span>
   1. <span data-ttu-id="15784-122">Fare clic con il pulsante destro del mouse sulla soluzione.</span><span class="sxs-lookup"><span data-stu-id="15784-122">Right-click the solution.</span></span> <span data-ttu-id="15784-123">Selezionare **aggiungi** > **progetto esistente**.</span><span class="sxs-lookup"><span data-stu-id="15784-123">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="15784-124">Passare al file di progetto di RCL.</span><span class="sxs-lookup"><span data-stu-id="15784-124">Navigate to the RCL's project file.</span></span>
   1. <span data-ttu-id="15784-125">Selezionare il file di progetto di RCL (con*estensione csproj*).</span><span class="sxs-lookup"><span data-stu-id="15784-125">Select the RCL's project file (*.csproj*).</span></span>
1. <span data-ttu-id="15784-126">Aggiungere un riferimento a RCL dall'app:</span><span class="sxs-lookup"><span data-stu-id="15784-126">Add a reference the RCL from the app:</span></span>
   1. <span data-ttu-id="15784-127">Fare clic con il pulsante destro del mouse sul progetto app.</span><span class="sxs-lookup"><span data-stu-id="15784-127">Right-click the app project.</span></span> <span data-ttu-id="15784-128">Selezionare **Aggiungi** **riferimento** > .</span><span class="sxs-lookup"><span data-stu-id="15784-128">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="15784-129">Selezionare il progetto RCL.</span><span class="sxs-lookup"><span data-stu-id="15784-129">Select the RCL project.</span></span> <span data-ttu-id="15784-130">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="15784-130">Select **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="15784-131">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="15784-131">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="15784-132">Usare il modello di **libreria di classi Razor** (`razorclasslib`) con il comando [DotNet New](/dotnet/core/tools/dotnet-new) in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="15784-132">Use the **Razor Class Library** template (`razorclasslib`) with the [dotnet new](/dotnet/core/tools/dotnet-new) command in a command shell.</span></span> <span data-ttu-id="15784-133">Nell'esempio seguente viene creato un RCL denominato `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="15784-133">In the following example, an RCL is created named `MyComponentLib1`.</span></span> <span data-ttu-id="15784-134">La cartella che include `MyComponentLib1` viene creata automaticamente quando si esegue il comando:</span><span class="sxs-lookup"><span data-stu-id="15784-134">The folder that holds `MyComponentLib1` is created automatically when the command is executed:</span></span>

   ```dotnetcli
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. <span data-ttu-id="15784-135">Per aggiungere la libreria a un progetto esistente, usare il comando [DotNet Add Reference](/dotnet/core/tools/dotnet-add-reference) in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="15784-135">To add the library to an existing project, use the [dotnet add reference](/dotnet/core/tools/dotnet-add-reference) command in a command shell.</span></span> <span data-ttu-id="15784-136">Nell'esempio seguente viene aggiunto RCL all'app.</span><span class="sxs-lookup"><span data-stu-id="15784-136">In the following example, the RCL is added to the app.</span></span> <span data-ttu-id="15784-137">Eseguire il comando seguente dalla cartella del progetto dell'app con il percorso della libreria:</span><span class="sxs-lookup"><span data-stu-id="15784-137">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```dotnetcli
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="consume-a-library-component"></a><span data-ttu-id="15784-138">Utilizzare un componente di libreria</span><span class="sxs-lookup"><span data-stu-id="15784-138">Consume a library component</span></span>

<span data-ttu-id="15784-139">Per utilizzare i componenti definiti in una raccolta in un altro progetto, utilizzare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="15784-139">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="15784-140">Usare il nome completo del tipo con lo spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="15784-140">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="15784-141">Usare la direttiva Razor [\@using](xref:mvc/views/razor#using) .</span><span class="sxs-lookup"><span data-stu-id="15784-141">Use Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="15784-142">I singoli componenti possono essere aggiunti in base al nome.</span><span class="sxs-lookup"><span data-stu-id="15784-142">Individual components can be added by name.</span></span>

<span data-ttu-id="15784-143">Negli esempi seguenti `MyComponentLib1` è una libreria di componenti contenente un componente di `SalesReport`.</span><span class="sxs-lookup"><span data-stu-id="15784-143">In the following examples, `MyComponentLib1` is a component library containing a `SalesReport` component.</span></span>

<span data-ttu-id="15784-144">È possibile fare riferimento al componente `SalesReport` usando il nome completo del tipo con lo spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="15784-144">The `SalesReport` component can be referenced using its full type name with namespace:</span></span>

```razor
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="15784-145">È possibile fare riferimento al componente anche se la libreria viene inserita nell'ambito con una direttiva `@using`:</span><span class="sxs-lookup"><span data-stu-id="15784-145">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```razor
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="15784-146">Includere la direttiva `@using MyComponentLib1` nel file *_Import. Razor* di primo livello per rendere disponibili i componenti della libreria a un intero progetto.</span><span class="sxs-lookup"><span data-stu-id="15784-146">Include the `@using MyComponentLib1` directive in the top-level *_Import.razor* file to make the library's components available to an entire project.</span></span> <span data-ttu-id="15784-147">Aggiungere la direttiva a un file *_Import. Razor* a qualsiasi livello per applicare lo spazio dei nomi a una singola pagina o a un set di pagine all'interno di una cartella.</span><span class="sxs-lookup"><span data-stu-id="15784-147">Add the directive to an *_Import.razor* file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="15784-148">Compilare, comprimere e spedire a NuGet</span><span class="sxs-lookup"><span data-stu-id="15784-148">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="15784-149">Poiché le librerie dei componenti sono librerie .NET standard, la creazione di pacchetti e la spedizione a NuGet non è diversa dalla creazione di pacchetti e dalla distribuzione di qualsiasi libreria a NuGet.</span><span class="sxs-lookup"><span data-stu-id="15784-149">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="15784-150">La creazione di pacchetti viene eseguita usando il comando [DotNet Pack](/dotnet/core/tools/dotnet-pack) in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="15784-150">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command in a command shell:</span></span>

```dotnetcli
dotnet pack
```

<span data-ttu-id="15784-151">Caricare il pacchetto in NuGet usando il comando [DotNet NuGet push](/dotnet/core/tools/dotnet-nuget-push) in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="15784-151">Upload the package to NuGet using the [dotnet nuget push](/dotnet/core/tools/dotnet-nuget-push) command in a command shell.</span></span>

## <a name="create-a-razor-components-class-library-with-static-assets"></a><span data-ttu-id="15784-152">Creare una libreria di classi di componenti Razor con asset statici</span><span class="sxs-lookup"><span data-stu-id="15784-152">Create a Razor components class library with static assets</span></span>

<span data-ttu-id="15784-153">Un RCL può includere asset statici.</span><span class="sxs-lookup"><span data-stu-id="15784-153">An RCL can include static assets.</span></span> <span data-ttu-id="15784-154">Gli asset statici sono disponibili per qualsiasi app che usa la libreria.</span><span class="sxs-lookup"><span data-stu-id="15784-154">The static assets are available to any app that consumes the library.</span></span> <span data-ttu-id="15784-155">Per ulteriori informazioni, vedere <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span><span class="sxs-lookup"><span data-stu-id="15784-155">For more information, see <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="15784-156">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="15784-156">Additional resources</span></span>

* <xref:razor-pages/ui-class>
