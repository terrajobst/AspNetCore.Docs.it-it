---
title: Layout di Razor componenti
author: guardrex
description: Informazioni su come creare componenti riutilizzabili di layout per App i componenti di Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/layouts
ms.openlocfilehash: 31ed940ce40e3ae6e3744418cf241d396308f4fe
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515524"
---
# <a name="razor-components-layouts"></a><span data-ttu-id="1ce98-103">Layout di Razor componenti</span><span class="sxs-lookup"><span data-stu-id="1ce98-103">Razor Components layouts</span></span>

<span data-ttu-id="1ce98-104">Da [Stropek Rainer](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="1ce98-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="1ce98-105">Le App in genere contengono più di un componente.</span><span class="sxs-lookup"><span data-stu-id="1ce98-105">Apps typically contain more than one component.</span></span> <span data-ttu-id="1ce98-106">Gli elementi di layout, ad esempio menu, i messaggi sul copyright e i loghi, devono essere presenti in tutti i componenti.</span><span class="sxs-lookup"><span data-stu-id="1ce98-106">Layout elements, such as menus, copyright messages, and logos, must be present on all components.</span></span> <span data-ttu-id="1ce98-107">Copia il codice di questi elementi di layout in tutti i componenti di un'app non è un approccio efficace.</span><span class="sxs-lookup"><span data-stu-id="1ce98-107">Copying the code of these layout elements into all of the components of an app isn't an efficient approach.</span></span> <span data-ttu-id="1ce98-108">Tale funzionalità è difficile da gestire e probabilmente conduce al contenuto incoerente nel corso del tempo.</span><span class="sxs-lookup"><span data-stu-id="1ce98-108">Such duplication is hard to maintain and probably leads to inconsistent content over time.</span></span> <span data-ttu-id="1ce98-109">*Layout* risolvere questo problema.</span><span class="sxs-lookup"><span data-stu-id="1ce98-109">*Layouts* solve this problem.</span></span>

<span data-ttu-id="1ce98-110">Tecnicamente, un layout è semplicemente un altro componente.</span><span class="sxs-lookup"><span data-stu-id="1ce98-110">Technically, a layout is just another component.</span></span> <span data-ttu-id="1ce98-111">Un layout è definito in un modello Razor o in C# del codice e può contenere [associazione di dati](xref:razor-components/components#data-binding), [inserimento di dipendenze](xref:razor-components/dependency-injection)e altre funzionalità comuni dei componenti.</span><span class="sxs-lookup"><span data-stu-id="1ce98-111">A layout is defined in a Razor template or in C# code and can contain [data binding](xref:razor-components/components#data-binding), [dependency injection](xref:razor-components/dependency-injection), and other ordinary features of components.</span></span>

<span data-ttu-id="1ce98-112">Due aspetti aggiuntivi attiva una *component* in un *layout*</span><span class="sxs-lookup"><span data-stu-id="1ce98-112">Two additional aspects turn a *component* into a *layout*</span></span>

* <span data-ttu-id="1ce98-113">Il componente del layout deve ereditare da `LayoutComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="1ce98-113">The layout component must inherit from `LayoutComponentBase`.</span></span> `LayoutComponentBase` <span data-ttu-id="1ce98-114">definisce un `Body` proprietà che contiene il contenuto da sottoporre a rendering all'interno del layout.</span><span class="sxs-lookup"><span data-stu-id="1ce98-114">defines a `Body` property that contains the content to be rendered inside the layout.</span></span>
* <span data-ttu-id="1ce98-115">Il componente layout utilizza il `Body` proprietà per specificare dove deve essere il contenuto del corpo viene eseguito il rendering tramite la sintassi Razor `@Body`.</span><span class="sxs-lookup"><span data-stu-id="1ce98-115">The layout component uses the `Body` property to specify where the body content should be rendered using the Razor syntax `@Body`.</span></span> <span data-ttu-id="1ce98-116">Durante il rendering, `@Body` viene sostituito dal contenuto del layout.</span><span class="sxs-lookup"><span data-stu-id="1ce98-116">During rendering, `@Body` is replaced by the content of the layout.</span></span>

<span data-ttu-id="1ce98-117">Esempio di codice seguente viene illustrato il modello di un componente del layout Razor.</span><span class="sxs-lookup"><span data-stu-id="1ce98-117">The following code sample shows the Razor template of a layout component.</span></span> <span data-ttu-id="1ce98-118">Si noti l'uso del `LayoutComponentBase` e `@Body`:</span><span class="sxs-lookup"><span data-stu-id="1ce98-118">Note the use of `LayoutComponentBase` and `@Body`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.cshtml)]

## <a name="use-a-layout-in-a-component"></a><span data-ttu-id="1ce98-119">Usare un layout in un componente</span><span class="sxs-lookup"><span data-stu-id="1ce98-119">Use a layout in a component</span></span>

<span data-ttu-id="1ce98-120">Usare la direttiva Razor `@layout` per applicare un layout a un componente.</span><span class="sxs-lookup"><span data-stu-id="1ce98-120">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="1ce98-121">Il compilatore converte questa direttiva in un `LayoutAttribute`, cui viene applicata alla classe del componente.</span><span class="sxs-lookup"><span data-stu-id="1ce98-121">The compiler converts this directive into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="1ce98-122">Esempio di codice seguente viene illustrato il concetto.</span><span class="sxs-lookup"><span data-stu-id="1ce98-122">The following code sample demonstrates the concept.</span></span> <span data-ttu-id="1ce98-123">Il contenuto di questo componente viene inserito il *MasterLayout* in corrispondenza della posizione di `@Body`:</span><span class="sxs-lookup"><span data-stu-id="1ce98-123">The content of this component is inserted into the *MasterLayout* at the position of `@Body`:</span></span>

```cshtml
@layout MasterLayout
@page "/master-list"

<h2>Master Episode List</h2>
```

## <a name="centralized-layout-selection"></a><span data-ttu-id="1ce98-124">Selezione di layout centralizzato</span><span class="sxs-lookup"><span data-stu-id="1ce98-124">Centralized layout selection</span></span>

<span data-ttu-id="1ce98-125">Ogni cartella di un un'app può contenere un file di modello denominato *viewimports. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1ce98-125">Every folder of a an app can optionally contain a template file named *_ViewImports.cshtml*.</span></span> <span data-ttu-id="1ce98-126">Il compilatore include le direttive specificate nel file di importazioni di visualizzazione in tutti i modelli Razor nella stessa cartella e in modo ricorsivo in tutte le relative sottocartelle.</span><span class="sxs-lookup"><span data-stu-id="1ce98-126">The compiler includes the directives specified in the view imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="1ce98-127">Pertanto, un *viewimports. cshtml* file contenente `@layout MainLayout` assicura che tutti i componenti in un cartella, usare i *MainLayout* layout.</span><span class="sxs-lookup"><span data-stu-id="1ce98-127">Therefore, a *_ViewImports.cshtml* file containing `@layout MainLayout` ensures that all of the components in a folder use the *MainLayout* layout.</span></span> <span data-ttu-id="1ce98-128">Non è necessario aggiungere ripetutamente `@layout` a tutte le *.razor* file.</span><span class="sxs-lookup"><span data-stu-id="1ce98-128">There's no need to repeatedly add `@layout` to all of the *.razor* files.</span></span>

<span data-ttu-id="1ce98-129">Si noti che il modello predefinito Usa la *viewimports. cshtml* meccanismo per la selezione di layout.</span><span class="sxs-lookup"><span data-stu-id="1ce98-129">Note that the default template uses the *_ViewImports.cshtml* mechanism for layout selection.</span></span> <span data-ttu-id="1ce98-130">Un'app appena creata contiene il *viewimports. cshtml* del file nel *componenti o delle pagine* cartella.</span><span class="sxs-lookup"><span data-stu-id="1ce98-130">A newly created app contains the *_ViewImports.cshtml* file in the *Components/Pages* folder.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="1ce98-131">Layout annidati</span><span class="sxs-lookup"><span data-stu-id="1ce98-131">Nested layouts</span></span>

<span data-ttu-id="1ce98-132">Le app possono essere costituito da layout annidati.</span><span class="sxs-lookup"><span data-stu-id="1ce98-132">Apps can consist of nested layouts.</span></span> <span data-ttu-id="1ce98-133">Un componente può fare riferimento a un layout che a sua volta fa riferimento a un altro layout.</span><span class="sxs-lookup"><span data-stu-id="1ce98-133">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="1ce98-134">Ad esempio, il layout di annidamento è utilizzabile in base a una struttura a più livelli.</span><span class="sxs-lookup"><span data-stu-id="1ce98-134">For example, nesting layouts can be used to reflect a multi-level menu structure.</span></span>

<span data-ttu-id="1ce98-135">Esempi di codice seguenti mostrano come usare layout annidati.</span><span class="sxs-lookup"><span data-stu-id="1ce98-135">The following code samples show how to use nested layouts.</span></span> <span data-ttu-id="1ce98-136">Il *EpisodesComponent.cshtml* file è il componente da visualizzare.</span><span class="sxs-lookup"><span data-stu-id="1ce98-136">The *EpisodesComponent.cshtml* file is the component to display.</span></span> <span data-ttu-id="1ce98-137">Si noti che il componente vi fa riferimento il layout `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="1ce98-137">Note that the component references the layout `MasterListLayout`.</span></span>

<span data-ttu-id="1ce98-138">*EpisodesComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1ce98-138">*EpisodesComponent.cshtml*:</span></span>

```cshtml
@layout MasterListLayout
@page "/master-list/episodes"

<h1>Episodes</h1>
```

<span data-ttu-id="1ce98-139">Il *MasterListLayout.cshtml* fornisce file di `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="1ce98-139">The *MasterListLayout.cshtml* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="1ce98-140">Il layout fa riferimento a un altro layout, `MasterLayout`, in cui verrà incorporato.</span><span class="sxs-lookup"><span data-stu-id="1ce98-140">The layout references another layout, `MasterLayout`, where it's going to be embedded.</span></span>

<span data-ttu-id="1ce98-141">*MasterListLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1ce98-141">*MasterListLayout.cshtml*:</span></span>

```cshtml
@layout MasterLayout
@inherits LayoutComponentBase

<nav>
    <!-- Menu structure of master list -->
    ...
</nav>

@Body
```

<span data-ttu-id="1ce98-142">Infine, `MasterLayout` contiene gli elementi di layout di primo livello, ad esempio l'intestazione, piè di pagina e menu principale.</span><span class="sxs-lookup"><span data-stu-id="1ce98-142">Finally, `MasterLayout` contains the top-level layout elements, such as the header, footer, and main menu.</span></span>

<span data-ttu-id="1ce98-143">*MasterLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1ce98-143">*MasterLayout.cshtml*:</span></span>

```cshtml
@inherits LayoutComponentBase

<header>...</header>
<nav>...</nav>

@Body
```
