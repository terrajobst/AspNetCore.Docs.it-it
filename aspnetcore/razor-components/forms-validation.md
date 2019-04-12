---
title: Convalida e razor componenti form
author: guardrex
description: Informazioni su come usare moduli e gli scenari di convalida campo nei componenti di Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: razor-components/forms-validation
ms.openlocfilehash: a4ed75efc6b5a733ce4ff4e82f354a8e2fd48500
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515542"
---
# <a name="razor-components-forms-and-validation"></a><span data-ttu-id="919b0-103">Convalida e razor componenti form</span><span class="sxs-lookup"><span data-stu-id="919b0-103">Razor Components forms and validation</span></span>

<span data-ttu-id="919b0-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="919b0-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="919b0-105">Moduli e convalida sono supportati nei componenti di Razor usando [annotazioni dei dati](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="919b0-105">Forms and validation are supported in Razor Components using [data annotations](xref:mvc/models/validation).</span></span>

> [!NOTE]
> <span data-ttu-id="919b0-106">Moduli e gli scenari di convalida sono soggette a modifica a ogni versione di anteprima.</span><span class="sxs-lookup"><span data-stu-id="919b0-106">Forms and validation scenarios are likely to change with each preview release.</span></span>

<span data-ttu-id="919b0-107">Nell'esempio `ExampleModel` tipo definisce la logica di convalida tramite le annotazioni dei dati:</span><span class="sxs-lookup"><span data-stu-id="919b0-107">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="919b0-108">Un modulo viene definito mediante il `<EditForm>` componente.</span><span class="sxs-lookup"><span data-stu-id="919b0-108">A form is defined using the `<EditForm>` component.</span></span> <span data-ttu-id="919b0-109">Elementi tipici, componenti e codice Razor, viene illustrato il formato seguente:</span><span class="sxs-lookup"><span data-stu-id="919b0-109">The following form demonstrates typical elements, components, and Razor code:</span></span>

```csharp
<EditForm Model="@exampleModel" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" bind-Value="@exampleModel.Name" />

    <button type="submit">Submit</button>
</EditForm>

@functions {
    private ExampleModel exampleModel = new ExampleModel();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

* <span data-ttu-id="919b0-110">Il modulo convalida l'input utente nel `name` campo usando il processo di convalida definito nel `ExampleModel` tipo.</span><span class="sxs-lookup"><span data-stu-id="919b0-110">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="919b0-111">La creazione del modello nella proprietà del componente `@functions` bloccare e contenuti in un campo privato (`exampleModel`).</span><span class="sxs-lookup"><span data-stu-id="919b0-111">The model is created in the component's `@functions` block and held in a private field (`exampleModel`).</span></span> <span data-ttu-id="919b0-112">È assegnato il campo per il `Model` attributo del `<EditForm>`.</span><span class="sxs-lookup"><span data-stu-id="919b0-112">The field is assigned to the `Model` attribute of the `<EditForm>`.</span></span>
* <span data-ttu-id="919b0-113">Il componente del Validator di annotazioni dei dati (`<DataAnnotationsValidator>`) collega supporto per la convalida tramite le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="919b0-113">The Data Annotations Validator component (`<DataAnnotationsValidator>`) attaches validation support using data annotations.</span></span>
* <span data-ttu-id="919b0-114">Il componente di riepilogo della convalida (`<ValidationSummary>`) vengono riepilogati i messaggi di convalida.</span><span class="sxs-lookup"><span data-stu-id="919b0-114">The Validation Summary component (`<ValidationSummary>`) summarizes validation messages.</span></span>
* `HandleValidSubmit` <span data-ttu-id="919b0-115">viene attivato quando il form invia correttamente (supera la convalida).</span><span class="sxs-lookup"><span data-stu-id="919b0-115">is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="919b0-116">Un set di componenti di input predefiniti sono disponibili per ricevere e convalidare l'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="919b0-116">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="919b0-117">Gli input vengono convalidati quando si è modificati e quando viene inviato un modulo.</span><span class="sxs-lookup"><span data-stu-id="919b0-117">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="919b0-118">Nella tabella seguente vengono visualizzati i componenti di input disponibili.</span><span class="sxs-lookup"><span data-stu-id="919b0-118">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="919b0-119">Componente di input</span><span class="sxs-lookup"><span data-stu-id="919b0-119">Input component</span></span>   | <span data-ttu-id="919b0-120">Sottoposto a rendering come&hellip;</span><span class="sxs-lookup"><span data-stu-id="919b0-120">Rendered as&hellip;</span></span>       |
| ----------------- | ------------------------- |
| `<InputText>`     | `<input>`                 |
| `<InputTextArea>` | `<textarea>`              |
| `<InputSelect>`   | `<select>`                |
| `<InputNumber>`   | `<input type="number">`   |
| `<InputCheckbox>` | `<input type="checkbox">` |
| `<InputDate>`     | `<input type="date">`     |

<span data-ttu-id="919b0-121">Componenti di input specificare il comportamento predefinito per la convalida su una modifica e modificando la relativa classe CSS per riflettere lo stato di campo.</span><span class="sxs-lookup"><span data-stu-id="919b0-121">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="919b0-122">Alcuni componenti di includono logica di analisi utile.</span><span class="sxs-lookup"><span data-stu-id="919b0-122">Some components include useful parsing logic.</span></span> <span data-ttu-id="919b0-123">Ad esempio, `<InputDate>` e `<InputNumber>` gestire correttamente i valori non analizzabile registrandoli come errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="919b0-123">For example, `<InputDate>` and `<InputNumber>` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="919b0-124">I tipi che possono accettare valori null supportano anche i valori null del campo di destinazione (ad esempio, `int?`).</span><span class="sxs-lookup"><span data-stu-id="919b0-124">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="919b0-125">Quanto segue `Starship` tipo definisce la logica di convalida usando un set più ampio di proprietà e [annotazioni dei dati](xref:mvc/models/validation) del precedente `ExampleModel`:</span><span class="sxs-lookup"><span data-stu-id="919b0-125">The following `Starship` type defines validation logic using a larger set of properties and [data annotations](xref:mvc/models/validation) than the earlier `ExampleModel`:</span></span>

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    [Required]
    [StringLength(16, 
        ErrorMessage = "Identifier too long (16 character limit).")]
    public string Identifier { get; set; }

    // Optional (no data annotations)
    public string Description { get; set; }

    [Required]
    public string Classification { get; set; }

    [Range(1, 100000, ErrorMessage = "Accommodation invalid (1-100000).")]
    public int MaximumAccommodation { get; set; }

    [Required]
    [Range(typeof(bool), "true", "true", 
        ErrorMessage = "Form disallowed for unapproved ships.")]
    public bool IsValidatedDesign { get; set; }

    [Required]
    public DateTime ProductionDate { get; set; }
}
```

<span data-ttu-id="919b0-126">Il formato seguente convalida input dell'utente tramite il processo di convalida definito nel `Starship` modello:</span><span class="sxs-lookup"><span data-stu-id="919b0-126">The following form validates user input using the validation defined in the `Starship` model:</span></span>

```cshtml
@page "/FormsValidation"

<h1>Starfleet Starship Database</h1>

<h2>New Ship Entry Form</h2>

<EditForm Model="@starship" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label for="identifier">Identifier: </label>
        <InputText id="identifier" bind-Value="@starship.Identifier" />
    </p>
    <p>
        <label for="description">Description (optional): </label>
        <InputTextArea Id="description" bind-Value="@starship.Description" />
    </p>
    <p>
        <label for="classification">Primary Classification: </label>
        <InputSelect id="classification" bind-Value="@starship.Classification">
            <option value"">Select classification ...</option>
            <option value="Defense">Defense</option>
            <option value="Exploration">Exploration</option>
            <option value="Diplomacy">Diplomacy</option>
        </InputSelect>
    </p>
    <p>
        <label for="accommodation">Maximum Accommodation: </label>
        <InputNumber id="accommodation" 
            bind-Value="@starship.MaximumAccommodation" />
    </p>
    <p>
        <label for="valid">Engineering Approval: </label>
        <InputCheckbox id="valid" bind-Value="@starship.IsValidatedDesign" />
    </p>
    <p>
        <label for="productionDate">Production Date: </label>
        <InputDate Id="productionDate" bind-Value="@starship.ProductionDate" />
    </p>

    <button type="submit">Submit</button>

    <p>
        <a href="http://www.startrek.com/">Star Trek</a>, 
        &copy;1966-2019 CBS Studios, Inc. and 
        <a href="http://www.paramount.com">Paramount Pictures</a>
    </p>
</EditForm>

@functions {
    private Starship starship = new Starship();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

<span data-ttu-id="919b0-127">Il `<EditForm>` crea un' `EditContext` come una [valore CSS](xref:razor-components/components#cascading-values-and-parameters) che tiene traccia dei metadati sul processo di modifica, tra cui i campi che sono stati modificati e i messaggi di convalida corrente.</span><span class="sxs-lookup"><span data-stu-id="919b0-127">The `<EditForm>` creates an `EditContext` as a [cascading value](xref:razor-components/components#cascading-values-and-parameters) that tracks metadata about the edit process, including what fields have been modified and the current validation messages.</span></span> <span data-ttu-id="919b0-128">Il `<EditForm>` fornisce anche eventi pratici per Invia valido e non è valido (`OnValidSubmit`, `OnInvalidSubmit`).</span><span class="sxs-lookup"><span data-stu-id="919b0-128">The `<EditForm>` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="919b0-129">In alternativa, usare `OnSubmit` per attivare la convalida e controllare i valori dei campi con codice di convalida personalizzato.</span><span class="sxs-lookup"><span data-stu-id="919b0-129">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

<span data-ttu-id="919b0-130">Il componente del Validator di annotazioni dei dati (`<DataAnnotationsValidator>`) collega supporto per la convalida tramite le annotazioni dei dati per il sottomenu `EditContext`.</span><span class="sxs-lookup"><span data-stu-id="919b0-130">The Data Annotations Validator component (`<DataAnnotationsValidator>`) attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="919b0-131">Abilitazione del supporto per la convalida tramite le annotazioni dei dati attualmente richiede questo movimento esplicito, ma si sta considerando rende questo il comportamento predefinito che è quindi possibile eseguire l'override.</span><span class="sxs-lookup"><span data-stu-id="919b0-131">Enabling support for validation using data annotations currently requires this explicit gesture, but we're considering making this the default behavior that you can then override.</span></span> <span data-ttu-id="919b0-132">Per usare un sistema di convalida diverso rispetto a annotazioni dei dati, sostituire il Validator di annotazioni dei dati con un'implementazione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="919b0-132">To use a different validation system than data annotations, replace the Data Annotations Validator with a custom implementation.</span></span> <span data-ttu-id="919b0-133">L'implementazione di ASP.NET Core è disponibile per l'ispezione dell'origine di riferimento: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="919b0-133">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs).</span></span> *<span data-ttu-id="919b0-134">L'implementazione di ASP.NET Core è soggetto agli aggiornamenti rapidi durante il periodo di versione di anteprima.</span><span class="sxs-lookup"><span data-stu-id="919b0-134">The ASP.NET Core implementation is subject to rapid updates during the preview release period.</span></span>*

<span data-ttu-id="919b0-135">Il componente di riepilogo della convalida (`<ValidationSummary>`) vengono riepilogati tutti i messaggi di convalida, che è simile al [Helper per Tag riepilogo di convalida](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="919b0-135">The Validation Summary component (`<ValidationSummary>`) summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span></span>

<span data-ttu-id="919b0-136">Il componente di convalida (`<ValidationMessage>`) consente di visualizzare i messaggi di convalida per un campo specifico, che è simile al [Helper Tag Validation Message](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="919b0-136">The Validation Message component (`<ValidationMessage>`) displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="919b0-137">Specificare il campo per la convalida con il `For` attributo e un'espressione lambda la proprietà del modello di denominazione:</span><span class="sxs-lookup"><span data-stu-id="919b0-137">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

> [!NOTE]
> <span data-ttu-id="919b0-138">Componenti predefiniti di input presentano delle limitazioni che si prevedono di risolvere nelle future versioni.</span><span class="sxs-lookup"><span data-stu-id="919b0-138">Built-in input components have limitations that we expect to resolve in future releases.</span></span> <span data-ttu-id="919b0-139">Ad esempio, è possibile specificare gli attributi arbitrari su generato `<input>` tag.</span><span class="sxs-lookup"><span data-stu-id="919b0-139">For example, you can't specify arbitrary attributes on the generated `<input>` tags.</span></span> <span data-ttu-id="919b0-140">Compilare il proprio le sottoclassi di componenti per gestire scenari non disponibili.</span><span class="sxs-lookup"><span data-stu-id="919b0-140">Build your own component subclasses to handle unavailable scenarios.</span></span>
