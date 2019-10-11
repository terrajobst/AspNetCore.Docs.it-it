---
title: Moduli e convalida di ASP.NET Core Blazor
author: guardrex
description: Informazioni su come usare i moduli e gli scenari di convalida del campo in blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/forms-validation
ms.openlocfilehash: 6f6ace3deb7ed4262b643d897273bc767334b5e6
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2019
ms.locfileid: "71207183"
---
# <a name="aspnet-core-blazor-forms-and-validation"></a><span data-ttu-id="597dd-103">Moduli e convalida di ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="597dd-103">ASP.NET Core Blazor forms and validation</span></span>

<span data-ttu-id="597dd-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="597dd-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="597dd-105">I moduli e la convalida sono supportati in Blazor usando le [annotazioni dei dati](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="597dd-105">Forms and validation are supported in Blazor using [data annotations](xref:mvc/models/validation).</span></span>

<span data-ttu-id="597dd-106">Il tipo `ExampleModel` seguente definisce la logica di convalida usando le annotazioni dei dati:</span><span class="sxs-lookup"><span data-stu-id="597dd-106">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="597dd-107">Un modulo viene definito usando il `EditForm` componente.</span><span class="sxs-lookup"><span data-stu-id="597dd-107">A form is defined using the `EditForm` component.</span></span> <span data-ttu-id="597dd-108">Il modulo seguente illustra elementi tipici, componenti e codice Razor:</span><span class="sxs-lookup"><span data-stu-id="597dd-108">The following form demonstrates typical elements, components, and Razor code:</span></span>

```csharp
<EditForm Model="@exampleModel" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" @bind-Value="@exampleModel.Name" />

    <button type="submit">Submit</button>
</EditForm>

@code {
    private ExampleModel exampleModel = new ExampleModel();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

* <span data-ttu-id="597dd-109">Il modulo convalida l'input dell'utente nel `name` campo usando la convalida definita `ExampleModel` nel tipo.</span><span class="sxs-lookup"><span data-stu-id="597dd-109">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="597dd-110">Il modello viene creato nel `@code` blocco del componente e viene mantenuto in un campo privato (`exampleModel`).</span><span class="sxs-lookup"><span data-stu-id="597dd-110">The model is created in the component's `@code` block and held in a private field (`exampleModel`).</span></span> <span data-ttu-id="597dd-111">Il campo viene assegnato all' `Model` attributo `<EditForm>` dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="597dd-111">The field is assigned to the `Model` attribute of the `<EditForm>` element.</span></span>
* <span data-ttu-id="597dd-112">Il `DataAnnotationsValidator` componente connette il supporto di convalida usando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="597dd-112">The `DataAnnotationsValidator` component attaches validation support using data annotations.</span></span>
* <span data-ttu-id="597dd-113">Il `ValidationSummary` componente riepiloga i messaggi di convalida.</span><span class="sxs-lookup"><span data-stu-id="597dd-113">The `ValidationSummary` component summarizes validation messages.</span></span>
* <span data-ttu-id="597dd-114">`HandleValidSubmit`viene attivato quando il form viene inviato correttamente (supera la convalida).</span><span class="sxs-lookup"><span data-stu-id="597dd-114">`HandleValidSubmit` is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="597dd-115">È disponibile un set di componenti di input predefiniti per la ricezione e la convalida dell'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="597dd-115">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="597dd-116">Gli input vengono convalidati quando vengono modificati e quando viene inviato un modulo.</span><span class="sxs-lookup"><span data-stu-id="597dd-116">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="597dd-117">I componenti di input disponibili sono illustrati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="597dd-117">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="597dd-118">Componente di input</span><span class="sxs-lookup"><span data-stu-id="597dd-118">Input component</span></span> | <span data-ttu-id="597dd-119">Con rendering come&hellip;</span><span class="sxs-lookup"><span data-stu-id="597dd-119">Rendered as&hellip;</span></span>       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

<span data-ttu-id="597dd-120">Tutti i componenti di input, tra `EditForm`cui, supportano attributi arbitrari.</span><span class="sxs-lookup"><span data-stu-id="597dd-120">All of the input components, including `EditForm`, support arbitrary attributes.</span></span> <span data-ttu-id="597dd-121">Qualsiasi attributo che non corrisponde a un parametro component viene aggiunto all'elemento HTML sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="597dd-121">Any attribute that doesn't match a component parameter is added to the rendered HTML element.</span></span>

<span data-ttu-id="597dd-122">I componenti di input forniscono il comportamento predefinito per la convalida in base alla modifica e alla modifica della classe CSS in modo da riflettere lo stato del campo.</span><span class="sxs-lookup"><span data-stu-id="597dd-122">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="597dd-123">Alcuni componenti includono una logica di analisi utile.</span><span class="sxs-lookup"><span data-stu-id="597dd-123">Some components include useful parsing logic.</span></span> <span data-ttu-id="597dd-124">`InputDate` E`InputNumber` , ad esempio, gestire i valori non analizzabili con la registrazione normale come errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="597dd-124">For example, `InputDate` and `InputNumber` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="597dd-125">I tipi che possono accettare valori null supportano anche il supporto di valori null del campo di destinazione `int?`, ad esempio.</span><span class="sxs-lookup"><span data-stu-id="597dd-125">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="597dd-126">Il tipo `Starship` seguente definisce la logica di convalida usando un set di proprietà e annotazioni di dati `ExampleModel`più ampio rispetto a quello precedente:</span><span class="sxs-lookup"><span data-stu-id="597dd-126">The following `Starship` type defines validation logic using a larger set of properties and data annotations than the earlier `ExampleModel`:</span></span>

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    [Required]
    [StringLength(16, ErrorMessage = "Identifier too long (16 character limit).")]
    public string Identifier { get; set; }

    public string Description { get; set; }

    [Required]
    public string Classification { get; set; }

    [Range(1, 100000, ErrorMessage = "Accommodation invalid (1-100000).")]
    public int MaximumAccommodation { get; set; }

    [Required]
    [Range(typeof(bool), "true", "true", 
        ErrorMessage = "This form disallows unapproved ships.")]
    public bool IsValidatedDesign { get; set; }

    [Required]
    public DateTime ProductionDate { get; set; }
}
```

<span data-ttu-id="597dd-127">Nell'esempio `Description` precedente è facoltativo perché non sono presenti annotazioni di dati.</span><span class="sxs-lookup"><span data-stu-id="597dd-127">In the preceding example, `Description` is optional because no data annotations are present.</span></span>

<span data-ttu-id="597dd-128">Il form seguente convalida l'input dell'utente utilizzando la convalida definita nel `Starship` modello:</span><span class="sxs-lookup"><span data-stu-id="597dd-128">The following form validates user input using the validation defined in the `Starship` model:</span></span>

```cshtml
@page "/FormsValidation"

<h1>Starfleet Starship Database</h1>

<h2>New Ship Entry Form</h2>

<EditForm Model="@starship" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label for="identifier">Identifier: </label>
        <InputText id="identifier" @bind-Value="starship.Identifier" />
    </p>
    <p>
        <label for="description">Description (optional): </label>
        <InputTextArea id="description" @bind-Value="starship.Description" />
    </p>
    <p>
        <label for="classification">Primary Classification: </label>
        <InputSelect id="classification" @bind-Value="starship.Classification">
            <option value="">Select classification ...</option>
            <option value="Exploration">Exploration</option>
            <option value="Diplomacy">Diplomacy</option>
            <option value="Defense">Defense</option>
        </InputSelect>
    </p>
    <p>
        <label for="accommodation">Maximum Accommodation: </label>
        <InputNumber id="accommodation" 
            @bind-Value="starship.MaximumAccommodation" />
    </p>
    <p>
        <label for="valid">Engineering Approval: </label>
        <InputCheckbox id="valid" @bind-Value="starship.IsValidatedDesign" />
    </p>
    <p>
        <label for="productionDate">Production Date: </label>
        <InputDate id="productionDate" @bind-Value="starship.ProductionDate" />
    </p>

    <button type="submit">Submit</button>

    <p>
        <a href="http://www.startrek.com/">Star Trek</a>, 
        &copy;1966-2019 CBS Studios, Inc. and 
        <a href="https://www.paramount.com">Paramount Pictures</a>
    </p>
</EditForm>

@code {
    private Starship starship = new Starship();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

<span data-ttu-id="597dd-129">`EditForm` crea un oggetto `EditContext` come [valore di propagazione](xref:blazor/components#cascading-values-and-parameters) che tiene traccia dei metadati relativi al processo di modifica, inclusi i campi modificati e i messaggi di convalida correnti.</span><span class="sxs-lookup"><span data-stu-id="597dd-129">The `EditForm` creates an `EditContext` as a [cascading value](xref:blazor/components#cascading-values-and-parameters) that tracks metadata about the edit process, including which fields have been modified and the current validation messages.</span></span> <span data-ttu-id="597dd-130">Fornisce inoltre eventi pratici per invii validi e non validi (`OnValidSubmit`, `OnInvalidSubmit`). `EditForm`</span><span class="sxs-lookup"><span data-stu-id="597dd-130">The `EditForm` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="597dd-131">In alternativa, usare `OnSubmit` per attivare la convalida e verificare i valori dei campi con il codice di convalida personalizzato.</span><span class="sxs-lookup"><span data-stu-id="597dd-131">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

## <a name="inputtext-based-on-the-input-event"></a><span data-ttu-id="597dd-132">InputText basato sull'evento di input</span><span class="sxs-lookup"><span data-stu-id="597dd-132">InputText based on the input event</span></span>

<span data-ttu-id="597dd-133">Utilizzare il `InputText` componente per creare un componente personalizzato che utilizza l' `input` evento anziché l' `change` evento.</span><span class="sxs-lookup"><span data-stu-id="597dd-133">Use the `InputText` component to create a custom component that uses the `input` event instead of the `change` event.</span></span>

<span data-ttu-id="597dd-134">Creare un componente con il markup seguente e usare il componente Analogamente a `InputText` quanto viene usato:</span><span class="sxs-lookup"><span data-stu-id="597dd-134">Create a component with the following markup, and use the component just as `InputText` is used:</span></span>

```cshtml
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="validation-support"></a><span data-ttu-id="597dd-135">Supporto della convalida</span><span class="sxs-lookup"><span data-stu-id="597dd-135">Validation support</span></span>

<span data-ttu-id="597dd-136">Il `DataAnnotationsValidator` componente connette il supporto di convalida usando le annotazioni dei dati `EditContext`a a cascata.</span><span class="sxs-lookup"><span data-stu-id="597dd-136">The `DataAnnotationsValidator` component attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="597dd-137">L'abilitazione del supporto per la convalida usando le annotazioni dei dati richiede questo movimento esplicito.</span><span class="sxs-lookup"><span data-stu-id="597dd-137">Enabling support for validation using data annotations requires this explicit gesture.</span></span> <span data-ttu-id="597dd-138">Per usare un sistema di convalida diverso rispetto alle annotazioni dei `DataAnnotationsValidator` dati, sostituire con un'implementazione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="597dd-138">To use a different validation system than data annotations, replace the `DataAnnotationsValidator` with a custom implementation.</span></span> <span data-ttu-id="597dd-139">L'implementazione del ASP.NET Core è disponibile per l'ispezione nell'origine riferimento: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="597dd-139">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span></span>

<span data-ttu-id="597dd-140">Il `ValidationSummary` componente riepiloga tutti i messaggi di convalida, che è simile all' [Helper tag di riepilogo della convalida](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="597dd-140">The `ValidationSummary` component summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span></span>

<span data-ttu-id="597dd-141">Il `ValidationMessage` componente Visualizza i messaggi di convalida per un campo specifico, che è simile all' [Helper tag del messaggio di convalida](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="597dd-141">The `ValidationMessage` component displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="597dd-142">Specificare il campo per la convalida con `For` l'attributo e un'espressione lambda che denomina la proprietà del modello:</span><span class="sxs-lookup"><span data-stu-id="597dd-142">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

<span data-ttu-id="597dd-143">I `ValidationMessage` componenti `ValidationSummary` e supportano attributi arbitrari.</span><span class="sxs-lookup"><span data-stu-id="597dd-143">The `ValidationMessage` and `ValidationSummary` components support arbitrary attributes.</span></span> <span data-ttu-id="597dd-144">Qualsiasi attributo che non corrisponde a un parametro component viene aggiunto all'elemento `<div>` generato `<ul>` o.</span><span class="sxs-lookup"><span data-stu-id="597dd-144">Any attribute that doesn't match a component parameter is added to the generated `<div>` or `<ul>` element.</span></span>

### <a name="validation-of-complex-or-collection-type-properties"></a><span data-ttu-id="597dd-145">Convalida di proprietà di tipo complesso o di raccolta</span><span class="sxs-lookup"><span data-stu-id="597dd-145">Validation of complex or collection type properties</span></span>

<span data-ttu-id="597dd-146">Gli attributi di convalida applicati alle proprietà di un modello vengono convalidati quando viene inviato il modulo.</span><span class="sxs-lookup"><span data-stu-id="597dd-146">Validation attributes applied to the properties of a model validate when the form is submitted.</span></span> <span data-ttu-id="597dd-147">Tuttavia, le proprietà delle raccolte o dei tipi di dati complessi di un modello non vengono convalidate durante l'invio del modulo.</span><span class="sxs-lookup"><span data-stu-id="597dd-147">However, the properties of collections or complex data types of a model aren't validated on form submission.</span></span> <span data-ttu-id="597dd-148">Per rispettare gli attributi di convalida annidati in questo scenario, usare un componente di convalida personalizzato.</span><span class="sxs-lookup"><span data-stu-id="597dd-148">To honor the nested validation attributes in this scenario, use a custom validation component.</span></span> <span data-ttu-id="597dd-149">Per un esempio, vedere l' [esempio di convalida di Blazor nel repository GitHub ASPNET/Samples](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).</span><span class="sxs-lookup"><span data-stu-id="597dd-149">For an example, see the [Blazor Validation sample in the aspnet/samples GitHub repository](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).</span></span>
