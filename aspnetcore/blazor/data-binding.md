---
title: ASP.NET Core Blazor data binding
author: guardrex
description: Informazioni sugli scenari di data binding per i componenti e gli elementi DOM nelle app Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/data-binding
ms.openlocfilehash: c38e6095d4e93d3eead10fa8bb0356b2113bb220
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453233"
---
# <a name="aspnet-core-opno-locblazor-data-binding"></a><span data-ttu-id="f35fe-103">ASP.NET Core Blazor data binding</span><span class="sxs-lookup"><span data-stu-id="f35fe-103">ASP.NET Core Blazor data binding</span></span>

<span data-ttu-id="f35fe-104">Di [Luke Latham](https://github.com/guardrex) e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="f35fe-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="f35fe-105">L'associazione dati a entrambi i componenti e gli elementi DOM viene eseguita con l'attributo [`@bind`](xref:mvc/views/razor#bind) .</span><span class="sxs-lookup"><span data-stu-id="f35fe-105">Data binding to both components and DOM elements is accomplished with the [`@bind`](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="f35fe-106">Nell'esempio seguente viene associato un `CurrentValue` proprietà al valore della casella di testo:</span><span class="sxs-lookup"><span data-stu-id="f35fe-106">The following example binds a `CurrentValue` property to the text box's value:</span></span>

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="f35fe-107">Quando la casella di testo perde lo stato attivo, viene aggiornato il valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="f35fe-107">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="f35fe-108">La casella di testo viene aggiornata nell'interfaccia utente solo quando viene eseguito il rendering del componente, non in risposta alla modifica del valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="f35fe-108">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="f35fe-109">Poiché i componenti eseguono il rendering dopo l'esecuzione del codice del gestore eventi, gli aggiornamenti delle proprietà vengono in *genere* riflessi nell'interfaccia utente immediatamente dopo l'attivazione di un gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="f35fe-109">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="f35fe-110">L'uso di `@bind` con la proprietà `CurrentValue` (`<input @bind="CurrentValue" />`) è essenzialmente equivalente a quanto segue:</span><span class="sxs-lookup"><span data-stu-id="f35fe-110">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="f35fe-111">Quando viene eseguito il rendering del componente, il `value` dell'elemento di input deriva dalla proprietà `CurrentValue`.</span><span class="sxs-lookup"><span data-stu-id="f35fe-111">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="f35fe-112">Quando l'utente digita nella casella di testo e modifica lo stato attivo dell'elemento, viene generato l'evento `onchange` e la proprietà `CurrentValue` viene impostata sul valore modificato.</span><span class="sxs-lookup"><span data-stu-id="f35fe-112">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="f35fe-113">In realtà, la generazione del codice è più complessa perché `@bind` gestisce i casi in cui vengono eseguite le conversioni dei tipi.</span><span class="sxs-lookup"><span data-stu-id="f35fe-113">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="f35fe-114">In linea di principio, `@bind` associa il valore corrente di un'espressione a un attributo `value` e gestisce le modifiche utilizzando il gestore registrato.</span><span class="sxs-lookup"><span data-stu-id="f35fe-114">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="f35fe-115">Oltre a gestire gli eventi di `onchange` con `@bind` sintassi, una proprietà o un campo può essere associato utilizzando altri eventi specificando un attributo [`@bind-value`](xref:mvc/views/razor#bind) con un parametro `event` ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span><span class="sxs-lookup"><span data-stu-id="f35fe-115">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [`@bind-value`](xref:mvc/views/razor#bind) attribute with an `event` parameter ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="f35fe-116">Nell'esempio seguente viene associata la proprietà `CurrentValue` per l'evento `oninput`:</span><span class="sxs-lookup"><span data-stu-id="f35fe-116">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```razor
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="f35fe-117">A differenza `onchange`, che viene attivato quando l'elemento perde lo stato attivo, `oninput` generato quando viene modificato il valore della casella di testo.</span><span class="sxs-lookup"><span data-stu-id="f35fe-117">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="f35fe-118">`@bind-value` nell'esempio precedente esegue il binding:</span><span class="sxs-lookup"><span data-stu-id="f35fe-118">`@bind-value` in the preceding example binds:</span></span>

* <span data-ttu-id="f35fe-119">Espressione specificata (`CurrentValue`) nell'attributo `value` dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="f35fe-119">The specified expression (`CurrentValue`) to the element's `value` attribute.</span></span>
* <span data-ttu-id="f35fe-120">Delegato dell'evento di modifica all'evento specificato da `@bind-value:event`.</span><span class="sxs-lookup"><span data-stu-id="f35fe-120">A change event delegate to the event specified by `@bind-value:event`.</span></span>

### <a name="unparsable-values"></a><span data-ttu-id="f35fe-121">Valori non analizzabili</span><span class="sxs-lookup"><span data-stu-id="f35fe-121">Unparsable values</span></span>

<span data-ttu-id="f35fe-122">Quando un utente fornisce un valore non analizzabile a un elemento associato a un oggetto DataBound, il valore non analizzabile viene automaticamente ripristinato al valore precedente quando viene attivato l'evento bind.</span><span class="sxs-lookup"><span data-stu-id="f35fe-122">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="f35fe-123">Si consideri lo scenario seguente:</span><span class="sxs-lookup"><span data-stu-id="f35fe-123">Consider the following scenario:</span></span>

* <span data-ttu-id="f35fe-124">Un elemento `<input>` è associato a un tipo di `int` con un valore iniziale di `123`:</span><span class="sxs-lookup"><span data-stu-id="f35fe-124">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="f35fe-125">L'utente aggiorna il valore dell'elemento per `123.45` nella pagina e modifica lo stato attivo dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="f35fe-125">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="f35fe-126">Nello scenario precedente, il valore dell'elemento viene ripristinato in `123`.</span><span class="sxs-lookup"><span data-stu-id="f35fe-126">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="f35fe-127">Quando il valore `123.45` viene rifiutato a favore del valore originale di `123`, l'utente riconosce che il relativo valore non è stato accettato.</span><span class="sxs-lookup"><span data-stu-id="f35fe-127">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="f35fe-128">Per impostazione predefinita, l'associazione si applica all'evento `onchange` dell'elemento (`@bind="{PROPERTY OR FIELD}"`).</span><span class="sxs-lookup"><span data-stu-id="f35fe-128">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="f35fe-129">Usare `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` per impostare un evento diverso.</span><span class="sxs-lookup"><span data-stu-id="f35fe-129">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="f35fe-130">Per l'evento `oninput` (`@bind-value:event="oninput"`), la riversione viene eseguita dopo qualsiasi sequenza di tasti che introduce un valore non analizzabile.</span><span class="sxs-lookup"><span data-stu-id="f35fe-130">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="f35fe-131">Quando la destinazione è `oninput` evento con un tipo associato a `int`, a un utente viene impedito di digitare un `.` carattere.</span><span class="sxs-lookup"><span data-stu-id="f35fe-131">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="f35fe-132">Un carattere `.` viene immediatamente rimosso, quindi l'utente riceve il feedback immediato che sono consentiti solo numeri interi.</span><span class="sxs-lookup"><span data-stu-id="f35fe-132">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="f35fe-133">Esistono scenari in cui il ripristino del valore nell'evento `oninput` non è ideale, ad esempio quando l'utente deve essere autorizzato a cancellare un valore `<input>` non analizzabile.</span><span class="sxs-lookup"><span data-stu-id="f35fe-133">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="f35fe-134">Le alternative includono:</span><span class="sxs-lookup"><span data-stu-id="f35fe-134">Alternatives include:</span></span>

* <span data-ttu-id="f35fe-135">Non usare l'evento `oninput`.</span><span class="sxs-lookup"><span data-stu-id="f35fe-135">Don't use the `oninput` event.</span></span> <span data-ttu-id="f35fe-136">Usare l'evento `onchange` predefinito (`@bind="{PROPERTY OR FIELD}"`), in cui un valore non valido non viene ripristinato fino a quando l'elemento non perde lo stato attivo.</span><span class="sxs-lookup"><span data-stu-id="f35fe-136">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="f35fe-137">Eseguire l'associazione a un tipo nullable, ad esempio `int?` o `string`, e fornire la logica personalizzata per gestire le voci non valide.</span><span class="sxs-lookup"><span data-stu-id="f35fe-137">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="f35fe-138">Utilizzare un [componente di convalida del modulo](xref:blazor/forms-validation), ad esempio `InputNumber` o `InputDate`.</span><span class="sxs-lookup"><span data-stu-id="f35fe-138">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="f35fe-139">I componenti di convalida dei moduli includono il supporto predefinito per la gestione di input non validi.</span><span class="sxs-lookup"><span data-stu-id="f35fe-139">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="f35fe-140">Componenti di convalida dei moduli:</span><span class="sxs-lookup"><span data-stu-id="f35fe-140">Form validation components:</span></span>
  * <span data-ttu-id="f35fe-141">Consente all'utente di fornire un input non valido e di ricevere errori di convalida nel `EditContext`associato.</span><span class="sxs-lookup"><span data-stu-id="f35fe-141">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="f35fe-142">Visualizzare gli errori di convalida nell'interfaccia utente senza interferire con l'utente che immette dati Web Form aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="f35fe-142">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

### <a name="format-strings"></a><span data-ttu-id="f35fe-143">Stringhe di formato</span><span class="sxs-lookup"><span data-stu-id="f35fe-143">Format strings</span></span>

<span data-ttu-id="f35fe-144">Il data binding funziona con stringhe di formato <xref:System.DateTime> usando [`@bind:format`](xref:mvc/views/razor#bind).</span><span class="sxs-lookup"><span data-stu-id="f35fe-144">Data binding works with <xref:System.DateTime> format strings using [`@bind:format`](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="f35fe-145">Altre espressioni di formato, ad esempio i formati di valuta o numerici, non sono disponibili in questo momento.</span><span class="sxs-lookup"><span data-stu-id="f35fe-145">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="f35fe-146">Nel codice precedente, il tipo di campo dell'elemento `<input>` (`type`) viene impostato come valore predefinito `text`.</span><span class="sxs-lookup"><span data-stu-id="f35fe-146">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="f35fe-147">`@bind:format` è supportato per l'associazione dei tipi .NET seguenti:</span><span class="sxs-lookup"><span data-stu-id="f35fe-147">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="f35fe-148"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="f35fe-148"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="f35fe-149"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="f35fe-149"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="f35fe-150">L'attributo `@bind:format` specifica il formato di data da applicare al `value` dell'elemento `<input>`.</span><span class="sxs-lookup"><span data-stu-id="f35fe-150">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="f35fe-151">Il formato viene usato anche per analizzare il valore quando si verifica un evento `onchange`.</span><span class="sxs-lookup"><span data-stu-id="f35fe-151">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="f35fe-152">Non è consigliabile specificare un formato per il tipo di campo `date` perché Blazor dispone del supporto incorporato per formattare le date.</span><span class="sxs-lookup"><span data-stu-id="f35fe-152">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span> <span data-ttu-id="f35fe-153">A prescindere dalla raccomandazione, utilizzare solo il formato di data `yyyy-MM-dd` per il corretto funzionamento dell'associazione se viene fornito un formato con il tipo di campo `date`:</span><span class="sxs-lookup"><span data-stu-id="f35fe-153">In spite of the recommendation, only use the `yyyy-MM-dd` date format for binding to work correctly if a format is supplied with the `date` field type:</span></span>

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

### <a name="parent-to-child-binding-with-component-parameters"></a><span data-ttu-id="f35fe-154">Associazione da padre a figlio con parametri dei componenti</span><span class="sxs-lookup"><span data-stu-id="f35fe-154">Parent-to-child binding with component parameters</span></span>

<span data-ttu-id="f35fe-155">Il binding riconosce i parametri del componente, dove `@bind-{property}` può associare un valore di proprietà da un componente padre a un componente figlio.</span><span class="sxs-lookup"><span data-stu-id="f35fe-155">Binding recognizes component parameters, where `@bind-{property}` can bind a property value from a parent component down to a child component.</span></span> <span data-ttu-id="f35fe-156">Il binding da un figlio a un padre viene trattato nell' [associazione da figlio a padre con la sezione di binding concatenata](#child-to-parent-binding-with-chained-bind) .</span><span class="sxs-lookup"><span data-stu-id="f35fe-156">Binding from a child to a parent is covered in the [Child-to-parent binding with chained bind](#child-to-parent-binding-with-chained-bind) section.</span></span>

<span data-ttu-id="f35fe-157">Il componente figlio seguente (`ChildComponent`) dispone di un parametro del componente `Year` e di un callback `YearChanged`:</span><span class="sxs-lookup"><span data-stu-id="f35fe-157">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

```razor
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="f35fe-158">`EventCallback<T>` viene spiegato in <xref:blazor/event-handling#eventcallback>.</span><span class="sxs-lookup"><span data-stu-id="f35fe-158">`EventCallback<T>` is explained in <xref:blazor/event-handling#eventcallback>.</span></span>

<span data-ttu-id="f35fe-159">Il componente padre seguente utilizza:</span><span class="sxs-lookup"><span data-stu-id="f35fe-159">The following parent component uses:</span></span>

* <span data-ttu-id="f35fe-160">`ChildComponent` e associa il parametro `ParentYear` dall'elemento padre al parametro `Year` sul componente figlio.</span><span class="sxs-lookup"><span data-stu-id="f35fe-160">`ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component.</span></span>
* <span data-ttu-id="f35fe-161">L'evento `onclick` viene usato per attivare il metodo di `ChangeTheYear`.</span><span class="sxs-lookup"><span data-stu-id="f35fe-161">The `onclick` event is used to trigger the `ChangeTheYear` method.</span></span> <span data-ttu-id="f35fe-162">Per ulteriori informazioni, vedere <xref:blazor/event-handling>.</span><span class="sxs-lookup"><span data-stu-id="f35fe-162">For more information, see <xref:blazor/event-handling>.</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent @bind-Year="ParentYear" />

<button class="btn btn-primary" @onclick="ChangeTheYear">
    Change Year to 1986
</button>

@code {
    [Parameter]
    public int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="f35fe-163">Il caricamento del `ParentComponent` produce il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="f35fe-163">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="f35fe-164">Se il valore della proprietà `ParentYear` viene modificato selezionando il pulsante nell'`ParentComponent`, viene aggiornata la proprietà `Year` del `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="f35fe-164">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="f35fe-165">Il rendering del nuovo valore di `Year` viene eseguito nell'interfaccia utente quando viene eseguito il rendering del `ParentComponent`:</span><span class="sxs-lookup"><span data-stu-id="f35fe-165">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="f35fe-166">Il parametro `Year` è associabile perché contiene un evento `YearChanged` complementare che corrisponde al tipo del parametro `Year`.</span><span class="sxs-lookup"><span data-stu-id="f35fe-166">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="f35fe-167">Per convenzione, `<ChildComponent @bind-Year="ParentYear" />` è essenzialmente equivalente alla scrittura:</span><span class="sxs-lookup"><span data-stu-id="f35fe-167">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="f35fe-168">In generale, una proprietà può essere associata a un gestore eventi corrispondente usando `@bind-property:event` attributo.</span><span class="sxs-lookup"><span data-stu-id="f35fe-168">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="f35fe-169">Ad esempio, la proprietà `MyProp` può essere associata a `MyEventHandler` utilizzando i due attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f35fe-169">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

### <a name="child-to-parent-binding-with-chained-bind"></a><span data-ttu-id="f35fe-170">Associazione da figlio a padre con binding concatenato</span><span class="sxs-lookup"><span data-stu-id="f35fe-170">Child-to-parent binding with chained bind</span></span>

<span data-ttu-id="f35fe-171">Uno scenario comune consiste nel concatenare un parametro associato a dati a un elemento Page nell'output del componente.</span><span class="sxs-lookup"><span data-stu-id="f35fe-171">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="f35fe-172">Questo scenario è denominato *Binding concatenato* perché si verificano simultaneamente più livelli di associazione.</span><span class="sxs-lookup"><span data-stu-id="f35fe-172">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="f35fe-173">Un binding concatenato non può essere implementato con `@bind` sintassi nell'elemento della pagina.</span><span class="sxs-lookup"><span data-stu-id="f35fe-173">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="f35fe-174">Il gestore eventi e il valore devono essere specificati separatamente.</span><span class="sxs-lookup"><span data-stu-id="f35fe-174">The event handler and value must be specified separately.</span></span> <span data-ttu-id="f35fe-175">Un componente padre, tuttavia, può utilizzare la sintassi `@bind` con il parametro del componente.</span><span class="sxs-lookup"><span data-stu-id="f35fe-175">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="f35fe-176">Il componente `PasswordField` seguente (*PasswordField. Razor*):</span><span class="sxs-lookup"><span data-stu-id="f35fe-176">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="f35fe-177">Imposta il valore di un elemento `<input>` su una proprietà `Password`.</span><span class="sxs-lookup"><span data-stu-id="f35fe-177">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="f35fe-178">Espone le modifiche della proprietà `Password` a un componente padre con un [EventCallback](xref:blazor/event-handling#eventcallback).</span><span class="sxs-lookup"><span data-stu-id="f35fe-178">Exposes changes of the `Password` property to a parent component with an [EventCallback](xref:blazor/event-handling#eventcallback).</span></span>
* <span data-ttu-id="f35fe-179">Usa l'evento `onclick` viene usato per attivare il metodo di `ToggleShowPassword`.</span><span class="sxs-lookup"><span data-stu-id="f35fe-179">Uses the `onclick` event is used to trigger the `ToggleShowPassword` method.</span></span> <span data-ttu-id="f35fe-180">Per ulteriori informazioni, vedere <xref:blazor/event-handling>.</span><span class="sxs-lookup"><span data-stu-id="f35fe-180">For more information, see <xref:blazor/event-handling>.</span></span>

```razor
<h1>Child Component</h2>

Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool _showPassword;

    [Parameter]
    public string Password { get; set; }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        _showPassword = !_showPassword;
    }
}
```

<span data-ttu-id="f35fe-181">Il componente `PasswordField` viene usato in un altro componente:</span><span class="sxs-lookup"><span data-stu-id="f35fe-181">The `PasswordField` component is used in another component:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<PasswordField @bind-Password="_password" />

@code {
    private string _password;
}
```

<span data-ttu-id="f35fe-182">Per eseguire controlli o intercettare gli errori sulla password nell'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="f35fe-182">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="f35fe-183">Creare un campo di supporto per `Password` (`_password` nell'esempio di codice seguente).</span><span class="sxs-lookup"><span data-stu-id="f35fe-183">Create a backing field for `Password` (`_password` in the following example code).</span></span>
* <span data-ttu-id="f35fe-184">Eseguire i controlli o gli errori di trap nel Setter del `Password`.</span><span class="sxs-lookup"><span data-stu-id="f35fe-184">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="f35fe-185">Nell'esempio seguente viene fornito un feedback immediato all'utente se viene utilizzato uno spazio nel valore della password:</span><span class="sxs-lookup"><span data-stu-id="f35fe-185">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@_validationMessage</span>

@code {
    private bool _showPassword;
    private string _password;
    private string _validationMessage;

    [Parameter]
    public string Password
    {
        get { return _password ?? string.Empty; }
        set
        {
            if (_password != value)
            {
                if (value.Contains(' '))
                {
                    _validationMessage = "Spaces not allowed!";
                }
                else
                {
                    _password = value;
                    _validationMessage = string.Empty;
                }
            }
        }
    }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        _showPassword = !_showPassword;
    }
}
```

### <a name="radio-buttons"></a><span data-ttu-id="f35fe-186">Pulsanti di opzione</span><span class="sxs-lookup"><span data-stu-id="f35fe-186">Radio buttons</span></span>

<span data-ttu-id="f35fe-187">Per informazioni sull'associazione ai pulsanti di opzione in un modulo, vedere <xref:blazor/forms-validation#work-with-radio-buttons>.</span><span class="sxs-lookup"><span data-stu-id="f35fe-187">For information on binding to radio buttons in a form, see <xref:blazor/forms-validation#work-with-radio-buttons>.</span></span>
