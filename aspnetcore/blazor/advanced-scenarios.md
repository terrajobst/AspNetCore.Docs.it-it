---
title: Scenari ASP.NET Core Blazor avanzati
author: guardrex
description: Informazioni sugli scenari avanzati in Blazor, inclusa la modalità di incorporamento della logica RenderTreeBuilder manuale in un'app.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/advanced-scenarios
ms.openlocfilehash: 5e0618faa7b1b5e4cc15e30d9c16afaf7ccabaf0
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453261"
---
# <a name="aspnet-core-blazor-advanced-scenarios"></a><span data-ttu-id="3a794-103">Scenari avanzati di ASP.NET Core Blazer</span><span class="sxs-lookup"><span data-stu-id="3a794-103">ASP.NET Core Blazor advanced scenarios</span></span>

<span data-ttu-id="3a794-104">Di [Luke Latham](https://github.com/guardrex) e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="3a794-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="3a794-105">Logica RenderTreeBuilder manuale</span><span class="sxs-lookup"><span data-stu-id="3a794-105">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="3a794-106">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` fornisce metodi per la modifica di componenti ed elementi, inclusa la compilazione manuale C# dei componenti nel codice.</span><span class="sxs-lookup"><span data-stu-id="3a794-106">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="3a794-107">L'uso di `RenderTreeBuilder` per creare componenti è uno scenario avanzato.</span><span class="sxs-lookup"><span data-stu-id="3a794-107">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="3a794-108">Un componente con formato non valido, ad esempio un tag di markup non chiuso, può causare un comportamento indefinito.</span><span class="sxs-lookup"><span data-stu-id="3a794-108">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="3a794-109">Si consideri il componente `PetDetails` seguente, che può essere incorporato manualmente in un altro componente:</span><span class="sxs-lookup"><span data-stu-id="3a794-109">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```razor
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="3a794-110">Nell'esempio seguente il ciclo nel metodo `CreateComponent` genera tre componenti `PetDetails`.</span><span class="sxs-lookup"><span data-stu-id="3a794-110">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="3a794-111">Quando si chiamano `RenderTreeBuilder` metodi per creare i componenti (`OpenComponent` e `AddAttribute`), i numeri di sequenza sono numeri di riga del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="3a794-111">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="3a794-112">L'algoritmo di differenza Blaze si basa sui numeri di sequenza corrispondenti a righe di codice distinte, non sulle chiamate di chiamata distinti.</span><span class="sxs-lookup"><span data-stu-id="3a794-112">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="3a794-113">Quando si crea un componente con `RenderTreeBuilder` metodi, impostare come hardcoded gli argomenti per i numeri di sequenza.</span><span class="sxs-lookup"><span data-stu-id="3a794-113">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="3a794-114">**L'utilizzo di un calcolo o di un contatore per generare il numero di sequenza può causare un calo delle prestazioni.**</span><span class="sxs-lookup"><span data-stu-id="3a794-114">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="3a794-115">Per ulteriori informazioni, vedere la sezione [numeri di sequenza correlati ai numeri di riga del codice e non all'ordine di esecuzione](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="3a794-115">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="3a794-116">componente `BuiltContent`:</span><span class="sxs-lookup"><span data-stu-id="3a794-116">`BuiltContent` component:</span></span>

```razor
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
    Create three Pet Details components
</button>

@code {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```

> [!WARNING]
> <span data-ttu-id="3a794-117">I tipi in `Microsoft.AspNetCore.Components.RenderTree` consentono l'elaborazione dei *risultati* delle operazioni di rendering.</span><span class="sxs-lookup"><span data-stu-id="3a794-117">The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="3a794-118">Questi sono i dettagli interni dell'implementazione del Framework Blazor.</span><span class="sxs-lookup"><span data-stu-id="3a794-118">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="3a794-119">Questi tipi devono essere considerati *instabili* e soggetti a modifiche nelle versioni future.</span><span class="sxs-lookup"><span data-stu-id="3a794-119">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="3a794-120">I numeri di sequenza sono correlati ai numeri di riga del codice e non all'ordine di esecuzione</span><span class="sxs-lookup"><span data-stu-id="3a794-120">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="3a794-121">I file dei componenti Razor (*Razor*) vengono sempre compilati.</span><span class="sxs-lookup"><span data-stu-id="3a794-121">Razor component files (*.razor*) are always compiled.</span></span> <span data-ttu-id="3a794-122">La compilazione è un potenziale vantaggio rispetto all'interpretazione del codice perché il passaggio di compilazione può essere usato per inserire informazioni che migliorano le prestazioni dell'app in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="3a794-122">Compilation is a potential advantage over interpreting code because the compile step can be used to inject information that improves app performance at runtime.</span></span>

<span data-ttu-id="3a794-123">Un esempio fondamentale di questi miglioramenti riguarda i *numeri di sequenza*.</span><span class="sxs-lookup"><span data-stu-id="3a794-123">A key example of these improvements involves *sequence numbers*.</span></span> <span data-ttu-id="3a794-124">I numeri di sequenza indicano al runtime quali output provengono da righe di codice distinte e ordinate.</span><span class="sxs-lookup"><span data-stu-id="3a794-124">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="3a794-125">Il runtime usa queste informazioni per generare differenze di albero efficienti nel tempo lineare, che è molto più veloce rispetto a quanto normalmente è possibile per un algoritmo diff della struttura ad albero generale.</span><span class="sxs-lookup"><span data-stu-id="3a794-125">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="3a794-126">Si consideri il seguente file di componente Razor (*Razor*):</span><span class="sxs-lookup"><span data-stu-id="3a794-126">Consider the following Razor component (*.razor*) file:</span></span>

```razor
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="3a794-127">Il codice precedente viene compilato in un modo simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="3a794-127">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="3a794-128">Quando il codice viene eseguito per la prima volta, se `someFlag` è `true`, il generatore riceve:</span><span class="sxs-lookup"><span data-stu-id="3a794-128">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="3a794-129">Sequence</span><span class="sxs-lookup"><span data-stu-id="3a794-129">Sequence</span></span> | <span data-ttu-id="3a794-130">Type</span><span class="sxs-lookup"><span data-stu-id="3a794-130">Type</span></span>      | <span data-ttu-id="3a794-131">Data</span><span class="sxs-lookup"><span data-stu-id="3a794-131">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="3a794-132">0</span><span class="sxs-lookup"><span data-stu-id="3a794-132">0</span></span>        | <span data-ttu-id="3a794-133">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="3a794-133">Text node</span></span> | <span data-ttu-id="3a794-134">First</span><span class="sxs-lookup"><span data-stu-id="3a794-134">First</span></span>  |
| <span data-ttu-id="3a794-135">1</span><span class="sxs-lookup"><span data-stu-id="3a794-135">1</span></span>        | <span data-ttu-id="3a794-136">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="3a794-136">Text node</span></span> | <span data-ttu-id="3a794-137">Second</span><span class="sxs-lookup"><span data-stu-id="3a794-137">Second</span></span> |

<span data-ttu-id="3a794-138">Si supponga che `someFlag` diventi `false`e che venga nuovamente eseguito il rendering del markup.</span><span class="sxs-lookup"><span data-stu-id="3a794-138">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="3a794-139">Questa volta, il generatore riceve:</span><span class="sxs-lookup"><span data-stu-id="3a794-139">This time, the builder receives:</span></span>

| <span data-ttu-id="3a794-140">Sequence</span><span class="sxs-lookup"><span data-stu-id="3a794-140">Sequence</span></span> | <span data-ttu-id="3a794-141">Type</span><span class="sxs-lookup"><span data-stu-id="3a794-141">Type</span></span>       | <span data-ttu-id="3a794-142">Data</span><span class="sxs-lookup"><span data-stu-id="3a794-142">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="3a794-143">1</span><span class="sxs-lookup"><span data-stu-id="3a794-143">1</span></span>        | <span data-ttu-id="3a794-144">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="3a794-144">Text node</span></span>  | <span data-ttu-id="3a794-145">Second</span><span class="sxs-lookup"><span data-stu-id="3a794-145">Second</span></span> |

<span data-ttu-id="3a794-146">Quando il runtime esegue una diff, rileva che l'elemento in sequenza `0` è stato rimosso, quindi genera lo *script di modifica*semplice seguente:</span><span class="sxs-lookup"><span data-stu-id="3a794-146">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="3a794-147">Rimuovere il primo nodo di testo.</span><span class="sxs-lookup"><span data-stu-id="3a794-147">Remove the first text node.</span></span>

### <a name="the-problem-with-generating-sequence-numbers-programmatically"></a><span data-ttu-id="3a794-148">Il problema della generazione di numeri di sequenza a livello di codice</span><span class="sxs-lookup"><span data-stu-id="3a794-148">The problem with generating sequence numbers programmatically</span></span>

<span data-ttu-id="3a794-149">Si supponga invece che sia stata scritta la logica del generatore di albero di rendering seguente:</span><span class="sxs-lookup"><span data-stu-id="3a794-149">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="3a794-150">A questo punto, il primo output è:</span><span class="sxs-lookup"><span data-stu-id="3a794-150">Now, the first output is:</span></span>

| <span data-ttu-id="3a794-151">Sequence</span><span class="sxs-lookup"><span data-stu-id="3a794-151">Sequence</span></span> | <span data-ttu-id="3a794-152">Type</span><span class="sxs-lookup"><span data-stu-id="3a794-152">Type</span></span>      | <span data-ttu-id="3a794-153">Data</span><span class="sxs-lookup"><span data-stu-id="3a794-153">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="3a794-154">0</span><span class="sxs-lookup"><span data-stu-id="3a794-154">0</span></span>        | <span data-ttu-id="3a794-155">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="3a794-155">Text node</span></span> | <span data-ttu-id="3a794-156">First</span><span class="sxs-lookup"><span data-stu-id="3a794-156">First</span></span>  |
| <span data-ttu-id="3a794-157">1</span><span class="sxs-lookup"><span data-stu-id="3a794-157">1</span></span>        | <span data-ttu-id="3a794-158">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="3a794-158">Text node</span></span> | <span data-ttu-id="3a794-159">Second</span><span class="sxs-lookup"><span data-stu-id="3a794-159">Second</span></span> |

<span data-ttu-id="3a794-160">Questo risultato è identico al caso precedente, pertanto non esistono problemi negativi.</span><span class="sxs-lookup"><span data-stu-id="3a794-160">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="3a794-161">`someFlag` è `false` sul secondo rendering e l'output è:</span><span class="sxs-lookup"><span data-stu-id="3a794-161">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="3a794-162">Sequence</span><span class="sxs-lookup"><span data-stu-id="3a794-162">Sequence</span></span> | <span data-ttu-id="3a794-163">Type</span><span class="sxs-lookup"><span data-stu-id="3a794-163">Type</span></span>      | <span data-ttu-id="3a794-164">Data</span><span class="sxs-lookup"><span data-stu-id="3a794-164">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="3a794-165">0</span><span class="sxs-lookup"><span data-stu-id="3a794-165">0</span></span>        | <span data-ttu-id="3a794-166">Nodo testo</span><span class="sxs-lookup"><span data-stu-id="3a794-166">Text node</span></span> | <span data-ttu-id="3a794-167">Second</span><span class="sxs-lookup"><span data-stu-id="3a794-167">Second</span></span> |

<span data-ttu-id="3a794-168">Questa volta, l'algoritmo Diff rileva che si sono verificate *due* modifiche e l'algoritmo genera lo script di modifica seguente:</span><span class="sxs-lookup"><span data-stu-id="3a794-168">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="3a794-169">Modificare il valore del primo nodo di testo in `Second`.</span><span class="sxs-lookup"><span data-stu-id="3a794-169">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="3a794-170">Rimuovere il secondo nodo di testo.</span><span class="sxs-lookup"><span data-stu-id="3a794-170">Remove the second text node.</span></span>

<span data-ttu-id="3a794-171">La generazione dei numeri di sequenza ha perso tutte le informazioni utili sul punto in cui i `if/else` rami e i cicli erano presenti nel codice originale.</span><span class="sxs-lookup"><span data-stu-id="3a794-171">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="3a794-172">In questo modo si ottiene una differenza **due volte più a lungo** .</span><span class="sxs-lookup"><span data-stu-id="3a794-172">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="3a794-173">Questo è un esempio semplice.</span><span class="sxs-lookup"><span data-stu-id="3a794-173">This is a trivial example.</span></span> <span data-ttu-id="3a794-174">Nei casi più realistici con strutture complesse e profondamente annidate e soprattutto con i cicli, il costo delle prestazioni è in genere più elevato.</span><span class="sxs-lookup"><span data-stu-id="3a794-174">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is usually higher.</span></span> <span data-ttu-id="3a794-175">Invece di identificare immediatamente i blocchi o i rami del ciclo che sono stati inseriti o rimossi, l'algoritmo Diff deve ripresentarsi in modo approfondito negli alberi di rendering.</span><span class="sxs-lookup"><span data-stu-id="3a794-175">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees.</span></span> <span data-ttu-id="3a794-176">Ciò comporta in genere la necessità di compilare script di modifica più lunghi perché l'algoritmo Diff viene informato in modo non più approfondito sulle relazioni tra le vecchie e le nuove strutture.</span><span class="sxs-lookup"><span data-stu-id="3a794-176">This usually results in having to build longer edit scripts because the diff algorithm is misinformed about how the old and new structures relate to each other.</span></span>

### <a name="guidance-and-conclusions"></a><span data-ttu-id="3a794-177">Linee guida e conclusioni</span><span class="sxs-lookup"><span data-stu-id="3a794-177">Guidance and conclusions</span></span>

* <span data-ttu-id="3a794-178">Le prestazioni dell'app soffrono se i numeri di sequenza vengono generati dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="3a794-178">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="3a794-179">Il Framework non è in grado di creare automaticamente i propri numeri di sequenza in fase di esecuzione perché le informazioni necessarie non esistono a meno che non vengano acquisite in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="3a794-179">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="3a794-180">Non scrivere blocchi lunghi di logica di `RenderTreeBuilder` implementata manualmente.</span><span class="sxs-lookup"><span data-stu-id="3a794-180">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="3a794-181">Preferire i file *Razor* e consentire al compilatore di gestire i numeri di sequenza.</span><span class="sxs-lookup"><span data-stu-id="3a794-181">Prefer *.razor* files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="3a794-182">Se non si è in grado di evitare la logica manuale `RenderTreeBuilder`, suddividere i blocchi di codice lunghi in parti più piccole racchiuse in `OpenRegion`/chiamate `CloseRegion`.</span><span class="sxs-lookup"><span data-stu-id="3a794-182">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="3a794-183">Ogni area ha il proprio spazio separato dei numeri di sequenza, quindi è possibile riavviare da zero (o qualsiasi altro numero arbitrario) all'interno di ogni area.</span><span class="sxs-lookup"><span data-stu-id="3a794-183">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="3a794-184">Se i numeri di sequenza sono hardcoded, l'algoritmo Diff richiede solo che i numeri di sequenza aumentino nel valore.</span><span class="sxs-lookup"><span data-stu-id="3a794-184">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="3a794-185">Il valore iniziale e i gap sono irrilevanti.</span><span class="sxs-lookup"><span data-stu-id="3a794-185">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="3a794-186">Una delle opzioni legittime consiste nell'usare il numero di riga del codice come numero di sequenza oppure iniziare da zero e aumentare di uno o di centinaia (o qualsiasi intervallo preferito).</span><span class="sxs-lookup"><span data-stu-id="3a794-186">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* Blazor<span data-ttu-id="3a794-187"> usa i numeri di sequenza, mentre altri Framework dell'interfaccia utente per le differenze tra gli alberi non li usano.</span><span class="sxs-lookup"><span data-stu-id="3a794-187"> uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="3a794-188">La diffing è molto più veloce quando si usano i numeri di sequenza e Blazor ha il vantaggio di un passaggio di compilazione che gestisce automaticamente i numeri di sequenza per gli sviluppatori che creano file con *estensione Razor* .</span><span class="sxs-lookup"><span data-stu-id="3a794-188">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring *.razor* files.</span></span>
