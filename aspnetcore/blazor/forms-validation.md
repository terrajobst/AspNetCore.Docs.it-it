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
# <a name="aspnet-core-opno-locblazor-forms-and-validation"></a>ASP.NET Core Blazor moduli e convalida

Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)

I moduli e la convalida sono supportati in Blazor usando le [annotazioni dei dati](xref:mvc/models/validation).

Il tipo di `ExampleModel` seguente definisce la logica di convalida usando le annotazioni dei dati:

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

Un modulo viene definito utilizzando il componente `EditForm`. Il modulo seguente illustra elementi tipici, componenti e codice Razor:

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

Nell'esempio precedente:

* Il modulo convalida l'input dell'utente nel campo `name` usando la convalida definita nel tipo di `ExampleModel`. Il modello viene creato nel blocco `@code` del componente e viene mantenuto in un campo privato (`exampleModel`). Il campo viene assegnato all'attributo `Model` dell'elemento `<EditForm>`.
* Il `@bind-Value` del componente `InputText` viene associato:
  * Proprietà del modello (`exampleModel.Name`) alla proprietà `Value` del componente di `InputText`.
  * Delegato dell'evento di modifica alla proprietà `ValueChanged` del componente `InputText`.
* Il componente `DataAnnotationsValidator` connette il supporto della convalida usando le annotazioni dei dati.
* Il componente `ValidationSummary` riepiloga i messaggi di convalida.
* `HandleValidSubmit` viene attivato quando il modulo viene inviato correttamente (supera la convalida).

È disponibile un set di componenti di input predefiniti per la ricezione e la convalida dell'input dell'utente. Gli input vengono convalidati quando vengono modificati e quando viene inviato un modulo. I componenti di input disponibili sono illustrati nella tabella seguente.

| Componente di input | Rendering eseguito come&hellip;       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

Tutti i componenti di input, tra cui `EditForm`, supportano attributi arbitrari. Qualsiasi attributo che non corrisponde a un parametro component viene aggiunto all'elemento HTML sottoposto a rendering.

I componenti di input forniscono il comportamento predefinito per la convalida in base alla modifica e alla modifica della classe CSS in modo da riflettere lo stato del campo. Alcuni componenti includono una logica di analisi utile. Ad esempio, `InputDate` e `InputNumber` gestiscono correttamente i valori non analizzabili registrando tali valori come errori di convalida. I tipi che possono accettare valori null supportano anche il supporto di valori null del campo di destinazione, ad esempio `int?`.

Il tipo di `Starship` seguente definisce la logica di convalida usando un set di proprietà e annotazioni di dati più ampio rispetto al `ExampleModel`precedente:

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

Nell'esempio precedente `Description` è facoltativo perché non sono presenti annotazioni di dati.

Il form seguente convalida l'input dell'utente usando la convalida definita nel modello di `Starship`:

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

