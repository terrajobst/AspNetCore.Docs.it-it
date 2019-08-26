---
title: Layout di ASP.NET Core Blazer
author: guardrex
description: Informazioni su come creare componenti di layout riutilizzabili per le app blazer.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/layouts
ms.openlocfilehash: 2d652e149381f0a93e3135da978ab5737d47c6f1
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2019
ms.locfileid: "68948221"
---
# <a name="aspnet-core-blazor-layouts"></a><span data-ttu-id="75c43-103">Layout di ASP.NET Core Blazer</span><span class="sxs-lookup"><span data-stu-id="75c43-103">ASP.NET Core Blazor layouts</span></span>

<span data-ttu-id="75c43-104">Di [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="75c43-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="75c43-105">Alcuni elementi dell'app, ad esempio i menu, i messaggi di copyright e i logo aziendali, fanno in genere parte del layout generale dell'app e vengono usati da ogni componente nell'app.</span><span class="sxs-lookup"><span data-stu-id="75c43-105">Some app elements, such as menus, copyright messages, and company logos, are usually part of app's overall layout and used by every component in the app.</span></span> <span data-ttu-id="75c43-106">La copia del codice di questi elementi in tutti i componenti di un'app non è un approccio&mdash;efficace ogni volta che uno degli elementi richiede un aggiornamento, ogni componente deve essere aggiornato.</span><span class="sxs-lookup"><span data-stu-id="75c43-106">Copying the code of these elements into all of the components of an app isn't an efficient approach&mdash;every time one of the elements requires an update, every component must be updated.</span></span> <span data-ttu-id="75c43-107">Tale duplicazione è difficile da gestire e può causare contenuti incoerenti nel tempo.</span><span class="sxs-lookup"><span data-stu-id="75c43-107">Such duplication is difficult to maintain and can lead to inconsistent content over time.</span></span> <span data-ttu-id="75c43-108">I *layout* risolvono questo problema.</span><span class="sxs-lookup"><span data-stu-id="75c43-108">*Layouts* solve this problem.</span></span>

<span data-ttu-id="75c43-109">Tecnicamente, un layout è semplicemente un altro componente.</span><span class="sxs-lookup"><span data-stu-id="75c43-109">Technically, a layout is just another component.</span></span> <span data-ttu-id="75c43-110">Un layout è definito in un modello Razor o nel C# codice e può usare [Data Binding](xref:blazor/components#data-binding), l' [inserimento](xref:blazor/dependency-injection)di dipendenze e altri scenari di componenti.</span><span class="sxs-lookup"><span data-stu-id="75c43-110">A layout is defined in a Razor template or in C# code and can use [data binding](xref:blazor/components#data-binding), [dependency injection](xref:blazor/dependency-injection), and other component scenarios.</span></span>

<span data-ttu-id="75c43-111">Per trasformare un *componente* in un *layout*, il componente:</span><span class="sxs-lookup"><span data-stu-id="75c43-111">To turn a *component* into a *layout*, the component:</span></span>

* <span data-ttu-id="75c43-112">Eredita da `LayoutComponentBase`, che definisce una `Body` proprietà per il contenuto di cui è stato eseguito il rendering all'interno del layout.</span><span class="sxs-lookup"><span data-stu-id="75c43-112">Inherits from `LayoutComponentBase`, which defines a `Body` property for the rendered content inside the layout.</span></span>
* <span data-ttu-id="75c43-113">Usa la sintassi Razor `@Body` per specificare il percorso nel markup di layout in cui viene eseguito il rendering del contenuto.</span><span class="sxs-lookup"><span data-stu-id="75c43-113">Uses the Razor syntax `@Body` to specify the location in the layout markup where the content is rendered.</span></span>

<span data-ttu-id="75c43-114">Nell'esempio di codice seguente viene illustrato il modello Razor di un componente di layout, *MainLayout. Razor*.</span><span class="sxs-lookup"><span data-stu-id="75c43-114">The following code sample shows the Razor template of a layout component, *MainLayout.razor*.</span></span> <span data-ttu-id="75c43-115">Il layout eredita `LayoutComponentBase` e imposta l' `@Body` oggetto tra la barra di spostamento e il piè di pagina:</span><span class="sxs-lookup"><span data-stu-id="75c43-115">The layout inherits `LayoutComponentBase` and sets the `@Body` between the navigation bar and the footer:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

## <a name="specify-a-layout-in-a-component"></a><span data-ttu-id="75c43-116">Specificare un layout in un componente</span><span class="sxs-lookup"><span data-stu-id="75c43-116">Specify a layout in a component</span></span>

<span data-ttu-id="75c43-117">Usare la direttiva `@layout` Razor per applicare un layout a un componente.</span><span class="sxs-lookup"><span data-stu-id="75c43-117">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="75c43-118">Il compilatore esegue `@layout` la conversione `LayoutAttribute`in un oggetto, che viene applicato alla classe Component.</span><span class="sxs-lookup"><span data-stu-id="75c43-118">The compiler converts `@layout` into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="75c43-119">Il contenuto del componente seguente, l'oggetto `MainLayout` *master. Razor*, viene inserito nella posizione di: `@Body`</span><span class="sxs-lookup"><span data-stu-id="75c43-119">The content of the following component, *MasterList.razor*, is inserted into the `MainLayout` at the position of `@Body`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

## <a name="centralized-layout-selection"></a><span data-ttu-id="75c43-120">Selezione layout centralizzata</span><span class="sxs-lookup"><span data-stu-id="75c43-120">Centralized layout selection</span></span>

<span data-ttu-id="75c43-121">Ogni cartella di un'app può contenere facoltativamente un file modello denominato *_Imports. Razor*.</span><span class="sxs-lookup"><span data-stu-id="75c43-121">Every folder of an app can optionally contain a template file named *_Imports.razor*.</span></span> <span data-ttu-id="75c43-122">Il compilatore include le direttive specificate nel file di importazione in tutti i modelli Razor nella stessa cartella e in modo ricorsivo in tutte le relative sottocartelle.</span><span class="sxs-lookup"><span data-stu-id="75c43-122">The compiler includes the directives specified in the imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="75c43-123">Pertanto, un file *_Imports. Razor* contenente `@layout MainLayout` garantisce che tutti i componenti di una cartella usino. `MainLayout`</span><span class="sxs-lookup"><span data-stu-id="75c43-123">Therefore, an *_Imports.razor* file containing `@layout MainLayout` ensures that all of the components in a folder use `MainLayout`.</span></span> <span data-ttu-id="75c43-124">Non è necessario aggiungere `@layout MainLayout` ripetutamente a tutti i file con *estensione Razor* nella cartella e nelle sottocartelle.</span><span class="sxs-lookup"><span data-stu-id="75c43-124">There's no need to repeatedly add `@layout MainLayout` to all of the *.razor* files within the folder and subfolders.</span></span> <span data-ttu-id="75c43-125">`@using`le direttive vengono applicate anche ai componenti nello stesso modo.</span><span class="sxs-lookup"><span data-stu-id="75c43-125">`@using` directives are also applied to components in the same way.</span></span>

<span data-ttu-id="75c43-126">Il file *_Imports. Razor* seguente importa:</span><span class="sxs-lookup"><span data-stu-id="75c43-126">The following *_Imports.razor* file imports:</span></span>

* <span data-ttu-id="75c43-127">`MainLayout`.</span><span class="sxs-lookup"><span data-stu-id="75c43-127">`MainLayout`.</span></span>
* <span data-ttu-id="75c43-128">Tutti i componenti Razor nella stessa cartella e in tutte le sottocartelle.</span><span class="sxs-lookup"><span data-stu-id="75c43-128">All Razor components in the same folder and any subfolders.</span></span>
* <span data-ttu-id="75c43-129">Spazio dei nomi `BlazorApp1.Data` .</span><span class="sxs-lookup"><span data-stu-id="75c43-129">The `BlazorApp1.Data` namespace.</span></span>
 
[!code-cshtml[](layouts/sample_snapshot/3.x/_Imports.razor)]

<span data-ttu-id="75c43-130">Il file *_Imports. Razor* è simile al [file _ViewImports. cshtml per le visualizzazioni Razor e le pagine,](xref:mvc/views/layout#importing-shared-directives) ma viene applicato in modo specifico ai file del componente Razor.</span><span class="sxs-lookup"><span data-stu-id="75c43-130">The *_Imports.razor* file is similar to the [_ViewImports.cshtml file for Razor views and pages](xref:mvc/views/layout#importing-shared-directives) but applied specifically to Razor component files.</span></span>

<span data-ttu-id="75c43-131">I modelli Blazer usano i file *_Imports. Razor* per la selezione del layout.</span><span class="sxs-lookup"><span data-stu-id="75c43-131">The Blazor templates use *_Imports.razor* files for layout selection.</span></span> <span data-ttu-id="75c43-132">Un'app creata da un modello di Blazer contiene il file *_Imports. Razor* nella radice del progetto e nella cartella *pages* .</span><span class="sxs-lookup"><span data-stu-id="75c43-132">An app created from a Blazor template contains the *_Imports.razor* file in the root of the project and in the *Pages* folder.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="75c43-133">Layout annidati</span><span class="sxs-lookup"><span data-stu-id="75c43-133">Nested layouts</span></span>

<span data-ttu-id="75c43-134">Le app possono essere costituite da layout annidati.</span><span class="sxs-lookup"><span data-stu-id="75c43-134">Apps can consist of nested layouts.</span></span> <span data-ttu-id="75c43-135">Un componente può fare riferimento a un layout che a sua volta fa riferimento a un altro layout.</span><span class="sxs-lookup"><span data-stu-id="75c43-135">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="75c43-136">Ad esempio, i layout di annidamento vengono usati per creare una struttura di menu a più livelli.</span><span class="sxs-lookup"><span data-stu-id="75c43-136">For example, nesting layouts are used to create a multi-level menu structure.</span></span>

<span data-ttu-id="75c43-137">Nell'esempio seguente viene illustrato come utilizzare layout annidati.</span><span class="sxs-lookup"><span data-stu-id="75c43-137">The following example shows how to use nested layouts.</span></span> <span data-ttu-id="75c43-138">Il file *EpisodesComponent. Razor* è il componente da visualizzare.</span><span class="sxs-lookup"><span data-stu-id="75c43-138">The *EpisodesComponent.razor* file is the component to display.</span></span> <span data-ttu-id="75c43-139">Il componente fa riferimento `MasterListLayout`a:</span><span class="sxs-lookup"><span data-stu-id="75c43-139">The component references the `MasterListLayout`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

<span data-ttu-id="75c43-140">Il file *MasterListLayout. Razor* fornisce `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="75c43-140">The *MasterListLayout.razor* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="75c43-141">Il layout fa riferimento a un `MasterLayout`altro layout,, in cui viene eseguito il rendering.</span><span class="sxs-lookup"><span data-stu-id="75c43-141">The layout references another layout, `MasterLayout`, where it's rendered.</span></span> <span data-ttu-id="75c43-142">`EpisodesComponent`viene sottoposto `@Body` a rendering quando viene visualizzato:</span><span class="sxs-lookup"><span data-stu-id="75c43-142">`EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

<span data-ttu-id="75c43-143">Infine, `MasterLayout` in *MasterLayout. Razor* contiene gli elementi di layout di primo livello, ad esempio l'intestazione, il menu principale e il piè di pagina.</span><span class="sxs-lookup"><span data-stu-id="75c43-143">Finally, `MasterLayout` in *MasterLayout.razor* contains the top-level layout elements, such as the header, main menu, and footer.</span></span> <span data-ttu-id="75c43-144">`MasterListLayout`con `EpisodesComponent` viene eseguito il `@Body` rendering di quando viene visualizzato:</span><span class="sxs-lookup"><span data-stu-id="75c43-144">`MasterListLayout` with `EpisodesComponent` are rendered where `@Body` appears:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="additional-resources"></a><span data-ttu-id="75c43-145">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="75c43-145">Additional resources</span></span>

* <xref:mvc/views/layout>
