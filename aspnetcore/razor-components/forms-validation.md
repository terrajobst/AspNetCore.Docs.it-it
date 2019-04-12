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
# <a name="razor-components-forms-and-validation"></a>Convalida e razor componenti form

Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)

Moduli e convalida sono supportati nei componenti di Razor usando [annotazioni dei dati](xref:mvc/models/validation).

> [!NOTE]
> Moduli e gli scenari di convalida sono soggette a modifica a ogni versione di anteprima.

Nell'esempio `ExampleModel` tipo definisce la logica di convalida tramite le annotazioni dei dati:

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

Un modulo viene definito mediante il `<EditForm>` componente. Elementi tipici, componenti e codice Razor, viene illustrato il formato seguente:

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

* Il modulo convalida l'input utente nel `name` campo usando il processo di convalida definito nel `ExampleModel` tipo. La creazione del modello nella proprietà del componente `@functions` bloccare e contenuti in un campo privato (`exampleModel`). È assegnato il campo per il `Model` attributo del `<EditForm>`.
* Il componente del Validator di annotazioni dei dati (`<DataAnnotationsValidator>`) collega supporto per la convalida tramite le annotazioni dei dati.
* Il componente di riepilogo della convalida (`<ValidationSummary>`) vengono riepilogati i messaggi di convalida.
* `HandleValidSubmit` viene attivato quando il form invia correttamente (supera la convalida).

Un set di componenti di input predefiniti sono disponibili per ricevere e convalidare l'input dell'utente. Gli input vengono convalidati quando si è modificati e quando viene inviato un modulo. Nella tabella seguente vengono visualizzati i componenti di input disponibili.

| Componente di input   | Sottoposto a rendering come&hellip;       |
| ----------------- | ------------------------- |
| `<InputText>`     | `<input>`                 |
| `<InputTextArea>` | `<textarea>`              |
| `<InputSelect>`   | `<select>`                |
| `<InputNumber>`   | `<input type="number">`   |
| `<InputCheckbox>` | `<input type="checkbox">` |
| `<InputDate>`     | `<input type="date">`     |

Componenti di input specificare il comportamento predefinito per la convalida su una modifica e modificando la relativa classe CSS per riflettere lo stato di campo. Alcuni componenti di includono logica di analisi utile. Ad esempio, `<InputDate>` e `<InputNumber>` gestire correttamente i valori non analizzabile registrandoli come errori di convalida. I tipi che possono accettare valori null supportano anche i valori null del campo di destinazione (ad esempio, `int?`).

Quanto segue `Starship` tipo definisce la logica di convalida usando un set più ampio di proprietà e [annotazioni dei dati](xref:mvc/models/validation) del precedente `ExampleModel`:

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

Il formato seguente convalida input dell'utente tramite il processo di convalida definito nel `Starship` modello:

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

Il `<EditForm>` crea un' `EditContext` come una [valore CSS](xref:razor-components/components#cascading-values-and-parameters) che tiene traccia dei metadati sul processo di modifica, tra cui i campi che sono stati modificati e i messaggi di convalida corrente. Il `<EditForm>` fornisce anche eventi pratici per Invia valido e non è valido (`OnValidSubmit`, `OnInvalidSubmit`). In alternativa, usare `OnSubmit` per attivare la convalida e controllare i valori dei campi con codice di convalida personalizzato.

Il componente del Validator di annotazioni dei dati (`<DataAnnotationsValidator>`) collega supporto per la convalida tramite le annotazioni dei dati per il sottomenu `EditContext`. Abilitazione del supporto per la convalida tramite le annotazioni dei dati attualmente richiede questo movimento esplicito, ma si sta considerando rende questo il comportamento predefinito che è quindi possibile eseguire l'override. Per usare un sistema di convalida diverso rispetto a annotazioni dei dati, sostituire il Validator di annotazioni dei dati con un'implementazione personalizzata. L'implementazione di ASP.NET Core è disponibile per l'ispezione dell'origine di riferimento: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs). *L'implementazione di ASP.NET Core è soggetto agli aggiornamenti rapidi durante il periodo di versione di anteprima.*

Il componente di riepilogo della convalida (`<ValidationSummary>`) vengono riepilogati tutti i messaggi di convalida, che è simile al [Helper per Tag riepilogo di convalida](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).

Il componente di convalida (`<ValidationMessage>`) consente di visualizzare i messaggi di convalida per un campo specifico, che è simile al [Helper Tag Validation Message](xref:mvc/views/working-with-forms#the-validation-message-tag-helper). Specificare il campo per la convalida con il `For` attributo e un'espressione lambda la proprietà del modello di denominazione:

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

> [!NOTE]
> Componenti predefiniti di input presentano delle limitazioni che si prevedono di risolvere nelle future versioni. Ad esempio, è possibile specificare gli attributi arbitrari su generato `<input>` tag. Compilare il proprio le sottoclassi di componenti per gestire scenari non disponibili.