`EditForm` crea un oggetto `EditContext` come [valore di propagazione](xref:blazor/components#cascading-values-and-parameters) che tiene traccia dei metadati relativi al processo di modifica, inclusi i campi modificati e i messaggi di convalida correnti. Il `EditForm` fornisce anche gli eventi pratici per le invii validi e non validi (`OnValidSubmit`, `OnInvalidSubmit`). In alternativa, usare `OnSubmit` per attivare la convalida e verificare i valori dei campi con il codice di convalida personalizzato.

Nell'esempio seguente:

* Il metodo `HandleSubmit` viene eseguito quando viene selezionato il pulsante **Invia** .
* Il modulo viene convalidato utilizzando la `EditContext`del modulo.
* Il modulo viene ulteriormente convalidato passando il `EditContext` al metodo `ServerValidate` che chiama un endpoint dell'API Web nel server (*non illustrato*).
* Il codice aggiuntivo viene eseguito a seconda del risultato della convalida lato client e lato server controllando `isValid`.

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

## <a name="inputtext-based-on-the-input-event"></a>InputText basato sull'evento di input

Utilizzare il componente `InputText` per creare un componente personalizzato che utilizza l'evento `input` anziché l'evento `change`.

Creare un componente con il markup seguente e usare il componente proprio come `InputText` viene usato:

```razor
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="work-with-radio-buttons"></a>Usare i pulsanti di opzione

Quando si utilizzano pulsanti di opzione in un modulo, data binding viene gestito in modo diverso rispetto ad altri elementi perché i pulsanti di opzione vengono valutati come un gruppo. Il valore di ogni pulsante di opzione è fisso, ma il valore del gruppo di pulsanti di opzione è il valore del pulsante di opzione selezionato. L'esempio seguente illustra come:

* Gestire data binding per un gruppo di pulsanti di opzione.
* Supporta la convalida usando un componente `InputRadio` personalizzato.

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

Il `EditForm` seguente usa il componente `InputRadio` precedente per ottenere e convalidare una classificazione dall'utente:

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

## <a name="validation-support"></a>Supporto della convalida

Il componente `DataAnnotationsValidator` connette il supporto di convalida usando le annotazioni dei dati per la `EditContext`a cascata. L'abilitazione del supporto per la convalida usando le annotazioni dei dati richiede questo movimento esplicito. Per utilizzare un sistema di convalida diverso rispetto alle annotazioni dei dati, sostituire il `DataAnnotationsValidator` con un'implementazione personalizzata. L'implementazione del ASP.NET Core è disponibile per l'ispezione nell'origine riferimento: [DataAnnotationsValidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).

Blazor esegue due tipi di convalida:

* La *convalida dei campi* viene eseguita quando l'utente esce da un campo. Durante la convalida dei campi, il componente `DataAnnotationsValidator` associa tutti i risultati della convalida segnalati al campo.
* La *convalida del modello* viene eseguita quando l'utente invia il modulo. Durante la convalida del modello, il componente `DataAnnotationsValidator` tenta di determinare il campo in base al nome del membro segnalato dal risultato della convalida. I risultati della convalida che non sono associati a un singolo membro sono associati al modello anziché a un campo.

### <a name="validation-summary-and-validation-message-components"></a>Componenti del messaggio di convalida e di riepilogo della convalida

Il componente `ValidationSummary` riepiloga tutti i messaggi di convalida, che è simile all' [Helper tag del riepilogo di convalida](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper):

```razor
<ValidationSummary />
```

Messaggi di convalida dell'output per un modello specifico con il parametro `Model`:
  
```razor
<ValidationSummary Model="@starship" />
```

Il componente `ValidationMessage` Visualizza i messaggi di convalida per un campo specifico, che è simile all' [Helper tag del messaggio di convalida](xref:mvc/views/working-with-forms#the-validation-message-tag-helper). Specificare il campo per la convalida con l'attributo `For` e un'espressione lambda che denomina la proprietà Model:

```razor
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

I componenti `ValidationMessage` e `ValidationSummary` supportano attributi arbitrari. Qualsiasi attributo che non corrisponde a un parametro component viene aggiunto all'elemento `<div>` o `<ul>` generato.

### <a name="custom-validation-attributes"></a>Attributi di convalida personalizzati

Per assicurarsi che un risultato di convalida venga associato correttamente a un campo quando si usa un [attributo di convalida personalizzato](xref:mvc/models/validation#custom-attributes), passare la <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> del contesto di convalida durante la creazione del <xref:System.ComponentModel.DataAnnotations.ValidationResult>:

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

### <a name="opno-locblazor-data-annotations-validation-package"></a>pacchetto di convalida delle annotazioni dei dati Blazor

[Microsoft. AspNetCore.Blazor. DataAnnotations. Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) è un pacchetto che colma i gap dell'esperienza di convalida usando il componente `DataAnnotationsValidator`. Il pacchetto è attualmente *sperimentale*.

### <a name="compareproperty-attribute"></a>Attributo [CompareProperty]

Il <xref:System.ComponentModel.DataAnnotations.CompareAttribute> non funziona correttamente con il componente `DataAnnotationsValidator` perché non associa il risultato della convalida a un membro specifico. Ciò può comportare un comportamento incoerente tra la convalida a livello di campo e quando l'intero modello viene convalidato in un invio. [Microsoft. AspNetCore.Blazor. DataAnnotations.](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) il pacchetto *sperimentale* di convalida introduce un attributo di convalida aggiuntivo, `ComparePropertyAttribute`, che aggira queste limitazioni. In un'app Blazor `[CompareProperty]` è una sostituzione diretta per l'attributo `[Compare]`.

### <a name="nested-models-collection-types-and-complex-types"></a>Modelli annidati, tipi di raccolta e tipi complessi

Blazor fornisce il supporto per la convalida dell'input del form usando le annotazioni dei dati con la `DataAnnotationsValidator`predefinita. Tuttavia, il `DataAnnotationsValidator` convalida solo le proprietà di primo livello del modello associato al form che non sono proprietà di tipo raccolta o complesso.

Per convalidare l'intero grafico di oggetti del modello associato, incluse le proprietà della raccolta e del tipo complesso, utilizzare la `ObjectGraphDataAnnotationsValidator` fornita dall'oggetto *sperimentale* [Microsoft. AspNetCore.Blazor. DataAnnotations. Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) (pacchetto):

```razor
<EditForm Model="@model" OnValidSubmit="HandleValidSubmit">
    <ObjectGraphDataAnnotationsValidator />
    ...
</EditForm>
```

Annotare le proprietà del modello con `[ValidateComplexType]`. Nelle classi del modello seguenti, la classe `ShipDescription` contiene annotazioni di dati aggiuntive da convalidare quando il modello è associato al form:

*Starship.cs*:

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

*ShipDescription.cs*:

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

### <a name="enable-the-submit-button-based-on-form-validation"></a>Abilita il pulsante Invia in base alla convalida del modulo

Per abilitare e disabilitare il pulsante Invia in base alla convalida del modulo:

* Utilizzare la `EditContext` del modulo per assegnare il modello quando il componente viene inizializzato.
* Convalidare il form nel callback `OnFieldChanged` del contesto per abilitare e disabilitare il pulsante Submit.

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

Nell'esempio precedente impostare `formInvalid` su `false` se:

* Il modulo viene precaricato con valori predefiniti validi.
* Si desidera attivare il pulsante Invia quando il modulo viene caricato.

Un effetto collaterale dell'approccio precedente è che un componente `ValidationSummary` viene popolato con campi non validi dopo che l'utente interagisce con un qualsiasi campo. Questo scenario può essere risolto in uno dei modi seguenti:

* Non usare un componente `ValidationSummary` nel form.
* Rendere visibile il componente `ValidationSummary` quando viene selezionato il pulsante Invia, ad esempio in un metodo di `HandleValidSubmit`.

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
