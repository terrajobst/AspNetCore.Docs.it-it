---
title: ASP.NET Core Blazor moduli e convalida
author: guardrex
description: Informazioni su come usare i moduli e gli scenari di convalida di campo in Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/forms-validation
ms.openlocfilehash: 6f6fdc13dbb754ecfe06025d496017d3c16951fe
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/17/2020
ms.locfileid: "76159963"
---
# <a name="aspnet-core-opno-locblazor-forms-and-validation"></a><span data-ttu-id="abac3-103">ASP.NET Core Blazor moduli e convalida</span><span class="sxs-lookup"><span data-stu-id="abac3-103">ASP.NET Core Blazor forms and validation</span></span>

<span data-ttu-id="abac3-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="abac3-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="abac3-105">I moduli e la convalida sono supportati in Blazor usando le [annotazioni dei dati](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="abac3-105">Forms and validation are supported in Blazor using [data annotations](xref:mvc/models/validation).</span></span>

<span data-ttu-id="abac3-106">Il tipo di `ExampleModel` seguente definisce la logica di convalida usando le annotazioni dei dati:</span><span class="sxs-lookup"><span data-stu-id="abac3-106">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="abac3-107">Un modulo viene definito utilizzando il componente `EditForm`.</span><span class="sxs-lookup"><span data-stu-id="abac3-107">A form is defined using the `EditForm` component.</span></span> <span data-ttu-id="abac3-108">Il modulo seguente illustra elementi tipici, componenti e codice Razor:</span><span class="sxs-lookup"><span data-stu-id="abac3-108">The following form demonstrates typical elements, components, and Razor code:</span></span>

```razor
<EditForm Model="@exampleModel" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" @bind-Value="exampleModel.Name" />

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

<span data-ttu-id="abac3-109">Nell'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="abac3-109">In the preceding example:</span></span>

* <span data-ttu-id="abac3-110">Il modulo convalida l'input dell'utente nel campo `name` usando la convalida definita nel tipo di `ExampleModel`.</span><span class="sxs-lookup"><span data-stu-id="abac3-110">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="abac3-111">Il modello viene creato nel blocco `@code` del componente e viene mantenuto in un campo privato (`exampleModel`).</span><span class="sxs-lookup"><span data-stu-id="abac3-111">The model is created in the component's `@code` block and held in a private field (`exampleModel`).</span></span> <span data-ttu-id="abac3-112">Il campo viene assegnato all'attributo `Model` dell'elemento `<EditForm>`.</span><span class="sxs-lookup"><span data-stu-id="abac3-112">The field is assigned to the `Model` attribute of the `<EditForm>` element.</span></span>
* <span data-ttu-id="abac3-113">Il `@bind-Value` del componente `InputText` viene associato:</span><span class="sxs-lookup"><span data-stu-id="abac3-113">The `InputText` component's `@bind-Value` binds:</span></span>
  * <span data-ttu-id="abac3-114">Proprietà del modello (`exampleModel.Name`) alla proprietà `Value` del componente di `InputText`.</span><span class="sxs-lookup"><span data-stu-id="abac3-114">The model property (`exampleModel.Name`) to the `InputText` component's `Value` property.</span></span>
  * <span data-ttu-id="abac3-115">Delegato dell'evento di modifica alla proprietà `ValueChanged` del componente `InputText`.</span><span class="sxs-lookup"><span data-stu-id="abac3-115">A change event delegate to the `InputText` component's `ValueChanged` property.</span></span>
* <span data-ttu-id="abac3-116">Il componente `DataAnnotationsValidator` connette il supporto della convalida usando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="abac3-116">The `DataAnnotationsValidator` component attaches validation support using data annotations.</span></span>
* <span data-ttu-id="abac3-117">Il componente `ValidationSummary` riepiloga i messaggi di convalida.</span><span class="sxs-lookup"><span data-stu-id="abac3-117">The `ValidationSummary` component summarizes validation messages.</span></span>
* <span data-ttu-id="abac3-118">`HandleValidSubmit` viene attivato quando il modulo viene inviato correttamente (supera la convalida).</span><span class="sxs-lookup"><span data-stu-id="abac3-118">`HandleValidSubmit` is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="abac3-119">È disponibile un set di componenti di input predefiniti per la ricezione e la convalida dell'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="abac3-119">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="abac3-120">Gli input vengono convalidati quando vengono modificati e quando viene inviato un modulo.</span><span class="sxs-lookup"><span data-stu-id="abac3-120">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="abac3-121">I componenti di input disponibili sono illustrati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="abac3-121">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="abac3-122">Componente di input</span><span class="sxs-lookup"><span data-stu-id="abac3-122">Input component</span></span> | <span data-ttu-id="abac3-123">Rendering eseguito come&hellip;</span><span class="sxs-lookup"><span data-stu-id="abac3-123">Rendered as&hellip;</span></span>       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

<span data-ttu-id="abac3-124">Tutti i componenti di input, tra cui `EditForm`, supportano attributi arbitrari.</span><span class="sxs-lookup"><span data-stu-id="abac3-124">All of the input components, including `EditForm`, support arbitrary attributes.</span></span> <span data-ttu-id="abac3-125">Qualsiasi attributo che non corrisponde a un parametro component viene aggiunto all'elemento HTML sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="abac3-125">Any attribute that doesn't match a component parameter is added to the rendered HTML element.</span></span>

<span data-ttu-id="abac3-126">I componenti di input forniscono il comportamento predefinito per la convalida in base alla modifica e alla modifica della classe CSS in modo da riflettere lo stato del campo.</span><span class="sxs-lookup"><span data-stu-id="abac3-126">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="abac3-127">Alcuni componenti includono una logica di analisi utile.</span><span class="sxs-lookup"><span data-stu-id="abac3-127">Some components include useful parsing logic.</span></span> <span data-ttu-id="abac3-128">Ad esempio, `InputDate` e `InputNumber` gestiscono correttamente i valori non analizzabili registrando tali valori come errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="abac3-128">For example, `InputDate` and `InputNumber` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="abac3-129">I tipi che possono accettare valori null supportano anche il supporto di valori null del campo di destinazione, ad esempio `int?`.</span><span class="sxs-lookup"><span data-stu-id="abac3-129">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="abac3-130">Il tipo di `Starship` seguente definisce la logica di convalida usando un set di proprietà e annotazioni di dati più ampio rispetto al `ExampleModel`precedente:</span><span class="sxs-lookup"><span data-stu-id="abac3-130">The following `Starship` type defines validation logic using a larger set of properties and data annotations than the earlier `ExampleModel`:</span></span>

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

<span data-ttu-id="abac3-131">Nell'esempio precedente `Description` è facoltativo perché non sono presenti annotazioni di dati.</span><span class="sxs-lookup"><span data-stu-id="abac3-131">In the preceding example, `Description` is optional because no data annotations are present.</span></span>

<span data-ttu-id="abac3-132">Il form seguente convalida l'input dell'utente usando la convalida definita nel modello di `Starship`:</span><span class="sxs-lookup"><span data-stu-id="abac3-132">The following form validates user input using the validation defined in the `Starship` model:</span></span>

```razor
@page "/FormsValidation"

<h1>Starfleet Starship Database</h1>

<h2>New Ship Entry Form</h2>

<EditForm Model="@starship" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label>
            Identifier:
            <InputText @bind-Value="starship.Identifier" />
        </label>
    </p>
    <p>
        <label>
            Description (optional):
            <InputTextArea @bind-Value="starship.Description" />
        </label>
    </p>
    <p>
        <label>
            Primary Classification:
            <InputSelect @bind-Value="starship.Classification">
                <option value="">Select classification ...</option>
                <option value="Exploration">Exploration</option>
                <option value="Diplomacy">Diplomacy</option>
                <option value="Defense">Defense</option>
            </InputSelect>
        </label>
    </p>
    <p>
        <label>
            Maximum Accommodation:
            <InputNumber @bind-Value="starship.MaximumAccommodation" />
        </label>
    </p>
    <p>
        <label>
            Engineering Approval:
            <InputCheckbox @bind-Value="starship.IsValidatedDesign" />
        </label>
    </p>
    <p>
        <label>
            Production Date:
            <InputDate @bind-Value="starship.ProductionDate" />
        </label>
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

<span data-ttu-id="abac3-133">`EditForm` crea un oggetto `EditContext` come [valore di propagazione](xref:blazor/components#cascading-values-and-parameters) che tiene traccia dei metadati relativi al processo di modifica, inclusi i campi modificati e i messaggi di convalida correnti.</span><span class="sxs-lookup"><span data-stu-id="abac3-133">The `EditForm` creates an `EditContext` as a [cascading value](xref:blazor/components#cascading-values-and-parameters) that tracks metadata about the edit process, including which fields have been modified and the current validation messages.</span></span> <span data-ttu-id="abac3-134">Il `EditForm` fornisce anche gli eventi pratici per le invii validi e non validi (`OnValidSubmit`, `OnInvalidSubmit`).</span><span class="sxs-lookup"><span data-stu-id="abac3-134">The `EditForm` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="abac3-135">In alternativa, usare `OnSubmit` per attivare la convalida e verificare i valori dei campi con il codice di convalida personalizzato.</span><span class="sxs-lookup"><span data-stu-id="abac3-135">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

<span data-ttu-id="abac3-136">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="abac3-136">In the following example:</span></span>

* <span data-ttu-id="abac3-137">Il metodo `HandleSubmit` viene eseguito quando viene selezionato il pulsante **Invia** .</span><span class="sxs-lookup"><span data-stu-id="abac3-137">The `HandleSubmit` method runs when the **Submit** button is selected.</span></span>
* <span data-ttu-id="abac3-138">Il modulo viene convalidato utilizzando la `EditContext`del modulo.</span><span class="sxs-lookup"><span data-stu-id="abac3-138">The form is validated using the form's `EditContext`.</span></span>
* <span data-ttu-id="abac3-139">Il modulo viene ulteriormente convalidato passando il `EditContext` al metodo `ServerValidate` che chiama un endpoint dell'API Web nel server (*non illustrato*).</span><span class="sxs-lookup"><span data-stu-id="abac3-139">The form is further validated by passing the `EditContext` to the `ServerValidate` method that calls a web API endpoint on the server (*not shown*).</span></span>
* <span data-ttu-id="abac3-140">Il codice aggiuntivo viene eseguito a seconda del risultato della convalida lato client e lato server controllando `isValid`.</span><span class="sxs-lookup"><span data-stu-id="abac3-140">Additional code is run depending on the result of the client- and server-side validation by checking `isValid`.</span></span>

```razor
<EditForm EditContext="@editContext" OnSubmit="@HandleSubmit">

    ...

    <button type="submit">Submit</button>
</EditForm>

@code {
    private Starship starship = new Starship();
    private EditContext editContext;

    protected override void OnInitialized()
    {
        editContext = new EditContext(starship);
    }

    private async Task HandleSubmit()
    {
        var isValid = editContext.Validate() && 
            await ServerValidate(editContext);

        if (isValid)
        {
            ...
        }
        else
        {
            ...
        }
    }

    private async Task<bool> ServerValidate(EditContext editContext)
    {
        var serverChecksValid = ...

        return serverChecksValid;
    }
}
```

## <a name="inputtext-based-on-the-input-event"></a><span data-ttu-id="abac3-141">InputText basato sull'evento di input</span><span class="sxs-lookup"><span data-stu-id="abac3-141">InputText based on the input event</span></span>

<span data-ttu-id="abac3-142">Utilizzare il componente `InputText` per creare un componente personalizzato che utilizza l'evento `input` anziché l'evento `change`.</span><span class="sxs-lookup"><span data-stu-id="abac3-142">Use the `InputText` component to create a custom component that uses the `input` event instead of the `change` event.</span></span>

<span data-ttu-id="abac3-143">Creare un componente con il markup seguente e usare il componente proprio come `InputText` viene usato:</span><span class="sxs-lookup"><span data-stu-id="abac3-143">Create a component with the following markup, and use the component just as `InputText` is used:</span></span>

```razor
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="work-with-radio-buttons"></a><span data-ttu-id="abac3-144">Usare i pulsanti di opzione</span><span class="sxs-lookup"><span data-stu-id="abac3-144">Work with radio buttons</span></span>

<span data-ttu-id="abac3-145">Quando si utilizzano pulsanti di opzione in un modulo, data binding viene gestito in modo diverso rispetto ad altri elementi perché i pulsanti di opzione vengono valutati come un gruppo.</span><span class="sxs-lookup"><span data-stu-id="abac3-145">When working with radio buttons in a form, data binding is handled differently than other elements because radio buttons are evaluated as a group.</span></span> <span data-ttu-id="abac3-146">Il valore di ogni pulsante di opzione è fisso, ma il valore del gruppo di pulsanti di opzione è il valore del pulsante di opzione selezionato.</span><span class="sxs-lookup"><span data-stu-id="abac3-146">The value of each radio button is fixed, but the value of the radio button group is the value of the selected radio button.</span></span> <span data-ttu-id="abac3-147">L'esempio seguente illustra come:</span><span class="sxs-lookup"><span data-stu-id="abac3-147">The following example shows how to:</span></span>

* <span data-ttu-id="abac3-148">Gestire data binding per un gruppo di pulsanti di opzione.</span><span class="sxs-lookup"><span data-stu-id="abac3-148">Handle data binding for a radio button group.</span></span>
* <span data-ttu-id="abac3-149">Supporta la convalida usando un componente `InputRadio` personalizzato.</span><span class="sxs-lookup"><span data-stu-id="abac3-149">Support validation using a custom `InputRadio` component.</span></span>

```razor
@using System.Globalization
@typeparam TValue
@inherits InputBase<TValue>

<input @attributes="AdditionalAttributes" type="radio" value="@SelectedValue" 
       checked="@(SelectedValue.Equals(Value))" @onchange="OnChange" />

@code {
    [Parameter]
    public TValue SelectedValue { get; set; }

    private void OnChange(ChangeEventArgs args)
    {
        CurrentValueAsString = args.Value.ToString();
    }

    protected override bool TryParseValueFromString(string value, 
        out TValue result, out string errorMessage)
    {
        var success = BindConverter.TryConvertTo<TValue>(
            value, CultureInfo.CurrentCulture, out var parsedValue);
        if (success)
        {
            result = parsedValue;
            errorMessage = null;

            return true;
        }
        else
        {
            result = default;
            errorMessage = $"{FieldIdentifier.FieldName} field isn't valid.";

            return false;
        }
    }
}
```

<span data-ttu-id="abac3-150">Il `EditForm` seguente usa il componente `InputRadio` precedente per ottenere e convalidare una classificazione dall'utente:</span><span class="sxs-lookup"><span data-stu-id="abac3-150">The following `EditForm` uses the preceding `InputRadio` component to obtain and validate a rating from the user:</span></span>

```razor
@page "/RadioButtonExample"
@using System.ComponentModel.DataAnnotations

<h1>Radio Button Group Test</h1>

<EditForm Model="model" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    @for (int i = 1; i <= 5; i++)
    {
        <label>
            <InputRadio name="rate" SelectedValue="i" @bind-Value="model.Rating" />
            @i
        </label>
    }

    <button type="submit">Submit</button>
</EditForm>

<p>You chose: @model.Rating</p>

@code {
    private Model model = new Model();

    private void HandleValidSubmit()
    {
        Console.WriteLine("valid");
    }

    public class Model
    {
        [Range(1, 5)]
        public int Rating { get; set; }
    }
}
```

## <a name="validation-support"></a><span data-ttu-id="abac3-151">Supporto della convalida</span><span class="sxs-lookup"><span data-stu-id="abac3-151">Validation support</span></span>

<span data-ttu-id="abac3-152">Il componente `DataAnnotationsValidator` connette il supporto di convalida usando le annotazioni dei dati per la `EditContext`a cascata.</span><span class="sxs-lookup"><span data-stu-id="abac3-152">The `DataAnnotationsValidator` component attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="abac3-153">L'abilitazione del supporto per la convalida usando le annotazioni dei dati richiede questo movimento esplicito.</span><span class="sxs-lookup"><span data-stu-id="abac3-153">Enabling support for validation using data annotations requires this explicit gesture.</span></span> <span data-ttu-id="abac3-154">Per utilizzare un sistema di convalida diverso rispetto alle annotazioni dei dati, sostituire il `DataAnnotationsValidator` con un'implementazione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="abac3-154">To use a different validation system than data annotations, replace the `DataAnnotationsValidator` with a custom implementation.</span></span> <span data-ttu-id="abac3-155">L'implementazione del ASP.NET Core è disponibile per l'ispezione nell'origine riferimento: [DataAnnotationsValidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="abac3-155">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span></span>

Blazor<span data-ttu-id="abac3-156"> esegue due tipi di convalida:</span><span class="sxs-lookup"><span data-stu-id="abac3-156"> performs two types of validation:</span></span>

* <span data-ttu-id="abac3-157">La *convalida dei campi* viene eseguita quando l'utente esce da un campo.</span><span class="sxs-lookup"><span data-stu-id="abac3-157">*Field validation* is performed when the user tabs out of a field.</span></span> <span data-ttu-id="abac3-158">Durante la convalida dei campi, il componente `DataAnnotationsValidator` associa tutti i risultati della convalida segnalati al campo.</span><span class="sxs-lookup"><span data-stu-id="abac3-158">During field validation, the `DataAnnotationsValidator` component associates all reported validation results with the field.</span></span>
* <span data-ttu-id="abac3-159">La *convalida del modello* viene eseguita quando l'utente invia il modulo.</span><span class="sxs-lookup"><span data-stu-id="abac3-159">*Model validation* is performed when the user submits the form.</span></span> <span data-ttu-id="abac3-160">Durante la convalida del modello, il componente `DataAnnotationsValidator` tenta di determinare il campo in base al nome del membro segnalato dal risultato della convalida.</span><span class="sxs-lookup"><span data-stu-id="abac3-160">During model validation, the `DataAnnotationsValidator` component attempts to determine the field based on the member name that the validation result reports.</span></span> <span data-ttu-id="abac3-161">I risultati della convalida che non sono associati a un singolo membro sono associati al modello anziché a un campo.</span><span class="sxs-lookup"><span data-stu-id="abac3-161">Validation results that aren't associated with an individual member are associated with the model rather than a field.</span></span>

### <a name="validation-summary-and-validation-message-components"></a><span data-ttu-id="abac3-162">Componenti del messaggio di convalida e di riepilogo della convalida</span><span class="sxs-lookup"><span data-stu-id="abac3-162">Validation Summary and Validation Message components</span></span>

<span data-ttu-id="abac3-163">Il componente `ValidationSummary` riepiloga tutti i messaggi di convalida, che è simile all' [Helper tag del riepilogo di convalida](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="abac3-163">The `ValidationSummary` component summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper):</span></span>

```razor
<ValidationSummary />
```

<span data-ttu-id="abac3-164">Messaggi di convalida dell'output per un modello specifico con il parametro `Model`:</span><span class="sxs-lookup"><span data-stu-id="abac3-164">Output validation messages for a specific model with the `Model` parameter:</span></span>
  
```razor
<ValidationSummary Model="@starship" />
```

<span data-ttu-id="abac3-165">Il componente `ValidationMessage` Visualizza i messaggi di convalida per un campo specifico, che è simile all' [Helper tag del messaggio di convalida](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="abac3-165">The `ValidationMessage` component displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="abac3-166">Specificare il campo per la convalida con l'attributo `For` e un'espressione lambda che denomina la proprietà Model:</span><span class="sxs-lookup"><span data-stu-id="abac3-166">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```razor
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

<span data-ttu-id="abac3-167">I componenti `ValidationMessage` e `ValidationSummary` supportano attributi arbitrari.</span><span class="sxs-lookup"><span data-stu-id="abac3-167">The `ValidationMessage` and `ValidationSummary` components support arbitrary attributes.</span></span> <span data-ttu-id="abac3-168">Qualsiasi attributo che non corrisponde a un parametro component viene aggiunto all'elemento `<div>` o `<ul>` generato.</span><span class="sxs-lookup"><span data-stu-id="abac3-168">Any attribute that doesn't match a component parameter is added to the generated `<div>` or `<ul>` element.</span></span>

### <a name="custom-validation-attributes"></a><span data-ttu-id="abac3-169">Attributi di convalida personalizzati</span><span class="sxs-lookup"><span data-stu-id="abac3-169">Custom validation attributes</span></span>

<span data-ttu-id="abac3-170">Per assicurarsi che un risultato di convalida venga associato correttamente a un campo quando si usa un [attributo di convalida personalizzato](xref:mvc/models/validation#custom-attributes), passare la <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> del contesto di convalida durante la creazione del <xref:System.ComponentModel.DataAnnotations.ValidationResult>:</span><span class="sxs-lookup"><span data-stu-id="abac3-170">To ensure that a validation result is correctly associated with a field when using a [custom validation attribute](xref:mvc/models/validation#custom-attributes), pass the validation context's <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> when creating the <xref:System.ComponentModel.DataAnnotations.ValidationResult>:</span></span>

```csharp
using System;
using System.ComponentModel.DataAnnotations;

private class MyCustomValidator : ValidationAttribute
{
    protected override ValidationResult IsValid(object value, 
        ValidationContext validationContext)
    {
        ...

        return new ValidationResult("Validation message to user.",
            new[] { validationContext.MemberName });
    }
}
```

### <a name="opno-locblazor-data-annotations-validation-package"></a><span data-ttu-id="abac3-171">pacchetto di convalida delle annotazioni dei dati Blazor</span><span class="sxs-lookup"><span data-stu-id="abac3-171">Blazor data annotations validation package</span></span>

<span data-ttu-id="abac3-172">[Microsoft. AspNetCore.Blazor. DataAnnotations. Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) è un pacchetto che colma i gap dell'esperienza di convalida usando il componente `DataAnnotationsValidator`.</span><span class="sxs-lookup"><span data-stu-id="abac3-172">The [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) is a package that fills validation experience gaps using the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="abac3-173">Il pacchetto è attualmente *sperimentale*.</span><span class="sxs-lookup"><span data-stu-id="abac3-173">The package is currently *experimental*.</span></span>

### <a name="compareproperty-attribute"></a><span data-ttu-id="abac3-174">Attributo [CompareProperty]</span><span class="sxs-lookup"><span data-stu-id="abac3-174">[CompareProperty] attribute</span></span>

<span data-ttu-id="abac3-175">Il <xref:System.ComponentModel.DataAnnotations.CompareAttribute> non funziona correttamente con il componente `DataAnnotationsValidator` perché non associa il risultato della convalida a un membro specifico.</span><span class="sxs-lookup"><span data-stu-id="abac3-175">The <xref:System.ComponentModel.DataAnnotations.CompareAttribute> doesn't work well with the `DataAnnotationsValidator` component because it doesn't associate the validation result with a specific member.</span></span> <span data-ttu-id="abac3-176">Ciò può comportare un comportamento incoerente tra la convalida a livello di campo e quando l'intero modello viene convalidato in un invio.</span><span class="sxs-lookup"><span data-stu-id="abac3-176">This can result in inconsistent behavior between field-level validation and when the entire model is validated on a submit.</span></span> <span data-ttu-id="abac3-177">[Microsoft. AspNetCore.Blazor. DataAnnotations.](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) il pacchetto *sperimentale* di convalida introduce un attributo di convalida aggiuntivo, `ComparePropertyAttribute`, che aggira queste limitazioni.</span><span class="sxs-lookup"><span data-stu-id="abac3-177">The [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) *experimental* package introduces an additional validation attribute, `ComparePropertyAttribute`, that works around these limitations.</span></span> <span data-ttu-id="abac3-178">In un'app Blazor `[CompareProperty]` è una sostituzione diretta per l'attributo `[Compare]`.</span><span class="sxs-lookup"><span data-stu-id="abac3-178">In a Blazor app, `[CompareProperty]` is a direct replacement for the `[Compare]` attribute.</span></span>

### <a name="nested-models-collection-types-and-complex-types"></a><span data-ttu-id="abac3-179">Modelli annidati, tipi di raccolta e tipi complessi</span><span class="sxs-lookup"><span data-stu-id="abac3-179">Nested models, collection types, and complex types</span></span>

Blazor<span data-ttu-id="abac3-180"> fornisce il supporto per la convalida dell'input del form usando le annotazioni dei dati con la `DataAnnotationsValidator`predefinita.</span><span class="sxs-lookup"><span data-stu-id="abac3-180"> provides support for validating form input using data annotations with the built-in `DataAnnotationsValidator`.</span></span> <span data-ttu-id="abac3-181">Tuttavia, il `DataAnnotationsValidator` convalida solo le proprietà di primo livello del modello associato al form che non sono proprietà di tipo raccolta o complesso.</span><span class="sxs-lookup"><span data-stu-id="abac3-181">However, the `DataAnnotationsValidator` only validates top-level properties of the model bound to the form that aren't collection- or complex-type properties.</span></span>

<span data-ttu-id="abac3-182">Per convalidare l'intero grafico di oggetti del modello associato, incluse le proprietà della raccolta e del tipo complesso, utilizzare la `ObjectGraphDataAnnotationsValidator` fornita dall'oggetto *sperimentale* [Microsoft. AspNetCore.Blazor. DataAnnotations. Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) (pacchetto):</span><span class="sxs-lookup"><span data-stu-id="abac3-182">To validate the bound model's entire object graph, including collection- and complex-type properties, use the `ObjectGraphDataAnnotationsValidator` provided by the *experimental* [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) package:</span></span>

```razor
<EditForm Model="@model" OnValidSubmit="HandleValidSubmit">
    <ObjectGraphDataAnnotationsValidator />
    ...
</EditForm>
```

<span data-ttu-id="abac3-183">Annotare le proprietà del modello con `[ValidateComplexType]`.</span><span class="sxs-lookup"><span data-stu-id="abac3-183">Annotate model properties with `[ValidateComplexType]`.</span></span> <span data-ttu-id="abac3-184">Nelle classi del modello seguenti, la classe `ShipDescription` contiene annotazioni di dati aggiuntive da convalidare quando il modello è associato al form:</span><span class="sxs-lookup"><span data-stu-id="abac3-184">In the following model classes, the `ShipDescription` class contains additional data annotations to validate when the model is bound to the form:</span></span>

<span data-ttu-id="abac3-185">*Starship.cs*:</span><span class="sxs-lookup"><span data-stu-id="abac3-185">*Starship.cs*:</span></span>

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    ...

    [ValidateComplexType]
    public ShipDescription ShipDescription { get; set; }

    ...
}
```

<span data-ttu-id="abac3-186">*ShipDescription.cs*:</span><span class="sxs-lookup"><span data-stu-id="abac3-186">*ShipDescription.cs*:</span></span>

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class ShipDescription
{
    [Required]
    [StringLength(40, ErrorMessage = "Description too long (40 char).")]
    public string ShortDescription { get; set; }
    
    [Required]
    [StringLength(240, ErrorMessage = "Description too long (240 char).")]
    public string LongDescription { get; set; }
}
```

### <a name="enable-the-submit-button-based-on-form-validation"></a><span data-ttu-id="abac3-187">Abilita il pulsante Invia in base alla convalida del modulo</span><span class="sxs-lookup"><span data-stu-id="abac3-187">Enable the submit button based on form validation</span></span>

<span data-ttu-id="abac3-188">Per abilitare e disabilitare il pulsante Invia in base alla convalida del modulo:</span><span class="sxs-lookup"><span data-stu-id="abac3-188">To enable and disable the submit button based on form validation:</span></span>

* <span data-ttu-id="abac3-189">Utilizzare la `EditContext` del modulo per assegnare il modello quando il componente viene inizializzato.</span><span class="sxs-lookup"><span data-stu-id="abac3-189">Use the form's `EditContext` to assign the model when the component is initialized.</span></span>
* <span data-ttu-id="abac3-190">Convalidare il form nel callback `OnFieldChanged` del contesto per abilitare e disabilitare il pulsante Submit.</span><span class="sxs-lookup"><span data-stu-id="abac3-190">Validate the form in the context's `OnFieldChanged` callback to enable and disable the submit button.</span></span>

```razor
<EditForm EditContext="@editContext">
    <DataAnnotationsValidator />
    <ValidationSummary />

    ...

    <button type="submit" disabled="@formInvalid">Submit</button>
</EditForm>

@code {
    private Starship starship = new Starship();
    private bool formInvalid = true;
    private EditContext editContext;

    protected override void OnInitialized()
    {
        editContext = new EditContext(starship);

        editContext.OnFieldChanged += (_, __) =>
        {
            formInvalid = !editContext.Validate();
            StateHasChanged();
        };
    }
}
```

<span data-ttu-id="abac3-191">Nell'esempio precedente impostare `formInvalid` su `false` se:</span><span class="sxs-lookup"><span data-stu-id="abac3-191">In the preceding example, set `formInvalid` to `false` if:</span></span>

* <span data-ttu-id="abac3-192">Il modulo viene precaricato con valori predefiniti validi.</span><span class="sxs-lookup"><span data-stu-id="abac3-192">The form is preloaded with valid default values.</span></span>
* <span data-ttu-id="abac3-193">Si desidera attivare il pulsante Invia quando il modulo viene caricato.</span><span class="sxs-lookup"><span data-stu-id="abac3-193">You want the submit button enabled when the form loads.</span></span>

<span data-ttu-id="abac3-194">Un effetto collaterale dell'approccio precedente è che un componente `ValidationSummary` viene popolato con campi non validi dopo che l'utente interagisce con un qualsiasi campo.</span><span class="sxs-lookup"><span data-stu-id="abac3-194">A side effect of the preceding approach is that a `ValidationSummary` component is populated with invalid fields after the user interacts with any one field.</span></span> <span data-ttu-id="abac3-195">Questo scenario può essere risolto in uno dei modi seguenti:</span><span class="sxs-lookup"><span data-stu-id="abac3-195">This scenario can be addressed in either of the following ways:</span></span>

* <span data-ttu-id="abac3-196">Non usare un componente `ValidationSummary` nel form.</span><span class="sxs-lookup"><span data-stu-id="abac3-196">Don't use a `ValidationSummary` component on the form.</span></span>
* <span data-ttu-id="abac3-197">Rendere visibile il componente `ValidationSummary` quando viene selezionato il pulsante Invia, ad esempio in un metodo di `HandleValidSubmit`.</span><span class="sxs-lookup"><span data-stu-id="abac3-197">Make the `ValidationSummary` component visible when the submit button is selected (for example, in a `HandleValidSubmit` method).</span></span>

```razor
<EditForm EditContext="@editContext" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary style="@displaySummary" />

    ...

    <button type="submit" disabled="@formInvalid">Submit</button>
</EditForm>

@code {
    private string displaySummary = "display:none";

    ...

    private void HandleValidSubmit()
    {
        displaySummary = "display:block";
    }
}
```
