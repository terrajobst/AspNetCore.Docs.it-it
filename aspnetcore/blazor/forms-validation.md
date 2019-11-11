---
title: Moduli e convalida di ASP.NET Core Blazer
author: guardrex
description: Informazioni su come usare i moduli e gli scenari di convalida del campo in blazer.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/04/2019
uid: blazor/forms-validation
ms.openlocfilehash: 6dcc36c5133367493b476655dbdf73b75db9d168
ms.sourcegitcommit: a7bbe3890befead19440075b05b9674351f98872
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2019
ms.locfileid: "73905739"
---
# <a name="aspnet-core-blazor-forms-and-validation"></a>Moduli e convalida di ASP.NET Core Blazer

Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)

I moduli e la convalida sono supportati in blazer usando le [annotazioni dei dati](xref:mvc/models/validation).

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

```csharp
<EditForm Model="@exampleModel" OnValidSubmit="@HandleValidSubmit">
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

* Il modulo convalida l'input dell'utente nel campo `name` usando la convalida definita nel tipo di `ExampleModel`. Il modello viene creato nel blocco `@code` del componente e viene mantenuto in un campo privato (`exampleModel`). Il campo viene assegnato all'attributo `Model` dell'elemento `<EditForm>`.
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

Il `EditForm` crea una `EditContext` come [valore](xref:blazor/components#cascading-values-and-parameters) di propagazione che tiene traccia dei metadati relativi al processo di modifica, inclusi i campi modificati e i messaggi di convalida correnti. Il `EditForm` fornisce anche gli eventi pratici per le invii validi e non validi (`OnValidSubmit`, `OnInvalidSubmit`). In alternativa, usare `OnSubmit` per attivare la convalida e verificare i valori dei campi con il codice di convalida personalizzato.

## <a name="inputtext-based-on-the-input-event"></a>InputText basato sull'evento di input

Utilizzare il componente `InputText` per creare un componente personalizzato che utilizza l'evento `input` anziché l'evento `change`.

Creare un componente con il markup seguente e usare il componente proprio come `InputText` viene usato:

```cshtml
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="validation-support"></a>Supporto della convalida

Il componente `DataAnnotationsValidator` connette il supporto di convalida usando le annotazioni dei dati per la `EditContext`a cascata. L'abilitazione del supporto per la convalida usando le annotazioni dei dati richiede questo movimento esplicito. Per utilizzare un sistema di convalida diverso rispetto alle annotazioni dei dati, sostituire il `DataAnnotationsValidator` con un'implementazione personalizzata. L'implementazione del ASP.NET Core è disponibile per l'ispezione nell'origine riferimento: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).

Il componente `ValidationSummary` riepiloga tutti i messaggi di convalida, che è simile all' [Helper tag di riepilogo della convalida](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).

Il componente `ValidationMessage` Visualizza i messaggi di convalida per un campo specifico, che è simile all' [Helper tag del messaggio di convalida](xref:mvc/views/working-with-forms#the-validation-message-tag-helper). Specificare il campo per la convalida con l'attributo `For` e un'espressione lambda che denomina la proprietà Model:

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

I componenti `ValidationMessage` e `ValidationSummary` supportano attributi arbitrari. Qualsiasi attributo che non corrisponde a un parametro component viene aggiunto all'elemento `<div>` o `<ul>` generato.

::: moniker range=">= aspnetcore-3.1"

**Pacchetto Microsoft. AspNetCore. Blazer. DataAnnotations. Validation**

[Microsoft. AspNetCore. Blazer. DataAnnotations. Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) è un pacchetto che colma i gap dell'esperienza di convalida usando il componente `DataAnnotationsValidator`. Il pacchetto è attualmente *sperimentale*e si prevede di aggiungere questi scenari all'ASP.NET Core Framework in una versione futura.

Il componente `DataAnnotationsValidator` non convalida le sottoproprietà di proprietà complesse in un modello di convalida. Gli elementi delle proprietà del tipo di raccolta non vengono convalidati. Per convalidare questi tipi, il pacchetto di `Microsoft.AspNetCore.Blazor.DataAnnotations.Validation` introduce l'attributo di convalida `ValidateComplexType` che funziona in tandem con il componente `ObjectGraphDataAnnotationsValidator`. Per un esempio di questi tipi in uso, vedere l' [esempio di convalida di Blazer nel repository di GitHub ASPNET/Samples ](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).

Il <xref:System.ComponentModel.DataAnnotations.CompareAttribute> non funziona correttamente con il componente `DataAnnotationsValidator`. Il pacchetto `Microsoft.AspNetCore.Blazor.DataAnnotations.Validation` introduce un attributo di convalida aggiuntivo, `ComparePropertyAttribute`, che risolve tali limitazioni. In un'app Blazer `ComparePropertyAttribute` è una sostituzione diretta del `CompareAttribute`. Per ulteriori informazioni, vedere [CompareAttribute ignorato con OnValidSubmit EditForm (ASPNET/AspNetCore \#10643)](https://github.com/aspnet/AspNetCore/issues/10643#issuecomment-543909748).

::: moniker-end

::: moniker range="< aspnetcore-3.1"

### <a name="validation-of-complex-or-collection-type-properties"></a>Convalida di proprietà di tipo complesso o di raccolta

Gli attributi di convalida applicati alle proprietà di un modello vengono convalidati quando viene inviato il modulo. Tuttavia, le proprietà delle raccolte o dei tipi di dati complessi di un modello non vengono convalidate durante l'invio del form da parte del componente `DataAnnotationsValidator`. Per rispettare gli attributi di convalida annidati in questo scenario, usare un componente di convalida personalizzato. Per un esempio, vedere l' [esempio di convalida di Blazer nel repository GitHub ASPNET/Samples](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).

::: moniker-end
