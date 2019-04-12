---
title: Librerie di classi di componenti Razor
author: guardrex
description: Scopri come i componenti possono essere inclusi nelle App Razor componenti dalla libreria componente esterno.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/14/2019
uid: razor-components/class-libraries
ms.openlocfilehash: 1064ad60d90af15af483ba9bca5ed85fb63c2924
ms.sourcegitcommit: 10e14b85490f064395e9b2f423d21e3c2d39ed8b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "59515462"
---
# <a name="razor-components-class-libraries"></a><span data-ttu-id="d1dc4-103">Librerie di classi di componenti Razor</span><span class="sxs-lookup"><span data-stu-id="d1dc4-103">Razor Components Class Libraries</span></span>

<span data-ttu-id="d1dc4-104">Da [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="d1dc4-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="d1dc4-105">I componenti possono essere condivise nelle librerie di classi Razor tra progetti.</span><span class="sxs-lookup"><span data-stu-id="d1dc4-105">Components can be shared in Razor class libraries across projects.</span></span> <span data-ttu-id="d1dc4-106">I componenti possono essere inclusi da:</span><span class="sxs-lookup"><span data-stu-id="d1dc4-106">Components can be included from:</span></span>

* <span data-ttu-id="d1dc4-107">Un altro progetto nella soluzione.</span><span class="sxs-lookup"><span data-stu-id="d1dc4-107">Another project in the solution.</span></span>
* <span data-ttu-id="d1dc4-108">Un pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="d1dc4-108">A NuGet package.</span></span>
* <span data-ttu-id="d1dc4-109">Una libreria .NET di cui viene fatto riferimento.</span><span class="sxs-lookup"><span data-stu-id="d1dc4-109">A referenced .NET library.</span></span>

<span data-ttu-id="d1dc4-110">Proprio come i componenti sono i tipi regolari di .NET, i componenti forniti dalle librerie di classi Razor sono assembly .NET normale.</span><span class="sxs-lookup"><span data-stu-id="d1dc4-110">Just as components are regular .NET types, components provided by Razor class libraries are normal .NET assemblies.</span></span>

<span data-ttu-id="d1dc4-111">Usare la `razorclasslib` modello (libreria di classi Razor) con i [dotnet nuovo](/dotnet/core/tools/dotnet-new) comando:</span><span class="sxs-lookup"><span data-stu-id="d1dc4-111">Use the `razorclasslib` (Razor class library) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command:</span></span>

```console
dotnet new razorclasslib -o MyComponentLib1
```

<span data-ttu-id="d1dc4-112">Aggiungere i file Razor componenti (*.razor*) alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="d1dc4-112">Add Razor Component files (*.razor*) to the Razor class library.</span></span>

<span data-ttu-id="d1dc4-113">Per aggiungere la libreria a un progetto esistente, usare il [dotnet sln](/dotnet/core/tools/dotnet-sln) comando:</span><span class="sxs-lookup"><span data-stu-id="d1dc4-113">To add the library to an existing project, use the [dotnet sln](/dotnet/core/tools/dotnet-sln) command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d1dc4-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d1dc4-114">Visual Studio</span></span>](#tab/visual-studio)

```console
dotnet sln add .\MyComponentLib1
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d1dc4-115">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="d1dc4-115">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet add WebApplication1 reference MyComponentLib1
```

---

> [!NOTE]
> <span data-ttu-id="d1dc4-116">Librerie di classi Razor non sono compatibili con le app Blazor nell'anteprima 3 di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d1dc4-116">Razor class libraries aren't compatible with Blazor apps in ASP.NET Core Preview 3.</span></span>
>
> <span data-ttu-id="d1dc4-117">Per creare i componenti in una libreria che può essere condivisi con le app Blazor e componenti di Razor, usare una libreria di classi Blazor creata il `blazorlib` modello.</span><span class="sxs-lookup"><span data-stu-id="d1dc4-117">To create components in a library that can be shared with Blazor and Razor Components apps, use a Blazor class library created by the `blazorlib` template.</span></span>
>
> <span data-ttu-id="d1dc4-118">Librerie di classi Razor non supportano asset statici in ASP.NET Core Preview 3.</span><span class="sxs-lookup"><span data-stu-id="d1dc4-118">Razor class libraries don't support static assets in ASP.NET Core Preview 3.</span></span> <span data-ttu-id="d1dc4-119">Librerie dei componenti utilizzando il `blazorlib` modello può includere i file statici, ad esempio immagini, JavaScript e fogli di stile.</span><span class="sxs-lookup"><span data-stu-id="d1dc4-119">Component libraries using the `blazorlib` template can include static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="d1dc4-120">In fase di compilazione, i file statici vengono incorporati nel file di assembly compilato (*DLL*), che consente l'utilizzo dei componenti senza la necessità di preoccuparsi di come includere le relative risorse.</span><span class="sxs-lookup"><span data-stu-id="d1dc4-120">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="d1dc4-121">File inclusi nel `content` directory sono contrassegnate come risorsa incorporata.</span><span class="sxs-lookup"><span data-stu-id="d1dc4-121">Any files included in the `content` directory are marked as an embedded resource.</span></span>

## <a name="consume-a-library-component"></a><span data-ttu-id="d1dc4-122">Usare un componente della libreria</span><span class="sxs-lookup"><span data-stu-id="d1dc4-122">Consume a library component</span></span>

<span data-ttu-id="d1dc4-123">Per utilizzare i componenti definiti in una libreria in un altro progetto, il [ @addTagHelper ](xref:mvc/views/tag-helpers/intro#add-helper-label) direttiva deve essere utilizzata.</span><span class="sxs-lookup"><span data-stu-id="d1dc4-123">In order to consume components defined in a library in another project, the [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) directive must be used.</span></span> <span data-ttu-id="d1dc4-124">È possibile aggiungere i singoli componenti in base al nome.</span><span class="sxs-lookup"><span data-stu-id="d1dc4-124">Individual components may be added by name.</span></span>

<span data-ttu-id="d1dc4-125">Il formato generale della direttiva è:</span><span class="sxs-lookup"><span data-stu-id="d1dc4-125">The general format of the directive is:</span></span>

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<Component1 />
```

<span data-ttu-id="d1dc4-126">Ad esempio, aggiunge la direttiva seguente `Component1` di `MyComponentLib1`:</span><span class="sxs-lookup"><span data-stu-id="d1dc4-126">For example, the following directive adds `Component1` of `MyComponentLib1`:</span></span>

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

<span data-ttu-id="d1dc4-127">Tuttavia, è comune per includere tutti i componenti da un assembly con un carattere jolly (`*`):</span><span class="sxs-lookup"><span data-stu-id="d1dc4-127">However, it's common to include all of the components from an assembly using a wildcard (`*`):</span></span>

```cshtml
@addTagHelper *, MyComponentLib1
```

<span data-ttu-id="d1dc4-128">Il `@addTagHelper` direttiva può essere incluso nella *_ViewImport.cshtml* per rendere i componenti disponibili per un intero progetto o applicate a una singola pagina o un set di pagine all'interno di una cartella.</span><span class="sxs-lookup"><span data-stu-id="d1dc4-128">The `@addTagHelper` directive can be included in *_ViewImport.cshtml* to make the components available for an entire project or applied to a single page or set of pages within a folder.</span></span> <span data-ttu-id="d1dc4-129">Con la `@addTagHelper` direttiva posto, i componenti della libreria di componenti possono essere utilizzati come se fossero nello stesso assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="d1dc4-129">With the `@addTagHelper` directive in place, the components of the component library can be consumed as if they were in the same assembly as the app.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="d1dc4-130">Compilazione, Service pack e spedire a NuGet</span><span class="sxs-lookup"><span data-stu-id="d1dc4-130">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="d1dc4-131">Poiché le librerie dei componenti sono librerie .NET standard, creazione di pacchetti e l'invio in NuGet non è diverso da imballaggio e spedizione qualsiasi libreria da NuGet.</span><span class="sxs-lookup"><span data-stu-id="d1dc4-131">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="d1dc4-132">Creazione di pacchetti viene eseguita utilizzando il [dotnet pack](/dotnet/core/tools/dotnet-pack) comando:</span><span class="sxs-lookup"><span data-stu-id="d1dc4-132">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command:</span></span>

```console
dotnet pack
```

<span data-ttu-id="d1dc4-133">Caricare il pacchetto NuGet usando il [dotnet nuget pubblicare](/dotnet/core/tools/dotnet-nuget-push) comando:</span><span class="sxs-lookup"><span data-stu-id="d1dc4-133">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="d1dc4-134">Quando si usa il `blazorlib` modello di risorse statiche sono inclusi nel pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="d1dc4-134">When using the `blazorlib` template, static resources are included in the NuGet package.</span></span> <span data-ttu-id="d1dc4-135">Consumer della libreria ricevono automaticamente gli script e fogli di stile, in modo che i consumer non è necessari installare manualmente le risorse.</span><span class="sxs-lookup"><span data-stu-id="d1dc4-135">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>
