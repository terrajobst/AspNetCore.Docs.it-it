---
title: Creare e usare ASP.NET Core componenti Razor
author: guardrex
description: Informazioni su come creare e usare i componenti Razor, tra cui la modalità di associazione ai dati, la gestione degli eventi e la gestione dei cicli di vita dei componenti.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/05/2019
uid: blazor/components
ms.openlocfilehash: a71bbf3921417cbd23aeb14d0d78ad8354d6e93a
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/15/2019
ms.locfileid: "72378693"
---
# <a name="create-and-use-aspnet-core-razor-components"></a>Creare e usare ASP.NET Core componenti Razor

Di [Luke Latham](https://github.com/guardrex) e [Daniel Roth](https://github.com/danroth27)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))

Le app Blazer vengono compilate usando i *componenti*. Un componente è un blocco di interfaccia utente (UI) autonomo, ad esempio una pagina, una finestra di dialogo o un form. Un componente include il markup HTML e la logica di elaborazione necessaria per inserire i dati o rispondere agli eventi dell'interfaccia utente. I componenti sono flessibili e leggeri. Possono essere annidati, riutilizzati e condivisi tra i progetti.

## <a name="component-classes"></a>Classi di componenti

I componenti sono implementati in file di componente [Razor](xref:mvc/views/razor) (*Razor*) usando una C# combinazione di e markup HTML. Un componente in blazer viene definito formalmente come *componente Razor*.

Il nome di un componente deve iniziare con un carattere maiuscolo. Ad esempio, *MyCoolComponent. Razor* è valido e *MyCoolComponent. Razor* non è valido.

L'interfaccia utente per un componente viene definita utilizzando HTML. La logica di rendering dinamico (ad esempio, cicli, condizioni, espressioni) viene aggiunta usando una sintassi C# incorporata chiamata [Razor](xref:mvc/views/razor). Quando viene compilata un'app, il markup C# HTML e la logica di rendering vengono convertiti in una classe Component. Il nome della classe generata corrisponde al nome del file.

I membri della classe del componente vengono definiti in un blocco `@code`. Nel blocco `@code`, lo stato del componente (proprietà, campi) viene specificato con i metodi per la gestione degli eventi o per la definizione di altre logiche dei componenti. È consentito più di un blocco `@code`.

> [!NOTE]
> Nelle anteprime precedenti di ASP.NET Core 3,0, i blocchi `@functions` venivano usati per lo stesso scopo dei blocchi `@code` nei componenti Razor. i blocchi `@functions` continuano a funzionare nei componenti Razor, ma è consigliabile usare il blocco `@code` in ASP.NET Core 3,0 Preview 6 o versione successiva.

I membri dei componenti possono essere usati come parte della logica di rendering del C# componente usando espressioni che iniziano con `@`. Il rendering di un C# campo, ad esempio, viene eseguito anteponendo il prefisso `@` al nome del campo. Nell'esempio seguente viene valutato ed eseguito il rendering:

* `_headingFontStyle` al valore della proprietà CSS per `font-style`.
* `_headingText` al contenuto dell'elemento `<h1>`.

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

Una volta eseguito il rendering iniziale del componente, il componente rigenera l'albero di rendering in risposta agli eventi. Blazer confronta quindi il nuovo albero di rendering con quello precedente e applica le modifiche apportate all'Document Object Model (DOM) del browser.

I componenti sono C# classi ordinarie e possono essere inseriti in qualsiasi punto all'interno di un progetto. I componenti che producono pagine Web in genere risiedono nella cartella *pages* . I componenti non di pagina vengono spesso inseriti nella cartella *condivisa* o in una cartella personalizzata aggiunta al progetto. Per usare una cartella personalizzata, aggiungere lo spazio dei nomi della cartella personalizzata al componente padre o al file *_Imports. Razor* dell'app. Lo spazio dei nomi seguente, ad esempio, rende disponibili i componenti in una cartella di *componenti* quando lo spazio dei nomi radice dell'app è `WebApplication`:

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a>Integrare i componenti nelle app Razor Pages e MVC

Usare i componenti con le app Razor Pages e MVC esistenti. Non è necessario riscrivere le pagine o le visualizzazioni esistenti per usare i componenti Razor. Quando viene eseguito il rendering della pagina o della vista, i componenti vengono prerenderizzati contemporaneamente.

Per eseguire il rendering di un componente da una pagina o da una vista, usare il metodo helper HTML `RenderComponentAsync<TComponent>`:

```cshtml
<div id="MyComponent">
    @(await Html.RenderComponentAsync<MyComponent>(RenderMode.ServerPrerendered))
</div>
```

Mentre le pagine e le visualizzazioni possono usare i componenti, il contrario non è vero. I componenti non possono usare scenari di visualizzazione e di pagina specifici, ad esempio visualizzazioni parziali e sezioni. Per usare la logica dalla visualizzazione parziale in un componente, scomporre la logica di visualizzazione parziale in un componente.

Per altre informazioni su come viene eseguito il rendering dei componenti e lo stato del componente viene gestito nelle app del server blazer, vedere l'articolo <xref:blazor/hosting-models>.

## <a name="use-components"></a>Usare i componenti

I componenti possono includere altri componenti dichiarando questi ultimi usando la sintassi degli elementi HTML. Il markup per l'uso di un componente è simile a un tag HTML, in cui il nome del tag è il tipo di componente.

L'associazione di attributi distingue tra maiuscole e minuscole. Ad esempio, `@bind` è valido e `@Bind` non è valido.

Il markup seguente in *index. Razor* esegue il rendering di un'istanza `HeadingComponent`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

*Components/HeadingComponent. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

Se un componente contiene un elemento HTML con una prima lettera maiuscola che non corrisponde a un nome di componente, viene emesso un avviso che indica che l'elemento ha un nome imprevisto. L'aggiunta di un'istruzione `@using` per lo spazio dei nomi del componente rende disponibile il componente, che rimuove l'avviso.

## <a name="component-parameters"></a>Parametri del componente

I componenti possono avere *parametri del componente*, che vengono definiti usando proprietà pubbliche nella classe Component con l'attributo `[Parameter]`. Usare gli attributi per specificare gli argomenti per un componente nel markup.

*Components/ChildComponent. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

Nell'esempio seguente, il `ParentComponent` imposta il valore della proprietà `Title` del `ChildComponent`.

*Pages/ParentComponent. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a>Contenuto figlio

I componenti possono impostare il contenuto di un altro componente. Il componente di assegnazione fornisce il contenuto tra i tag che specificano il componente di destinazione.

Nell'esempio seguente, il `ChildComponent` ha una proprietà `ChildContent` che rappresenta un `RenderFragment`, che rappresenta un segmento di interfaccia utente di cui eseguire il rendering. Il valore di `ChildContent` è posizionato nel markup del componente in cui deve essere eseguito il rendering del contenuto. Il valore di `ChildContent` viene ricevuto dal componente padre e ne viene eseguito il rendering all'interno del `panel-body` del pannello bootstrap.

*Components/ChildComponent. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> La proprietà che riceve il contenuto `RenderFragment` deve essere denominata `ChildContent` per convenzione.

Il `ParentComponent` seguente può fornire contenuti per il rendering del `ChildComponent` inserendo il contenuto all'interno dei tag `<ChildComponent>`.

*Pages/ParentComponent. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a>Attributo splatting e parametri arbitrari

I componenti possono acquisire ed eseguire il rendering di attributi aggiuntivi oltre ai parametri dichiarati del componente. È possibile acquisire attributi aggiuntivi in un dizionario e quindi *Splatted* su un elemento quando il componente viene sottoposto a rendering usando la direttiva Razor [@attributes](xref:mvc/views/razor#attributes) . Questo scenario è utile quando si definisce un componente che produce un elemento di markup che supporta un'ampia gamma di personalizzazioni. Ad esempio, può essere noioso definire gli attributi separatamente per un `<input>` che supporta molti parametri.

Nell'esempio seguente, il primo elemento `<input>` (`id="useIndividualParams"`) USA parametri dei singoli componenti, mentre il secondo elemento `<input>` (`id="useAttributesDict"`) usa l'attributo splatting:

```cshtml
<input id="useIndividualParams"
       maxlength="@Maxlength"
       placeholder="@Placeholder"
       required="@Required"
       size="@Size" />

<input id="useAttributesDict"
       @attributes="InputAttributes" />

@code {
    [Parameter]
    public string Maxlength { get; set; } = "10";

    [Parameter]
    public string Placeholder { get; set; } = "Input placeholder text";

    [Parameter]
    public string Required { get; set; } = "required";

    [Parameter]
    public string Size { get; set; } = "50";

    [Parameter]
    public Dictionary<string, object> InputAttributes { get; set; } =
        new Dictionary<string, object>()
        {
            { "maxlength", "10" },
            { "placeholder", "Input placeholder text" },
            { "required", "required" },
            { "size", "50" }
        };
}
```

Il tipo del parametro deve implementare `IEnumerable<KeyValuePair<string, object>>` con chiavi di stringa. In questo scenario è inoltre possibile utilizzare `IReadOnlyDictionary<string, object>`.

Gli elementi @no__t 0 sottoposti a rendering usando entrambi gli approcci sono identici:

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">
```

Per accettare attributi arbitrari, definire un parametro component usando l'attributo `[Parameter]` con la proprietà `CaptureUnmatchedValues` impostata su `true`:

```cshtml
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

La proprietà `CaptureUnmatchedValues` su `[Parameter]` consente al parametro di trovare la corrispondenza con tutti gli attributi che non corrispondono ad altri parametri. Un componente può definire un solo parametro con `CaptureUnmatchedValues`. Il tipo di proprietà utilizzato con `CaptureUnmatchedValues` deve essere assegnabile da `Dictionary<string, object>` con chiavi di stringa. in questo scenario sono inoltre disponibili le opzioni `IEnumerable<KeyValuePair<string, object>>` o `IReadOnlyDictionary<string, object>`.

## <a name="data-binding"></a>Associazione dati

L'associazione dati a entrambi i componenti e gli elementi DOM viene eseguita con l'attributo [@bind](xref:mvc/views/razor#bind) . Nell'esempio seguente viene associata una proprietà `CurrentValue` al valore della casella di testo:

```cshtml
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

Quando la casella di testo perde lo stato attivo, viene aggiornato il valore della proprietà.

La casella di testo viene aggiornata nell'interfaccia utente solo quando viene eseguito il rendering del componente, non in risposta alla modifica del valore della proprietà. Poiché i componenti eseguono il rendering dopo l'esecuzione del codice del gestore eventi, gli aggiornamenti delle proprietà vengono in *genere* riflessi nell'interfaccia utente immediatamente dopo l'attivazione di un gestore eventi.

L'uso di `@bind` con la proprietà `CurrentValue` (`<input @bind="CurrentValue" />`) equivale essenzialmente a quanto segue:

```cshtml
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

Quando viene eseguito il rendering del componente, il `value` dell'elemento di input deriva dalla proprietà `CurrentValue`. Quando l'utente digita nella casella di testo e modifica lo stato attivo dell'elemento, viene generato l'evento `onchange` e la proprietà `CurrentValue` viene impostata sul valore modificato. In realtà, la generazione del codice è più complessa perché `@bind` gestisce i casi in cui vengono eseguite le conversioni dei tipi. In teoria, `@bind` associa il valore corrente di un'espressione a un attributo `value` e gestisce le modifiche utilizzando il gestore registrato.

Oltre a gestire gli eventi `onchange` con la sintassi `@bind`, è possibile associare una proprietà o un campo usando altri eventi specificando un attributo [@bind-value](xref:mvc/views/razor#bind) con un parametro `event` ([@bind-value:event](xref:mvc/views/razor#bind)). Nell'esempio seguente viene associata la proprietà `CurrentValue` per l'evento `oninput`:

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

Diversamente da `onchange`, che viene attivato quando l'elemento perde lo stato attivo, `oninput` viene attivato quando viene modificato il valore della casella di testo.

**Valori non analizzabili**

Quando un utente fornisce un valore non analizzabile a un elemento associato a un oggetto DataBound, il valore non analizzabile viene automaticamente ripristinato al valore precedente quando viene attivato l'evento bind.

Si consideri lo scenario seguente:

* Un elemento `<input>` è associato a un tipo `int` con un valore iniziale di `123`:

  ```cshtml
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* L'utente aggiorna il valore dell'elemento a `123.45` nella pagina e modifica lo stato attivo dell'elemento.

Nello scenario precedente, il valore dell'elemento viene ripristinato `123`. Quando il valore `123.45` viene rifiutato a favore del valore originale di `123`, l'utente riconosce che il relativo valore non è stato accettato.

Per impostazione predefinita, l'associazione si applica all'evento `onchange` dell'elemento (`@bind="{PROPERTY OR FIELD}"`). Utilizzare `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` per impostare un evento diverso. Per l'evento `oninput` (`@bind-value:event="oninput"`), la riversione viene eseguita dopo qualsiasi sequenza di tasti che introduce un valore non analizzabile. Quando la destinazione è l'evento `oninput` con un tipo associato a @no__t 1, a un utente viene impedito di digitare un carattere `.`. Un carattere `.` viene immediatamente rimosso, quindi l'utente riceve il feedback immediato che sono consentiti solo numeri interi. Esistono scenari in cui il ripristino del valore nell'evento `oninput` non è ideale, ad esempio quando l'utente deve essere autorizzato a cancellare un valore `<input>` non analizzabile. Le alternative includono:

* Non usare l'evento `oninput`. Usare l'evento `onchange` predefinito (`@bind="{PROPERTY OR FIELD}"`), in cui un valore non valido non viene ripristinato fino a quando l'elemento non perde lo stato attivo.
* Eseguire l'associazione a un tipo nullable, ad esempio `int?` o `string`, e fornire la logica personalizzata per gestire le voci non valide.
* Usare un [componente di convalida del modulo](xref:blazor/forms-validation), ad esempio `InputNumber` o `InputDate`. I componenti di convalida dei moduli includono il supporto predefinito per la gestione di input non validi. Componenti di convalida dei moduli:
  * Consente all'utente di fornire un input non valido e di ricevere errori di convalida nell'@no__t 0 associato.
  * Visualizzare gli errori di convalida nell'interfaccia utente senza interferire con l'utente che immette dati Web Form aggiuntivi.

**Globalizzazione**

i valori `@bind` vengono formattati per la visualizzazione e l'analisi usando le regole delle impostazioni cultura correnti.

È possibile accedere alle impostazioni cultura correnti dalla proprietà <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>.

[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) viene usato per i tipi di campo seguenti (`<input type="{TYPE}" />`):

* `date`
* `number`

I tipi di campo precedenti:

* Vengono visualizzati utilizzando le regole di formattazione appropriate basate su browser.
* Non può contenere testo in formato libero.
* Fornire le caratteristiche di interazione utente in base all'implementazione del browser.

I tipi di campo seguenti hanno requisiti di formattazione specifici e non sono attualmente supportati da Blazer perché non sono supportati da tutti i browser principali:

* `datetime-local`
* `month`
* `week`

`@bind` supporta il parametro `@bind:culture` per fornire un <xref:System.Globalization.CultureInfo?displayProperty=fullName> per l'analisi e la formattazione di un valore. Non è consigliabile specificare impostazioni cultura quando si usano i tipi di campo `date` e `number`. `date` e `number` includono il supporto predefinito di blazer che fornisce le impostazioni cultura richieste.

Per informazioni su come impostare le impostazioni cultura dell'utente, vedere la sezione [localizzazione](#localization) .

**Stringhe di formato**

Il data binding funziona con stringhe di formato <xref:System.DateTime> con [@bind:format](xref:mvc/views/razor#bind). Altre espressioni di formato, ad esempio i formati di valuta o numerici, non sono disponibili in questo momento.

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

Nel codice precedente, il tipo di campo dell'elemento `<input>` (`type`) viene impostato sul valore predefinito `text`. `@bind:format` è supportato per l'associazione dei tipi .NET seguenti:

* <xref:System.DateTime?displayProperty=fullName>
* <xref:System.DateTime?displayProperty=fullName>?
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <xref:System.DateTimeOffset?displayProperty=fullName>?

L'attributo `@bind:format` specifica il formato della data da applicare al `value` dell'elemento `<input>`. Il formato viene usato anche per analizzare il valore quando si verifica un evento `onchange`.

La specifica di un formato per il tipo di campo `date` non è consigliata perché con il supporto incorporato per formattare le date.

**Parametri dei componenti**

Il binding riconosce i parametri del componente, dove `@bind-{property}` può associare un valore della proprietà tra i componenti.

Il componente figlio seguente (`ChildComponent`) ha un parametro del componente `Year` e un callback di `YearChanged`:

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

`EventCallback<T>` è descritto nella sezione [EventCallback](#eventcallback) .

Il componente padre seguente utilizza `ChildComponent` e associa il parametro `ParentYear` dall'elemento padre al parametro `Year` sul componente figlio:

```cshtml
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

Il caricamento del `ParentComponent` produce il markup seguente:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

Se il valore della proprietà `ParentYear` viene modificato selezionando il pulsante nell'`ParentComponent`, viene aggiornata la proprietà `Year` del `ChildComponent`. Viene eseguito il rendering del nuovo valore di `Year` nell'interfaccia utente quando viene eseguito il rendering del `ParentComponent`:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

Il parametro `Year` è associabile perché contiene un evento `YearChanged` complementare che corrisponde al tipo del parametro `Year`.

Per convenzione, `<ChildComponent @bind-Year="ParentYear" />` è essenzialmente equivalente alla scrittura:

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

In generale, una proprietà può essere associata a un gestore eventi corrispondente usando l'attributo `@bind-property:event`. Ad esempio, la proprietà `MyProp` può essere associata a `MyEventHandler` usando i due attributi seguenti:

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a>Gestione di eventi

I componenti Razor forniscono funzionalità di gestione degli eventi. Per un attributo dell'elemento HTML denominato `on{event}` (ad esempio, `onclick` e `onsubmit`) con un valore tipizzato da un delegato, i componenti Razor considera il valore dell'attributo come un gestore eventi. Il nome dell'attributo è sempre formattato [@on {Event}](xref:mvc/views/razor#onevent).

Il codice seguente chiama il metodo `UpdateHeading` quando si seleziona il pulsante nell'interfaccia utente:

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

Il codice seguente chiama il metodo `CheckChanged` quando la casella di controllo viene modificata nell'interfaccia utente:

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

I gestori di eventi possono anche essere asincroni e restituire un <xref:System.Threading.Tasks.Task>. Non è necessario chiamare manualmente `StateHasChanged()`. Le eccezioni vengono registrate quando si verificano.

Nell'esempio seguente `UpdateHeading` viene chiamato in modo asincrono quando si seleziona il pulsante:

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

### <a name="event-argument-types"></a>Tipi di argomenti dell'evento

Per alcuni eventi, i tipi di argomento dell'evento sono consentiti. Se l'accesso a uno di questi tipi di evento non è necessario, non è obbligatorio nella chiamata al metodo.

Nella tabella seguente sono riportati i `EventArgs` supportati.

| event | Class |
| ----- | ----- |
| Appunti        | `ClipboardEventArgs` |
| Trascinare             | `DragEventArgs` &ndash; `DataTransfer` e `DataTransferItem` contengono dati di elementi trascinati. |
| Error            | `ErrorEventArgs` |
| Stato attivo            | `FocusEventArgs` &ndash; non include il supporto per `relatedTarget`. |
| Modifica di`<input>` | `ChangeEventArgs` |
| Tastiera         | `KeyboardEventArgs` |
| Mouse            | `MouseEventArgs` |
| Puntatore del mouse    | `PointerEventArgs` |
| Rotellina del mouse      | `WheelEventArgs` |
| Stato         | `ProgressEventArgs` |
| Tocco            | `TouchEventArgs` &ndash; `TouchPoint` rappresenta un singolo punto di contatto su un dispositivo sensibile al tocco. |

Per informazioni sulle proprietà e sul comportamento di gestione degli eventi degli eventi nella tabella precedente, vedere le [classi EventArgs nell'origine riferimento (ASPNET/AspNetCore Release/3.0 Branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).

### <a name="lambda-expressions"></a>Espressioni lambda

È inoltre possibile utilizzare le espressioni lambda:

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

Spesso è consigliabile chiudere i valori aggiuntivi, ad esempio quando si esegue l'iterazione su un set di elementi. Nell'esempio seguente vengono creati tre pulsanti, ciascuno dei quali chiama `UpdateHeading` passando un argomento di evento (`MouseEventArgs`) e il relativo numero di pulsante (`buttonNumber`) se selezionato nell'interfaccia utente:

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> Non **usare la** variabile loop (`i`) in un ciclo `for` direttamente in un'espressione lambda. In caso contrario, la stessa variabile viene utilizzata da tutte le espressioni lambda che fanno sì che il valore di `i` sia lo stesso in tutte le espressioni lambda. Acquisire sempre il relativo valore in una variabile locale (`buttonNumber` nell'esempio precedente) e quindi usarlo.

### <a name="eventcallback"></a>EventCallback

Uno scenario comune con i componenti annidati è la volontà di eseguire il metodo di un componente padre quando si verifica un evento del componente figlio @ no__t-0per, quando si verifica un evento `onclick` nell'elemento figlio. Per esporre gli eventi tra i componenti, usare un `EventCallback`. Un componente padre può assegnare un metodo di callback a un componente figlio `EventCallback`.

Il `ChildComponent` nell'app di esempio illustra come viene configurato un gestore `onclick` di un pulsante per ricevere un delegato `EventCallback` dal `ParentComponent` dell'esempio. Il `EventCallback` è tipizzato con `MouseEventArgs`, appropriato per un evento di @no__t 2 da un dispositivo periferico:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

Il `ParentComponent` imposta il `EventCallback<T>` del figlio sul relativo metodo `ShowMessage`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

Quando il pulsante è selezionato in `ChildComponent`:

* Viene chiamato il metodo `ShowMessage` `ParentComponent`. `messageText` viene aggiornato e visualizzato nell'`ParentComponent`.
* Una chiamata a `StateHasChanged` non è obbligatoria nel metodo del callback (`ShowMessage`). `StateHasChanged` viene chiamato automaticamente per eseguire il rendering del `ParentComponent`, così come gli eventi figlio attivano il rendering dei componenti nei gestori eventi che vengono eseguiti all'interno dell'elemento figlio.

`EventCallback` e `EventCallback<T>` consentono delegati asincroni. `EventCallback<T>` è fortemente tipizzato e richiede un tipo di argomento specifico. `EventCallback` è debolmente tipizzato e consente qualsiasi tipo di argomento.

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

Richiamare un `EventCallback` o `EventCallback<T>` con `InvokeAsync` e attendere il <xref:System.Threading.Tasks.Task>:

```csharp
await callback.InvokeAsync(arg);
```

Utilizzare `EventCallback` e `EventCallback<T>` per la gestione degli eventi e i parametri del componente di associazione.

Preferisci la `EventCallback<T>` fortemente tipizzata su `EventCallback`. `EventCallback<T>` fornisce un migliore feedback sugli errori agli utenti del componente. Analogamente ad altri gestori di eventi dell'interfaccia utente, la specifica del parametro dell'evento è facoltativa. Utilizzare `EventCallback` quando non viene passato alcun valore al callback.

## <a name="chained-bind"></a>Binding concatenato

Uno scenario comune consiste nel concatenare un parametro associato a dati a un elemento Page nell'output del componente. Questo scenario è denominato *Binding concatenato* perché si verificano simultaneamente più livelli di associazione.

Un binding concatenato non può essere implementato con la sintassi `@bind` nell'elemento della pagina. Il gestore eventi e il valore devono essere specificati separatamente. Un componente padre, tuttavia, può utilizzare la sintassi `@bind` con il parametro del componente.

Il componente `PasswordField` seguente (*PasswordField. Razor*):

* Imposta il valore di un elemento `<input>` su una proprietà `Password`.
* Espone le modifiche della proprietà `Password` a un componente padre con un [EventCallback](#eventcallback).

```cshtml
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool showPassword;

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
        showPassword = !showPassword;
    }
}
```

Il componente `PasswordField` viene usato in un altro componente:

```cshtml
<PasswordField @bind-Password="password" />

@code {
    private string password;
}
```

Per eseguire controlli o intercettare gli errori sulla password nell'esempio precedente:

* Creare un campo sottostante per `Password` (`password` nel codice di esempio seguente).
* Eseguire i controlli o gli errori di trap nel Setter `Password`.

Nell'esempio seguente viene fornito un feedback immediato all'utente se viene utilizzato uno spazio nel valore della password:

```cshtml
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@validationMessage</span>

@code {
    private bool showPassword;
    private string password;
    private string validationMessage;

    [Parameter]
    public string Password
    {
        get { return password ?? string.Empty; }
        set
        {
            if (password != value)
            {
                if (value.Contains(' '))
                {
                    validationMessage = "Spaces not allowed!";
                }
                else
                {
                    password = value;
                    validationMessage = string.Empty;
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
        showPassword = !showPassword;
    }
}
```

## <a name="capture-references-to-components"></a>Acquisisci riferimenti ai componenti

I riferimenti ai componenti forniscono un modo per fare riferimento a un'istanza del componente in modo da poter emettere comandi per tale istanza, ad esempio `Show` o `Reset`. Per acquisire un riferimento a un componente:

* Aggiungere un attributo [@ref](xref:mvc/views/razor#ref) al componente figlio.
* Definire un campo con lo stesso tipo del componente figlio.

```cshtml
<MyLoginDialog @ref="loginDialog" ... />

@code {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

Quando viene eseguito il rendering del componente, il campo `loginDialog` viene popolato con l'istanza del componente figlio `MyLoginDialog`. È quindi possibile richiamare i metodi .NET nell'istanza del componente.

> [!IMPORTANT]
> La variabile `loginDialog` viene popolata solo dopo il rendering del componente e l'output include l'elemento `MyLoginDialog`. Fino a quel momento, non c'è niente a cui fare riferimento. Per modificare i riferimenti ai componenti dopo che il componente ha terminato il rendering, usare i [Metodi OnAfterRenderAsync o OnAfterRender](#lifecycle-methods).

Mentre l'acquisizione di riferimenti ai componenti usa una sintassi simile per l' [acquisizione di riferimenti a elementi](xref:blazor/javascript-interop#capture-references-to-elements), non è una funzionalità di [interoperabilità di JavaScript](xref:blazor/javascript-interop) . I riferimenti ai componenti non vengono passati al codice JavaScript @ no__t-0they're usato solo nel codice .NET.

> [!NOTE]
> **Non** usare i riferimenti ai componenti per mutare lo stato dei componenti figlio. Usare invece i normali parametri dichiarativi per passare i dati ai componenti figlio. L'utilizzo di normali parametri dichiarativi restituisce automaticamente i componenti figlio che eseguono il rendering alle ore corrette.

## <a name="invoke-component-methods-externally-to-update-state"></a>Richiama i metodi del componente esternamente per aggiornare lo stato

Blazer usa un `SynchronizationContext` per applicare un singolo thread logico di esecuzione. I metodi del ciclo di vita di un componente e tutti i callback di evento generati da Blazer vengono eseguiti in questo `SynchronizationContext`. Nel caso in cui un componente deve essere aggiornato in base a un evento esterno, ad esempio un timer o altre notifiche, usare il metodo `InvokeAsync`, che verrà inviato al `SynchronizationContext` di Blazer.

Si consideri ad esempio un *servizio Notifier* che può inviare notifiche a qualsiasi componente in ascolto dello stato aggiornato:

```csharp
public class NotifierService
{
    // Can be called from anywhere
    public async Task Update(string key, int value)
    {
        if (Notify != null)
        {
            await Notify.Invoke(key, value);
        }
    }

    public event Func<string, int, Task> Notify;
}
```

Utilizzo del `NotifierService` per aggiornare un componente:

```cshtml
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @lastNotification.key = @lastNotification.value</p>

@code {
    private (string key, int value) lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

Nell'esempio precedente, `NotifierService` richiama il metodo `OnNotify` del componente all'esterno del `SynchronizationContext` di Blaze. `InvokeAsync` viene usato per passare al contesto corretto e accodare un rendering.

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a>Usare \@key per controllare la conservazione di elementi e componenti

Quando si esegue il rendering di un elenco di elementi o componenti e gli elementi o i componenti successivamente cambiano, l'algoritmo di differenziazione di Blazer deve decidere quali elementi o componenti precedenti possono essere conservati e come eseguire il mapping degli oggetti modello. In genere, questo processo è automatico e può essere ignorato, ma in alcuni casi potrebbe essere necessario controllare il processo.

Si consideri l'esempio seguente:

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

Il contenuto della raccolta `People` può cambiare con le voci inserite, eliminate o riordinate. Quando viene eseguito il rendering del componente, il componente `<DetailsEditor>` può cambiare in modo da ricevere valori dei parametri `Details` diversi. Questo può causare un rirendering più complesso del previsto. In alcuni casi, il rirendering può comportare differenze di comportamento visibili, ad esempio lo stato attivo degli elementi persi.

È possibile controllare il processo di mapping con l'attributo della direttiva `@key`. `@key` induce l'algoritmo diff a garantire la conservazione di elementi o componenti in base al valore della chiave:

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="person" Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

Quando la raccolta `People` viene modificata, l'algoritmo diffing mantiene l'associazione tra le istanze di `<DetailsEditor>` e le istanze di `person`:

* Se un `Person` viene eliminato dall'elenco `People`, solo l'istanza corrispondente di `<DetailsEditor>` viene rimossa dall'interfaccia utente. Altre istanze rimangono invariate.
* Se un `Person` viene inserito in una determinata posizione nell'elenco, viene inserita una nuova istanza di `<DetailsEditor>` nella posizione corrispondente. Altre istanze rimangono invariate.
* Se le voci `Person` vengono riordinate, le istanze corrispondenti `<DetailsEditor>` vengono mantenute e riordinate nell'interfaccia utente.

In alcuni scenari, l'utilizzo di `@key` riduce al minimo la complessità del rendering ed evita potenziali problemi con le parti con stato del DOM che cambiano, ad esempio la posizione di messa a fuoco.

> [!IMPORTANT]
> Le chiavi sono locali per ogni elemento contenitore o componente. Le chiavi non vengono confrontate globalmente nell'intero documento.

### <a name="when-to-use-key"></a>Quando usare \@key

In genere, è opportuno usare `@key` ogni volta che viene eseguito il rendering di un elenco (ad esempio, in un blocco `@foreach`) e un valore appropriato per definire il `@key`.

È anche possibile usare `@key` per impedire a blazer di mantenere un sottoalbero di elementi o componenti quando un oggetto viene modificato:

```cshtml
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

Se `@currentPerson` cambia, la direttiva degli attributi `@key` impone a blazer di rimuovere l'intero `<div>` e i relativi discendenti e ricompilare il sottoalbero all'interno dell'interfaccia utente con nuovi elementi e componenti. Questa operazione può essere utile se è necessario garantire che non venga mantenuto alcuno stato dell'interfaccia utente quando viene modificato `@currentPerson`.

### <a name="when-not-to-use-key"></a>Quando non usare \@key

Si verifica un costo in termini di prestazioni in caso di differenze con `@key`. Il costo delle prestazioni non è elevato, ma è sufficiente specificare `@key` se il controllo delle regole di conservazione degli elementi o dei componenti avvantaggia l'app.

Anche se non si usa `@key`, il più possibile le istanze di elementi figlio e componenti. L'unico vantaggio dell'utilizzo di `@key` è il controllo sulla *modalità* di mapping delle istanze del modello alle istanze dei componenti conservati, anziché sull'algoritmo di diffing che seleziona il mapping.

### <a name="what-values-to-use-for-key"></a>Valori da usare per \@key

In genere, è opportuno fornire uno dei seguenti tipi di valore per `@key`:

* Istanze di oggetti modello (ad esempio, un'istanza `Person` come nell'esempio precedente). In questo modo si garantisce la conservazione in base all'uguaglianza del riferimento all'oggetto.
* Identificatori univoci (ad esempio, valori di chiave primaria di tipo `int`, `string` o `Guid`).

Assicurarsi che i valori usati per `@key` non siano in conflitto. Se vengono rilevati valori in conflitto all'interno dello stesso elemento padre, blazer genera un'eccezione perché non è in grado di eseguire il mapping deterministico di elementi o componenti precedenti a nuovi elementi o componenti. Utilizzare solo valori distinti, ad esempio istanze di oggetti o valori di chiave primaria.

## <a name="lifecycle-methods"></a>Metodi del ciclo di vita

`OnInitializedAsync` e `OnInitialized` eseguono il codice per inizializzare il componente. Per eseguire un'operazione asincrona, usare `OnInitializedAsync` e la parola chiave `await` nell'operazione:

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

> [!NOTE]
> Il lavoro asincrono durante l'inizializzazione dei componenti deve verificarsi durante l'evento del ciclo di vita `OnInitializedAsync`.

Per un'operazione sincrona, usare `OnInitialized`:

```csharp
protected override void OnInitialized()
{
    ...
}
```

`OnParametersSetAsync` e `OnParametersSet` vengono chiamati quando un componente riceve parametri dal padre e i valori vengono assegnati alle proprietà. Questi metodi vengono eseguiti dopo l'inizializzazione del componente e ogni volta che viene eseguito il rendering del componente:

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> Il lavoro asincrono quando si applicano i parametri e i valori delle proprietà deve verificarsi durante l'evento del ciclo di vita `OnParametersSetAsync`.

```csharp
protected override void OnParametersSet()
{
    ...
}
```

`OnAfterRenderAsync` e `OnAfterRender` vengono chiamati dopo che un componente ha terminato il rendering. I riferimenti a elementi e componenti vengono popolati a questo punto. Usare questa fase per eseguire passaggi di inizializzazione aggiuntivi usando il contenuto sottoposto a rendering, ad esempio l'attivazione di librerie JavaScript di terze parti che operano sugli elementi DOM sottoposti a rendering.

`OnAfterRender` *non viene chiamato quando si esegue il prerendering sul server.*

Il parametro `firstRender` per `OnAfterRenderAsync` e `OnAfterRender` è:

* Impostare su `true` la prima volta che l'istanza del componente viene richiamata.
* Garantisce che il lavoro di inizializzazione venga eseguito una sola volta.

```csharp
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await ...
    }
}
```

> [!NOTE]
> Il lavoro asincrono immediatamente dopo il rendering deve verificarsi durante l'evento del ciclo di vita `OnAfterRenderAsync`.

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

### <a name="handle-incomplete-async-actions-at-render"></a>Gestisci azioni asincrone incomplete durante il rendering

Le azioni asincrone eseguite negli eventi del ciclo di vita potrebbero non essere state completate prima del rendering del componente. Gli oggetti potrebbero essere `null` o compilati in modo incompleto con i dati mentre è in esecuzione il metodo del ciclo di vita. Fornire la logica di rendering per confermare che gli oggetti vengono inizializzati. Esegue il rendering degli elementi dell'interfaccia utente segnaposto (ad esempio, un messaggio di caricamento) mentre gli oggetti sono `null`.

Nel componente `FetchData` dei modelli di Blazer, viene eseguito l'override di `OnInitializedAsync` in asincrono ricevere i dati delle previsioni (`forecasts`). Quando `forecasts` è `null`, all'utente viene visualizzato un messaggio di caricamento. Al termine dell'`Task` restituito da `OnInitializedAsync`, il componente viene sottoposta a rendering con lo stato aggiornato.

*Pages/FetchData.razor*:

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a>Esegui codice prima dell'impostazione dei parametri

è possibile eseguire l'override di `SetParameters` per eseguire il codice prima di impostare i parametri:

```csharp
public override void SetParameters(ParameterView parameters)
{
    ...

    base.SetParameters(parameters);
}
```

Se `base.SetParameters` non viene richiamato, il codice personalizzato può interpretare il valore dei parametri in ingresso in qualsiasi modo necessario. I parametri in ingresso, ad esempio, non devono essere assegnati alle proprietà della classe.

### <a name="suppress-refreshing-of-the-ui"></a>Non aggiornare l'interfaccia utente

è possibile eseguire l'override di `ShouldRender` per non aggiornare l'interfaccia utente. Se l'implementazione restituisce `true`, l'interfaccia utente viene aggiornata. Anche se `ShouldRender` viene sottoposto a override, il componente viene sempre sottoposto a rendering iniziale.

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a>Eliminazione di componenti con IDisposable

Se un componente implementa <xref:System.IDisposable>, il [metodo Dispose](/dotnet/standard/garbage-collection/implementing-dispose) viene chiamato quando il componente viene rimosso dall'interfaccia utente. Il componente seguente usa `@implements IDisposable` e il metodo `Dispose`:

```csharp
@using System
@implements IDisposable

...

@code {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a>Routing

Il routing in blazer viene effettuato fornendo un modello di route a ogni componente accessibile nell'app.

Quando viene compilato un file Razor con una direttiva `@page`, alla classe generata viene assegnato un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> che specifica il modello di route. In fase di esecuzione, il router cerca le classi di componenti con un `RouteAttribute` ed esegue il rendering di qualsiasi componente con un modello di route corrispondente all'URL richiesto.

È possibile applicare più modelli di route a un componente. Il componente seguente risponde alle richieste per `/BlazorRoute` e `/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a>Parametri di route

I componenti possono ricevere parametri di route dal modello di route fornito nella direttiva `@page`. Il router usa parametri di route per popolare i parametri del componente corrispondente.

*Componente parametro di route*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

I parametri facoltativi non sono supportati, quindi vengono applicate due direttive `@page` nell'esempio precedente. Il primo consente la navigazione al componente senza un parametro. La seconda direttiva `@page` accetta il parametro di route `{text}` e assegna il valore alla proprietà `Text`.

## <a name="base-class-inheritance-for-a-code-behind-experience"></a>Ereditarietà della classe base per un'esperienza di "code-behind"

I file dei componenti combinano C# il markup HTML e il codice di elaborazione nello stesso file. La direttiva `@inherits` può essere usata per fornire alle app Blazer un'esperienza di "code-behind" che separa il markup dei componenti dal codice di elaborazione.

L' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) Mostra come un componente può ereditare una classe base, `BlazorRocksBase`, per fornire le proprietà e i metodi del componente.

*Pages/BlazorRocks. Razor*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

*BlazorRocksBase.cs*:

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

La classe base deve derivare da `ComponentBase`.

## <a name="import-components"></a>Importa componenti

Lo spazio dei nomi di un componente creato con Razor si basa su (in ordine di priorità):

* designazione [@namespace](xref:mvc/views/razor#namespace) nel markup del file Razor (Razor) (@no__t-*3).*
* @No__t-0 del progetto nel file di progetto (`<RootNamespace>BlazorSample</RootNamespace>`).
* Il nome del progetto, tratto dal nome file del file di progetto (con*estensione csproj*), e il percorso dalla radice del progetto al componente. Ad esempio, il Framework risolve *{Project root}/pages/index.Razor* (*BlazorSample. csproj*) nello spazio dei nomi `BlazorSample.Pages`. I componenti C# seguono le regole di associazione dei nomi. Per il componente `Index` in questo esempio, i componenti nell'ambito sono tutti i componenti:
  * Nella stessa cartella, *pagine*.
  * Componenti nella radice del progetto che non specificano in modo esplicito uno spazio dei nomi diverso.

I componenti definiti in uno spazio dei nomi diverso vengono introdotti nell'ambito usando la direttiva [@using](xref:mvc/views/razor#using) Razor.

Se un altro componente, `NavMenu.razor`, è presente nella cartella *BlazorSample/Shared/* , il componente può essere usato in `Index.razor` con l'istruzione `@using` seguente:

```cshtml
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

È anche possibile fare riferimento ai componenti usando i relativi nomi completi, che non richiedono la direttiva [@using](xref:mvc/views/razor#using) :

```cshtml
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> La qualificazione `global::` non è supportata.
>
> L'importazione di componenti con istruzioni `using` con alias, ad esempio `@using Foo = Bar`, non è supportata.
>
> I nomi parzialmente qualificati non sono supportati. Ad esempio, l'aggiunta di `@using BlazorSample` e il riferimento `NavMenu.razor` con `<Shared.NavMenu></Shared.NavMenu>` non è supportata.

## <a name="conditional-html-element-attributes"></a>Attributi dell'elemento HTML condizionale

Gli attributi degli elementi HTML vengono sottoposti a rendering in modo condizionale in base al valore .NET. Se il valore è `false` o `null`, non viene eseguito il rendering dell'attributo. Se il valore è `true`, l'attributo viene sottoposto a rendering ridotto a icona.

Nell'esempio seguente `IsCompleted` determina se viene eseguito il rendering di `checked` nel markup dell'elemento:

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

Se `IsCompleted` è `true`, viene eseguito il rendering della casella di controllo come segue:

```html
<input type="checkbox" checked />
```

Se `IsCompleted` è `false`, viene eseguito il rendering della casella di controllo come segue:

```html
<input type="checkbox" />
```

Per ulteriori informazioni, vedere <xref:mvc/views/razor>.

> [!WARNING]
> Alcuni attributi HTML, ad esempio [aria-premuti](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), non funzionano correttamente quando il tipo .NET è un `bool`. In questi casi, usare un tipo `string` anziché un `bool`.

## <a name="raw-html"></a>HTML non elaborato

Le stringhe vengono in genere sottoposte a rendering usando i nodi di testo DOM, il che significa che qualsiasi markup che può contenere viene ignorato e considerato come testo letterale. Per eseguire il rendering del codice HTML non elaborato, eseguire il wrapping del contenuto HTML in un valore `MarkupString`. Il valore viene analizzato in formato HTML o SVG e inserito nel DOM.

> [!WARNING]
> Il rendering di codice HTML non elaborato costruito da un'origine non attendibile costituisce un rischio per la **sicurezza** e deve essere evitato.

Nell'esempio seguente viene illustrato l'utilizzo del tipo `MarkupString` per aggiungere un blocco di contenuto HTML statico all'output sottoposto a rendering di un componente:

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a>Componenti basati su modelli

I componenti basati su modelli sono componenti che accettano uno o più modelli di interfaccia utente come parametri, che possono quindi essere usati come parte della logica di rendering del componente. I componenti basati su modelli consentono di creare componenti di livello superiore più riutilizzabili rispetto ai componenti normali. Di seguito sono riportati alcuni esempi:

* Componente della tabella che consente a un utente di specificare i modelli per l'intestazione, le righe e il piè di pagina della tabella.
* Componente di elenco che consente a un utente di specificare un modello per il rendering degli elementi in un elenco.

### <a name="template-parameters"></a>Parametri di modelli

Un componente basato su modelli viene definito specificando uno o più parametri del componente di tipo `RenderFragment` o `RenderFragment<T>`. Un frammento di rendering rappresenta un segmento di interfaccia utente di cui eseguire il rendering. `RenderFragment<T>` accetta un parametro di tipo che può essere specificato quando viene richiamato il frammento di rendering.

componente `TableTemplate`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

Quando si usa un componente basato su modelli, è possibile specificare i parametri del modello usando gli elementi figlio che corrispondono ai nomi dei parametri (`TableHeader` e `RowTemplate` nell'esempio seguente):

```cshtml
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@context.PetId</td>
        <td>@context.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="template-context-parameters"></a>Parametri di contesto del modello

Gli argomenti del componente di tipo `RenderFragment<T>` passati come elementi hanno un parametro implicito denominato `context` (ad esempio, nell'esempio di codice precedente, `@context.PetId`), ma è possibile modificare il nome del parametro usando l'attributo `Context` sull'elemento figlio. Nell'esempio seguente, l'attributo `Context` dell'elemento `RowTemplate` specifica il parametro `pet`:

```cshtml
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate Context="pet">
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

In alternativa, è possibile specificare l'attributo `Context` sull'elemento Component. L'attributo `Context` specificato si applica a tutti i parametri di modello specificati. Questa operazione può essere utile quando si desidera specificare il nome del parametro di contenuto per il contenuto figlio implicito (senza alcun elemento figlio di wrapping). Nell'esempio seguente l'attributo `Context` viene visualizzato nell'elemento `TableTemplate` e si applica a tutti i parametri del modello:

```cshtml
<TableTemplate Items="pets" Context="pet">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="generic-typed-components"></a>Componenti tipizzati in modo generico

I componenti basati su modelli spesso sono tipizzati in modo generico. Ad esempio, è possibile usare un componente `ListViewTemplate` generico per eseguire il rendering di valori `IEnumerable<T>`. Per definire un componente generico, usare la direttiva [@typeparam](xref:mvc/views/razor#typeparam) per specificare i parametri di tipo:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

Quando si usano componenti tipizzati generici, il parametro di tipo viene dedotto, se possibile:

```cshtml
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

In caso contrario, il parametro di tipo deve essere specificato in modo esplicito utilizzando un attributo che corrisponde al nome del parametro di tipo. Nell'esempio seguente `TItem="Pet"` specifica il tipo:

```cshtml
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a>Parametri e valori di propagazione

In alcuni scenari, non è pratico eseguire il flusso dei dati da un componente predecessore a un componente discendente usando i [parametri del componente](#component-parameters), soprattutto quando sono presenti diversi livelli di componenti. I valori e i parametri di propagazione consentono di risolvere questo problema fornendo un modo pratico per un componente predecessore per fornire un valore a tutti i relativi componenti discendenti. I parametri e i valori di propagazione forniscono anche un approccio per la coordinazione dei componenti.

### <a name="theme-example"></a>Esempio di tema

Nell'esempio seguente dall'app di esempio, la classe `ThemeInfo` specifica le informazioni sul tema per scorrere la gerarchia dei componenti in modo che tutti i pulsanti all'interno di una determinata parte dell'app condividono lo stesso stile.

*UIThemeClasses/themeinfo. cs*:

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

Un componente predecessore può fornire un valore di propagazione utilizzando il componente valore di propagazione. Il componente `CascadingValue` esegue il wrapping di un sottoalbero della gerarchia dei componenti e fornisce un singolo valore a tutti i componenti all'interno di tale sottoalbero.

Ad esempio, l'app di esempio specifica le informazioni sul tema (`ThemeInfo`) in uno dei layout dell'app come parametro di propagazione per tutti i componenti che costituiscono il corpo del layout della proprietà `@Body`. a `ButtonClass` viene assegnato il valore `btn-success` nel componente layout. Qualsiasi componente discendente può utilizzare questa proprietà tramite l'oggetto a cascata `ThemeInfo`.

componente `CascadingValuesParametersLayout`:

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

Per utilizzare i valori di propagazione, i componenti dichiarano parametri di propagazione utilizzando l'attributo `[CascadingParameter]`. I valori a cascata vengono associati ai parametri di propagazione per tipo.

Nell'app di esempio, il componente `CascadingValuesParametersTheme` associa il valore a cascata `ThemeInfo` a un parametro di propagazione. Il parametro viene usato per impostare la classe CSS per uno dei pulsanti visualizzati dal componente.

componente `CascadingValuesParametersTheme`:

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" @onclick="IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" @onclick="IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@code {
    private int currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

Per eseguire la propagazione di più valori dello stesso tipo all'interno dello stesso sottoalbero, specificare una stringa `Name` univoca per ogni componente `CascadingValue` e il corrispondente `CascadingParameter`. Nell'esempio seguente due componenti `CascadingValue` propagano istanze diverse di `MyCascadingType` per nome:

```cshtml
<CascadingValue Value=@ParentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType ParentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

In un componente discendente i parametri a cascata ricevono i rispettivi valori dai valori a cascata corrispondenti nel componente predecessore per nome:

```cshtml
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a>Esempio di tabulazione

I parametri di propagazione consentono inoltre ai componenti di collaborare attraverso la gerarchia dei componenti. Si consideri, ad esempio, l'esempio di *tabulazione* seguente nell'app di esempio.

L'app di esempio dispone di un'interfaccia `ITab` che le schede implementano:

[!code-csharp[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

Il componente `CascadingValuesParametersTabSet` usa il componente `TabSet`, che contiene diversi componenti `Tab`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

I componenti figlio `Tab` non vengono passati in modo esplicito come parametri al `TabSet`. Al contrario, i componenti figlio `Tab` fanno parte del contenuto figlio della `TabSet`. Tuttavia, il `TabSet` deve comunque conoscere ogni componente `Tab`, in modo che sia in grado di eseguire il rendering delle intestazioni e della scheda attiva. Per abilitare questo coordinamento senza richiedere codice aggiuntivo, il componente `TabSet` *può fornire se stesso come valore* di propagazione che viene quindi prelevato dai componenti `Tab` discendenti.

componente `TabSet`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

I componenti discendenti `Tab` acquisiscono la classe che contiene `TabSet` come parametro di propagazione, quindi i componenti `Tab` si aggiungono al `TabSet` e coordinano la scheda attiva.

componente `Tab`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a>Modelli Razor

I frammenti di rendering possono essere definiti usando la sintassi del modello Razor. I modelli Razor sono un modo per definire un frammento di interfaccia utente e presupporre il formato seguente:

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

Nell'esempio seguente viene illustrato come specificare i valori `RenderFragment` e `RenderFragment<T>` ed eseguire il rendering dei modelli direttamente in un componente. I frammenti di rendering possono anche essere passati come argomenti ai [componenti basati su modelli](#templated-components).

```cshtml
@timeTemplate

@petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> petTemplate = 
        (pet) => @<p>Your pet's name is @pet.Name.</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

Output del codice precedente sottoposto a rendering:

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a>Logica RenderTreeBuilder manuale

`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` fornisce metodi per la modifica di componenti ed elementi, inclusa la compilazione manuale C# dei componenti nel codice.

> [!NOTE]
> L'uso di `RenderTreeBuilder` per creare componenti è uno scenario avanzato. Un componente con formato non valido, ad esempio un tag di markup non chiuso, può causare un comportamento indefinito.

Si consideri il componente `PetDetails` seguente, che può essere incorporato manualmente in un altro componente:

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

Nell'esempio seguente il ciclo nel metodo `CreateComponent` genera tre componenti `PetDetails`. Quando si chiamano i metodi `RenderTreeBuilder` per creare i componenti (`OpenComponent` e `AddAttribute`), i numeri di sequenza sono numeri di riga del codice sorgente. L'algoritmo di differenza Blaze si basa sui numeri di sequenza corrispondenti a righe di codice distinte, non sulle chiamate di chiamata distinti. Quando si crea un componente con i metodi `RenderTreeBuilder`, impostare come hardcoded gli argomenti per i numeri di sequenza. **L'utilizzo di un calcolo o di un contatore per generare il numero di sequenza può causare un calo delle prestazioni.** Per ulteriori informazioni, vedere la sezione [numeri di sequenza correlati ai numeri di riga del codice e non all'ordine di esecuzione](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .

componente `BuiltContent`:

```cshtml
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

> ! AVVISO I tipi in `Microsoft.AspNetCore.Components.RenderTree` consentono l'elaborazione dei *risultati* delle operazioni di rendering. Questi sono i dettagli interni dell'implementazione del Framework blazer. Questi tipi devono essere considerati *instabili* e soggetti a modifiche nelle versioni future.

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a>I numeri di sequenza sono correlati ai numeri di riga del codice e non all'ordine di esecuzione

I file `.razor` Blazer vengono sempre compilati. Questo è potenzialmente un ottimo vantaggio per `.razor` perché il passaggio di compilazione può essere usato per inserire informazioni che migliorano le prestazioni dell'app in fase di esecuzione.

Un esempio fondamentale di questi miglioramenti riguarda i *numeri di sequenza*. I numeri di sequenza indicano al runtime quali output provengono da righe di codice distinte e ordinate. Il runtime usa queste informazioni per generare differenze di albero efficienti nel tempo lineare, che è molto più veloce rispetto a quanto normalmente è possibile per un algoritmo diff della struttura ad albero generale.

Si consideri il seguente semplice file `.razor`:

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

Il codice precedente viene compilato in un modo simile al seguente:

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

Quando il codice viene eseguito per la prima volta, se `someFlag` è `true`, il generatore riceve:

| Sequence | Digitare      | Dati   |
| :------: | --------- | :----: |
| 0        | Nodo testo | First  |
| 1        | Nodo testo | Second |

Si supponga che `someFlag` diventi `false` e che venga eseguito nuovamente il rendering del markup. Questa volta, il generatore riceve:

| Sequence | Digitare       | Dati   |
| :------: | ---------- | :----: |
| 1        | Nodo testo  | Second |

Quando il runtime esegue una diff, rileva che l'elemento in sequenza `0` è stato rimosso, quindi genera lo *script di modifica*semplice seguente:

* Rimuovere il primo nodo di testo.

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a>Cosa accade se si generano numeri di sequenza a livello di codice

Si supponga invece che sia stata scritta la logica del generatore di albero di rendering seguente:

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

A questo punto, il primo output è:

| Sequence | Digitare      | Dati   |
| :------: | --------- | :----: |
| 0        | Nodo testo | First  |
| 1        | Nodo testo | Second |

Questo risultato è identico al caso precedente, pertanto non esistono problemi negativi. `someFlag` è `false` nel secondo rendering e l'output è:

| Sequence | Digitare      | Dati   |
| :------: | --------- | ------ |
| 0        | Nodo testo | Second |

Questa volta, l'algoritmo Diff rileva che si sono verificate *due* modifiche e l'algoritmo genera lo script di modifica seguente:

* Modificare il valore del primo nodo di testo in `Second`.
* Rimuovere il secondo nodo di testo.

La generazione dei numeri di sequenza ha perso tutte le informazioni utili sul punto in cui i rami e i cicli `if/else` erano presenti nel codice originale. In questo modo si ottiene una differenza **due volte più a lungo** .

Questo è un esempio semplice. Nei casi più realistici con strutture complesse e profondamente annidate e soprattutto con i cicli, il costo delle prestazioni è più grave. Invece di identificare immediatamente i blocchi o i rami del ciclo che sono stati inseriti o rimossi, l'algoritmo Diff deve ripresentarsi in modo approfondito negli alberi di rendering e, in genere, compilare script di modifica molto più lunghi perché non è stato informato in modo indesiderato sul modo in cui le nuove strutture relazione tra loro.

#### <a name="guidance-and-conclusions"></a>Linee guida e conclusioni

* Le prestazioni dell'app soffrono se i numeri di sequenza vengono generati dinamicamente.
* Il Framework non è in grado di creare automaticamente i propri numeri di sequenza in fase di esecuzione perché le informazioni necessarie non esistono a meno che non vengano acquisite in fase di compilazione.
* Non scrivere blocchi lunghi per la logica `RenderTreeBuilder` implementata manualmente. Preferire i file `.razor` e consentire al compilatore di gestire i numeri di sequenza. Se non si è in grado di evitare la logica manuale `RenderTreeBuilder`, suddividere i blocchi di codice lunghi in parti più piccole racchiuse tra `OpenRegion` @ no__t-2 @ no__t-3 chiamate. Ogni area ha il proprio spazio separato dei numeri di sequenza, quindi è possibile riavviare da zero (o qualsiasi altro numero arbitrario) all'interno di ogni area.
* Se i numeri di sequenza sono hardcoded, l'algoritmo Diff richiede solo che i numeri di sequenza aumentino nel valore. Il valore iniziale e i gap sono irrilevanti. Una delle opzioni legittime consiste nell'usare il numero di riga del codice come numero di sequenza oppure iniziare da zero e aumentare di uno o di centinaia (o qualsiasi intervallo preferito). 
* Blazer usa i numeri di sequenza, mentre altri Framework dell'interfaccia utente con differenze tra gli alberi non li usano. La diffing è molto più veloce quando si usano i numeri di sequenza e Blazer ha il vantaggio di un passaggio di compilazione che riguarda automaticamente i numeri di sequenza per gli sviluppatori che creano file `.razor`.

## <a name="localization"></a>Localizzazione

Le app del server Blazer vengono localizzate usando il [middleware di localizzazione](xref:fundamentals/localization#localization-middleware). Il middleware seleziona le impostazioni cultura appropriate per gli utenti che richiedono risorse dall'app.

Le impostazioni cultura possono essere impostate utilizzando uno degli approcci seguenti:

* [Cookie](#cookies)
* [Fornire l'interfaccia utente per scegliere le impostazioni cultura](#provide-ui-to-choose-the-culture)

Per altre informazioni ed esempi, vedere <xref:fundamentals/localization>.

### <a name="cookies"></a>Cookie

Un cookie di impostazioni cultura di localizzazione può salvare in maniera permanente le impostazioni cultura dell'utente. Il cookie viene creato dal metodo `OnGet` della pagina host dell'app (*pages/host. cshtml. cs*). Il middleware di localizzazione legge il cookie nelle richieste successive per impostare le impostazioni cultura dell'utente. 

L'uso di un cookie garantisce che la connessione WebSocket possa propagare correttamente le impostazioni cultura. Se gli schemi di localizzazione sono basati sul percorso dell'URL o sulla stringa di query, lo schema potrebbe non essere in grado di funzionare con WebSocket, quindi non è possibile salvare in modo permanente le impostazioni cultura. Pertanto, l'utilizzo di un cookie di impostazioni cultura di localizzazione è l'approccio consigliato.

Qualsiasi tecnica può essere utilizzata per assegnare impostazioni cultura se le impostazioni cultura vengono rese permanente in un cookie di localizzazione. Se l'app dispone già di uno schema di localizzazione stabilito per ASP.NET Core lato server, continuare a usare l'infrastruttura di localizzazione esistente dell'app e impostare il cookie delle impostazioni cultura di localizzazione nello schema dell'app.

Nell'esempio seguente viene illustrato come impostare le impostazioni cultura correnti in un cookie che può essere letto dal middleware di localizzazione. Creare un file *pages/host. cshtml. cs* con il contenuto seguente nell'app del server Blazer:

```csharp
public class HostModel : PageModel
{
    public void OnGet()
    {
        HttpContext.Response.Cookies.Append(
            CookieRequestCultureProvider.DefaultCookieName,
            CookieRequestCultureProvider.MakeCookieValue(
                new RequestCulture(
                    CultureInfo.CurrentCulture,
                    CultureInfo.CurrentUICulture)));
    }
}
```

La localizzazione viene gestita nell'app:

1. Il browser invia una richiesta HTTP iniziale all'app.
1. Le impostazioni cultura vengono assegnate dal middleware di localizzazione.
1. Il metodo `OnGet` in *_Host. cshtml. cs* rende permanente le impostazioni cultura di un cookie come parte della risposta.
1. Il browser apre una connessione WebSocket per creare una sessione del server Interactive Blaze.
1. Il middleware di localizzazione legge il cookie e assegna le impostazioni cultura.
1. La sessione del server Blaze inizia con le impostazioni cultura corrette.

## <a name="provide-ui-to-choose-the-culture"></a>Fornire l'interfaccia utente per scegliere le impostazioni cultura

Per consentire a un utente di selezionare le impostazioni cultura, è consigliabile un *approccio basato su Reindirizzamento* . Il processo è simile a quello che accade in un'app Web quando un utente tenta di accedere a una risorsa protetta @ no__t-0The utente viene reindirizzato a una pagina di accesso e quindi reindirizzato di nuovo alla risorsa originale. 

L'app rende permanente le impostazioni cultura selezionate dall'utente tramite un reindirizzamento a un controller. Il controller imposta le impostazioni cultura selezionate dall'utente in un cookie e reindirizza di nuovo l'utente all'URI originale.

Stabilire un endpoint HTTP nel server per impostare le impostazioni cultura selezionate dall'utente in un cookie ed eseguire di nuovo il reindirizzamento all'URI originale:

```csharp
[Route("[controller]/[action]")]
public class CultureController : Controller
{
    public IActionResult SetCulture(string culture, string redirectUri)
    {
        if (culture != null)
        {
            HttpContext.Response.Cookies.Append(
                CookieRequestCultureProvider.DefaultCookieName,
                CookieRequestCultureProvider.MakeCookieValue(
                    new RequestCulture(culture)));
        }

        return LocalRedirect(redirectUri);
    }
}
```

> [!WARNING]
> Usare il risultato dell'azione `LocalRedirect` per impedire gli attacchi di reindirizzamento aperti. Per ulteriori informazioni, vedere <xref:security/preventing-open-redirects>.

Il componente seguente mostra un esempio di come eseguire il reindirizzamento iniziale quando l'utente seleziona le impostazioni cultura:

```cshtml
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private double textNumber;

    private void OnSelected(ChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(NavigationManager.Uri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        NavigationManager.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

### <a name="use-net-localization-scenarios-in-blazor-apps"></a>Usare scenari di localizzazione .NET in app Blazer

All'interno delle app blazer sono disponibili i seguenti scenari di globalizzazione e localizzazione di .NET:

* . Sistema di risorse di NET
* Formattazione di numeri e date specifiche delle impostazioni cultura

La funzionalità `@bind` di Blazer esegue la globalizzazione in base alle impostazioni cultura correnti dell'utente. Per ulteriori informazioni, vedere la sezione [Data Binding](#data-binding) .

Sono attualmente supportati un set limitato di scenari di localizzazione di ASP.NET Core:

* `IStringLocalizer<>` *è supportato* nelle app blazer.
* `IHtmlLocalizer<>`, `IViewLocalizer<>` e la localizzazione delle annotazioni dei dati è ASP.NET Core scenari MVC e **non è supportata** nelle app blazer.

Per ulteriori informazioni, vedere <xref:fundamentals/localization>.

## <a name="scalable-vector-graphics-svg-images"></a>Immagini SVG (Scalable Vector Graphics)

Poiché Blazer esegue il rendering di HTML, le immagini supportate dal browser, incluse le immagini SVG (Scalable Vector*Graphics),* sono supportate tramite il tag `<img>`:

```html
<img alt="Example image" src="some-image.svg" />
```

Analogamente, le immagini SVG sono supportate nelle regole CSS di un file di foglio di stile (*CSS*):

```css
.my-element {
    background-image: url("some-image.svg");
}
```

Tuttavia, il markup SVG inline non è supportato in tutti gli scenari. Se si inserisce un tag `<svg>` direttamente in un file di componente (*Razor*), il rendering delle immagini di base è supportato, ma molti scenari avanzati non sono ancora supportati. Ad esempio, i tag `<use>` non sono attualmente rispettati e `@bind` non può essere usato con alcuni tag SVG. Queste limitazioni verranno affrontate in una versione futura.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:security/blazor/server> &ndash; include informazioni aggiuntive sulla creazione di app Server blazer che devono essere confrontate con l'esaurimento delle risorse.
