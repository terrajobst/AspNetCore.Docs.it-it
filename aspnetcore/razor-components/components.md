---
title: Creare e usare componenti Razor
author: guardrex
description: Informazioni su come creare e usare componenti Razor, incluse le procedure eseguire l'associazione ai dati, gestire gli eventi e gestire i cicli di vita del componente.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: razor-components/components
ms.openlocfilehash: f8ac7f3844b94a162e8d1c45f80ae153d89536ee
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515563"
---
# <a name="create-and-use-razor-components"></a>Creare e usare componenti Razor

Dal [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), e [Morné Zaayman](https://github.com/MorneZaayman)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([procedura per il download](xref:index#how-to-download-a-sample)). Vedere l'argomento [Introduzione a Razor Components](xref:razor-components/get-started) per i prerequisiti.

Le app di componenti Razor vengono compilate usando *componenti*. Un componente è un blocco autonomo dell'interfaccia utente (UI), ad esempio una pagina, finestra di dialogo o form. Un componente include il markup HTML e la logica di elaborazione necessaria per inserire i dati o per rispondere a eventi di interfaccia utente. I componenti sono flessibili e leggero. Possono essere annidati, riutilizzati e condivise tra progetti.

## <a name="component-classes"></a>Classi Component

I componenti sono in genere implementati nei file Razor componente (*.razor*) usando una combinazione di C# e il markup HTML (*cshtml* file vengono usati nelle App Blazor).

I componenti possono essere creati nelle App Razor componenti usando il *. cshtml* estensione di file, purché i file vengono identificati come i file Razor componente utilizzando il `_RazorComponentInclude` proprietà MSBuild. Ad esempio, un'app creata usando il modello del componente Razor specifica che tutti i *. cshtml* file sotto il *componenti* cartella deve essere trattata come file dei componenti di Razor:

```xml
<_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
```

L'interfaccia utente per un componente viene definito utilizzando il codice HTML. La logica di rendering dinamico (ad esempio, cicli, condizioni, espressioni) viene aggiunta usando una sintassi C# incorporata chiamata [Razor](xref:mvc/views/razor). Quando viene compilata un'app Razor componenti, il markup HTML e C# per la logica per il rendering vengono convertiti in una classe del componente. Il nome della classe generata corrisponde al nome del file.

I membri della classe del componente sono definiti un `@functions` blocco (più `@functions` blocco è consentito). Nel `@functions` blocco, stato del componente (proprietà, campi) viene specificato con i metodi per la gestione degli eventi o per definire logica altri componenti.

I membri di componenti possono quindi essere usati come parte del componente di rendering per la logica usando C# le espressioni che iniziano con `@`. Ad esempio, un C# viene eseguito il rendering di campo anteponendo `@` per il nome del campo. Nell'esempio seguente viene valutata e viene eseguito il rendering:

* `_headingFontStyle` il valore di proprietà CSS per `font-style`.
* `_headingText` per il contenuto di `<h1>` elemento.

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@functions {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

Dopo che il componente inizialmente viene eseguito il rendering, il componente Rigenera relativa struttura ad albero di rendering in risposta agli eventi. I componenti di Razor quindi confronta il nuovo albero di rendering confrontandolo con quello precedente e si applica a tutte le modifiche a modello di oggetti del browser documento (DOM).

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a>Integrare i componenti nelle App Razor Pages ed MVC

Usare i componenti con le app esistenti di Razor Pages ed MVC. Non è necessario riscrivere le pagine esistenti o le viste per utilizzare i componenti di Razor. Quando viene eseguito il rendering della pagina o della vista, i componenti sono già sottoposte a rendering&dagger; nello stesso momento. 

> [!NOTE]
> &dagger;Il rendering preliminare sul lato server è abilitato per App i componenti di Razor per impostazione predefinita. App sul lato client Blazor supporterà il rendering preliminare nella prossima versione di anteprima 4. Per altre informazioni, vedere [aggiornare i modelli/middleware usare MapFallbackToPage/File](https://github.com/aspnet/AspNetCore/issues/8852).

Per eseguire il rendering di un componente da una pagina o vista, utilizzare il `RenderComponentAsync<TComponent>` metodo helper HTML:

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

Viene eseguito il rendering delle pagine e delle viste di componenti non sono ancora interattivi nella versione Preview 3. Ad esempio, selezionando un pulsante non attiva una chiamata al metodo. Una futura versione di anteprima verranno risolvere questa limitazione e aggiungere il supporto per i componenti per il rendering usando la normale sintassi di elementi e attributi.

Sebbene le pagine e le viste è possono usare i componenti, non accade il contrario. I componenti non è possibile usare scenari specifici della pagina e visualizzazione, ad esempio le visualizzazioni parziali e sezioni. Da utilizzare per la logica di visualizzazione parziale in un componente, separare la logica di visualizzazione parziale in un componente.

## <a name="using-components"></a>Uso di componenti

I componenti possono includere altri componenti dichiarandoli usando la sintassi degli elementi HTML. Il markup per l'uso di un componente è simile a un tag HTML, in cui il nome del tag è il tipo di componente.

Il markup seguente esegue il rendering di un `HeadingComponent` (*HeadingComponent.cshtml*) istanza:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.cshtml?name=snippet_HeadingComponent)]

## <a name="component-parameters"></a>Parametri del componente

I componenti possono avere *parametri del componente*, che vengono definiti utilizzando *pubblici* le proprietà nella classe del componente decorati con `[Parameter]`. Usare gli attributi per specificare gli argomenti per un componente nel markup.

Nell'esempio seguente, il `ParentComponent` imposta il valore della `Title` proprietà del `ChildComponent`:

*ParentComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=5)]

*ChildComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=7-8)]

## <a name="child-content"></a>Contenuto figlio

I componenti possono impostare il contenuto di un altro componente. Il componente assegnazione fornisce il contenuto tra i tag che specificano il componente ricevente. Ad esempio, un `ParentComponent` grado di fornire contenuto per il rendering da un componente figlio da posizionare il contenuto all'interno di `<ChildComponent>` tag.

*ParentComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=6-7)]

Il componente figlio ha un `ChildContent` proprietà che rappresenta un `RenderFragment`. Il valore di `ChildContent` viene posizionato nel markup del componente figlio in cui il contenuto deve essere sottoposto a rendering. Nell'esempio seguente, il valore di `ChildContent` viene ricevuto dal componente padre e il rendering all'interno del pannello Bootstrap `panel-body`.

*ChildComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=3,10-11)]

> [!NOTE]
> La ricezione di proprietà di `RenderFragment` contenuto deve essere denominato `ChildContent` per convenzione.

## <a name="data-binding"></a>Associazione dati

Viene eseguita l'associazione dati a entrambi i componenti e gli elementi DOM con il `bind` attributo. L'esempio seguente associa il `ItalicsCheck` selezionata dello stato per la casella di controllo:

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    bind="@_italicsCheck">
```

Quando la casella di controllo è selezionata e viene deselezionata, il valore della proprietà viene aggiornato alla `true` e `false`, rispettivamente.

La casella di controllo viene aggiornata nell'interfaccia utente solo quando il componente viene eseguito il rendering, non in risposta alla modifica il valore della proprietà. Poiché i componenti stessi esegue il rendering dopo l'esecuzione di codice del gestore eventi, gli aggiornamenti delle proprietà sono in genere immediatamente nell'interfaccia utente.

Usando `bind` con un `CurrentValue` proprietà (`<input bind="@CurrentValue">`) è essenzialmente equivalente a quanto segue:

```cshtml
<input value="@CurrentValue" 
    onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)">
```

Quando il componente viene eseguito il rendering, il `value` dell'elemento di input proviene il `CurrentValue` proprietà. Quando l'utente digita nella casella di testo, il `onchange` evento viene generato e `CurrentValue` proprietà è impostata per il valore modificato. In realtà, la generazione di codice è un po' più complesso perché `bind` gestisce alcuni casi in cui vengono eseguite conversioni dei tipi. In linea di principio `bind` associa il valore corrente di un'espressione con un `value` attributo e gestisce le modifiche utilizzando il gestore registrato.

Oltre a `onchange`, la proprietà può essere associata usando altri eventi, ad esempio `oninput` verificando più esplicito sulla destinazione dell'associazione:

```cshtml
<input type="text" bind-value-oninput="@CurrentValue">
```

A differenza `onchange`, `oninput` viene attivato in tutti i caratteri che costituisce l'input nella casella di testo.

**Stringhe di formato**

Data binding dati funziona con <xref:System.DateTime> stringhe di formato. Altre espressioni di formato, ad esempio valuta o formati di numero, non sono disponibili in questo momento.

```cshtml
<input bind="@StartDate" format-value="yyyy-MM-dd">

@functions {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

Il `format-value` attributo specifica il formato della data da applicare al `value` del `input` elemento. Il formato viene usato anche per analizzare il valore quando un `onchange` evento si verifica.

**Parametri del componente**

Binding riconosce anche i parametri del componente, in cui `bind-{property}` possibile associare un valore della proprietà tra i componenti.

Il componente seguente usa `ChildComponent` e associa il `ParentYear` parametro dall'oggetto padre di `Year` parametro sul componente figlio:

Componente padre:

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent bind-Year="@ParentYear" />

<button class="btn btn-primary" onclick="@ChangeTheYear">
    Change Year to 1986
</button>

@functions {
    [Parameter]
    private int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

Componente di figlio:

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@functions {
    [Parameter]
    private int Year { get; set; }

    [Parameter]
    private Action<int> YearChanged { get; set; }
}
```

Il caricamento di `ParentComponent` genera il markup seguente:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

Se il valore della `ParentYear` proprietà viene modificata selezionando il pulsante nel `ParentComponent`, il `Year` proprietà del `ChildComponent` viene aggiornato. Il nuovo valore della `Year` viene eseguito il rendering nell'interfaccia utente quando il `ParentComponent` è modificate:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

Il `Year` parametro è associabile perché contiene complementare `YearChanged` che corrisponde al tipo di evento il `Year` parametro.

Per convenzione, `<ChildComponent bind-Year="@ParentYear" />` è essenzialmente equivalente a scrittura,

```cshtml
    <ChildComponent bind-Year-YearChanged="@ParentYear" />
```

In generale, una proprietà può essere associata a un gestore eventi corrispondente utilizzando `bind-property-event` attributo.

## <a name="event-handling"></a>Gestione di eventi

I componenti di Razor forniscono funzionalità di gestione degli eventi. Per un attributo dell'elemento HTML denominato `on<event>` (ad esempio `onclick`, `onsubmit`) con un valore di tipo delegato, Razor componenti considera il valore dell'attributo come un gestore eventi. Il nome dell'attributo inizia sempre con `on`.

Il codice seguente chiama il `UpdateHeading` metodo quando si seleziona il pulsante nell'interfaccia utente:

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    private void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

Il codice seguente chiama il `CheckboxChanged` metodo quando la casella di controllo viene modificata nell'interfaccia utente:

```cshtml
<input type="checkbox" class="form-check-input" onchange="@CheckboxChanged">

@functions {
    private void CheckboxChanged()
    {
        ...
    }
}
```

Gestori di eventi possono anche essere asincrono e restituire un <xref:System.Threading.Tasks.Task>. Non è necessario chiamare manualmente `StateHasChanged()`. Le eccezioni vengono registrate quando si verificano.

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    private async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

Per alcuni eventi, sono consentiti i tipi di argomento di evento specifiche degli eventi. Se l'accesso a uno di questi tipi di evento non è necessaria, non è necessario nella chiamata al metodo.

L'elenco degli argomenti dell'evento supportato è:

* UIEventArgs
* UIChangeEventArgs
* UIKeyboardEventArgs
* UIMouseEventArgs

Sono anche possibile usare espressioni lambda:

```cshtml
<button onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

Spesso è comodo chiuse su valori aggiuntivi, ad esempio quando si esegue l'iterazione in un set di elementi. L'esempio seguente crea tre pulsanti, ognuno dei quali chiama `UpdateHeading` passando un argomento dell'evento (`UIMouseEventArgs`) e il relativo numero di pulsante (`buttonNumber`) se selezionata nell'interfaccia utente:

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@functions {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            "mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> Effettuare **non** usare la variabile di ciclo (`i`) in un `for` ciclo direttamente in un'espressione lambda. In caso contrario, la stessa variabile viene utilizzata da tutte le espressioni lambda causando `i`del valore per lo stesso in tutte le espressioni lambda. Sempre acquisisce il relativo valore in una variabile locale (`buttonNumber` nell'esempio precedente) e quindi usarla.

## <a name="capture-references-to-components"></a>Acquisire riferimenti ai componenti

Riferimenti a componenti forniscono un modo get un riferimento a un'istanza del componente in modo che è possibile inviare comandi a tale istanza, ad esempio `Show` o `Reset`. Per acquisire un riferimento del componente, aggiungere un `ref` attributo al componente figlio e quindi definire un campo con lo stesso nome e lo stesso tipo del componente figlio.

```cshtml
<MyLoginDialog ref="loginDialog" ... />

@functions {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

Quando il componente viene eseguito il rendering, il `loginDialog` campo viene popolato con il `MyLoginDialog` istanza del componente figlio. È quindi possibile richiamare i metodi .NET nell'istanza del componente.

> [!IMPORTANT]
> Il `loginDialog` variabile viene popolata solo dopo che il componente viene eseguito il rendering e l'output include il `MyLoginDialog` elemento. Fino a quel punto, non sono necessarie per fare riferimento. Per modificare i riferimenti a componenti dopo che il componente ha terminato il rendering, utilizzare il `OnAfterRenderAsync` o `OnAfterRender` metodi.

Durante l'acquisizione di riferimenti a componenti utilizza una sintassi simile a [riferimenti a elementi di acquisizione](xref:razor-components/javascript-interop#capture-references-to-elements), non è un [interoperabilità JavaScript](xref:razor-components/javascript-interop) funzionalità. Riferimenti a componenti non sono passati al codice JavaScript. vengono utilizzati solo nel codice .NET.

> [!NOTE]
> Effettuare **non** usare riferimenti a componenti per modificare lo stato dei componenti figlio. Utilizzare invece parametri normali dichiarativi per passare i dati per i componenti figlio. In questo modo i componenti figlio eseguire nuovamente il rendering negli orari corretti automaticamente.

## <a name="lifecycle-methods"></a>Metodi del ciclo di vita

`OnInitAsync` e `OnInit` eseguire codice per inizializzare il componente. Per eseguire un'operazione asincrona, utilizzare `OnInitAsync` e il `await` parola chiave per l'operazione:

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

Per un'operazione sincrona, usare `OnInit`:

```csharp
protected override void OnInit()
{
    ...
}
```

`OnParametersSetAsync` e `OnParametersSet` vengono chiamati quando un componente ha ricevuto i parametri dal relativo elemento padre e i valori vengono assegnati alle proprietà. Questi metodi vengono eseguiti dopo l'inizializzazione del componente e quindi ogni volta che il componente viene eseguito il rendering:

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

```csharp
protected override void OnParametersSet()
{
    ...
}
```

`OnAfterRenderAsync` e `OnAfterRender` vengono chiamati dopo che un componente ha completato il rendering. I riferimenti di elemento e dei componenti vengono popolati a questo punto. Utilizzare questa fase per eseguire ulteriori passaggi di inizializzazione utilizzando il contenuto sottoposto a rendering, ad esempio l'attivazione di librerie JavaScript di terze parti che operano su elementi DOM di cui è stato eseguito il rendering.

```csharp
protected override async Task OnAfterRenderAsync()
{
    await ...
}
```

```csharp
protected override void OnAfterRender()
{
    ...
}
```

`SetParameters` può essere sottoposto a override per eseguire codice prima che vengano impostati i parametri:

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

Se `base.SetParameters` non viene richiamato, il codice personalizzato può interpretare il valore di parametri in entrata in qualsiasi modo richiesto. Ad esempio, i parametri in entrata non è necessari essere assegnati alle proprietà nella classe.

`ShouldRender` può essere sottoposto a override per eliminare l'aggiornamento dell'interfaccia utente. Se l'implementazione restituisce `true`, l'interfaccia utente viene aggiornato. Anche se `ShouldRender` viene sottoposto a override, il componente è sempre inizialmente sottoposto a rendering.

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a>Eliminazione di componenti con IDisposable

Se un componente implementa <xref:System.IDisposable>, il [metodo Dispose](/dotnet/standard/garbage-collection/implementing-dispose) viene chiamato quando il componente è stato rimosso dall'interfaccia utente. Il componente seguente usa `@implements IDisposable` e il `Dispose` metodo:

```csharp
@using System
@implements IDisposable

...

@functions {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a>Routing

Routing in Razor componenti avviene fornendo un modello di route per ogni componente accessibile nell'app.

Quando un *. cshtml* file con un `@page` direttiva viene compilata, la classe generata viene assegnata un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> che specifica il modello di route. In fase di esecuzione, il router cerca classi di componenti con un `RouteAttribute` ed esegue il rendering qualunque sia il componente dispone di un modello di route corrispondente all'URL richiesto.

Più modelli di route possono essere applicati a un componente. Il seguente componente risponde alle richieste per `/BlazorRoute` e `/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a>Parametri di route

I componenti possono accettare parametri di route dal modello di route di cui il `@page` direttiva. Il router utilizza i parametri di route per popolare i parametri del componente corrispondente.

*RouteParameter.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter)]

Non sono supportati i parametri facoltativi, pertanto, due `@page` direttive vengono applicate nell'esempio precedente. La prima consente al componente senza un parametro di navigazione. La seconda `@page` direttiva accetta il `{text}` parametro di route e assegna il valore per il `Text` proprietà.

## <a name="base-class-inheritance-for-a-code-behind-experience"></a>Ereditarietà di classe di base per un'esperienza "code-behind"

File componente (*. cshtml*) combinare il markup HTML e C# codice nello stesso file di elaborazione. Il `@inherits` direttiva può essere utilizzata per fornire App Razor componenti con un'esperienza "code-behind" che separa il markup componente dal codice per l'elaborazione.

Il [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) Mostra come un componente può ereditare una classe di base, `BlazorRocksBase`, per fornire i metodi e proprietà del componente.

*BlazorRocks.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.cshtml?name=snippet_BlazorRocks)]

*BlazorRocksBase.cs*:

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

La classe di base deve derivare da `ComponentBase`.

## <a name="razor-support"></a>Supporto Razor

**Direttive Razor**

Le direttive Razor vengono visualizzate nella tabella seguente.

| Direttiva | Descrizione |
| --------- | ----------- |
| [\@Funzioni](xref:mvc/views/razor#section-5) | Aggiunge un C# blocco di codice a un componente. |
| `@implements` | Implementa un'interfaccia per la classe del componente generato. |
| [\@Eredita](xref:mvc/views/razor#section-3) | Fornisce il controllo completo della classe che eredita il componente. |
| [\@inserire](xref:mvc/views/razor#section-4) | Consente di inserire di servizi di [contenitore di servizi](xref:fundamentals/dependency-injection). Per altre informazioni, vedere [Inserimento di dipendenze in visualizzazioni](xref:mvc/views/dependency-injection). |
| `@layout` | Specifica un componente del layout. Componenti di layout vengono utilizzati per evitare incoerenze e la duplicazione del codice. |
| [\@Pagina](xref:razor-pages/index#razor-pages) | Specifica che il componente deve gestire le richieste direttamente. Il `@page` direttiva può essere specificata con una route e i parametri facoltativi. A differenza di Razor Pages, il `@page` direttiva non deve necessariamente essere la prima direttiva all'inizio del file. Per altre informazioni, vedere [Routing](xref:razor-components/routing). |
| [\@Utilizzo](xref:mvc/views/razor#using) | Aggiunge il C# `using` direttive alla classe del componente generato. |
| [\@addTagHelper](xref:mvc/views/razor#tag-helpers) | Usare `@addTagHelper` per utilizzare un componente in un assembly diverso rispetto all'assembly dell'app. |

**Attributi condizionali**

Gli attributi vengono visualizzati in modo condizionale in base al valore di .NET. Se il valore è `false` o `null`, non viene eseguito il rendering dell'attributo. Se il valore è `true`, viene eseguito il rendering dell'attributo ridotta a icona.

Nell'esempio riportato di seguito `IsCompleted` determina se `checked` viene eseguito il rendering nel markup del controllo:

```cshtml
<input type="checkbox" checked="@IsCompleted">

@functions {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

Se `IsCompleted` è `true`, la casella di controllo è sottoposto a rendering come:

```html
<input type="checkbox" checked>
```

Se `IsCompleted` è `false`, la casella di controllo è sottoposto a rendering come:

```html
<input type="checkbox">
```

**Informazioni aggiuntive su Razor**

Per altre informazioni su Razor, vedere la [riferimento alla sintassi Razor](xref:mvc/views/razor).

## <a name="raw-html"></a>HTML non elaborato

Le stringhe vengono in genere viene eseguito il rendering usando i nodi di testo DOM, il che significa che qualsiasi tag che possono contenere viene ignorata e trattata come testo letterale. Per eseguire il rendering HTML non elaborato, eseguire il wrapping del contenuto HTML in un `MarkupString` valore. Il valore viene analizzato come HTML o SVG e inserito nel DOM.

> [!WARNING]
> Il rendering HTML non elaborato costruito da qualsiasi non attendibile l'origine è un **rischio per la sicurezza** e deve essere evitata.

Nell'esempio seguente viene illustrato l'utilizzo di `MarkupString` tipo da aggiungere un blocco di contenuto HTML statico all'output del rendering di un componente:

```html
@((MarkupString)myMarkup)

@functions {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a>Componenti basati su modelli

Componenti basati su modelli sono componenti che accettano uno o più modelli dell'interfaccia utente come parametri, che quindi possono essere utilizzati come parte della logica di rendering del componente. Componenti basati su modelli consentono di modificare i componenti di livello superiore che sono più riutilizzabili più componenti regolari. Di seguito sono riportati alcuni esempi:

* Un componente di tabella che consente agli utenti di specificare i modelli per l'intestazione della tabella, righe e nel piè di pagina.
* Un componente di elenco che consente agli utenti di specificare un modello per il rendering degli elementi in un elenco.

### <a name="template-parameters"></a>Parametri di modelli

Un componente basato su modelli viene definito specificando uno o più parametri del componente di tipo `RenderFragment` o `RenderFragment<T>`. Un frammento di rendering rappresenta un segmento dell'interfaccia utente che viene eseguito il rendering dal componente. Facoltativamente, un frammento di rendering accetta un parametro che può essere specificato quando viene richiamato il frammento di rendering.

*Components/TableTemplate.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.cshtml)]

Quando si usa un componente basato su modelli, i parametri del modello possono essere specificati usando gli elementi figlio che corrispondono ai nomi dei parametri (`TableHeader` e `RowTemplate` nell'esempio seguente):

```cshtml
<TableTemplate Items="@pets">
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

Gli argomenti di componente di tipo `RenderFragment<T>` passato come elementi hanno un parametro implicito denominato `context` (ad esempio dall'esempio di codice precedente, `@context.PetId`), ma è possibile modificare il nome di parametro usando il `Context` attributo sull'elemento figlio elemento. Nell'esempio seguente, il `RowTemplate` dell'elemento `Context` attributo specifica il `pet` parametro:

```cshtml
<TableTemplate Items="@pets">
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

In alternativa, è possibile specificare il `Context` attributo sull'elemento component. L'oggetto specificato `Context` attributo viene applicato a tutti i parametri di modello specificato. Ciò risulta utile quando si desidera specificare il nome di parametro del contenuto per il contenuto figlio implicita (senza wrapping qualsiasi elemento figlio). Nell'esempio seguente, il `Context` attributo incluso nella `TableTemplate` elemento e si applica a tutti i parametri di modello:

```cshtml
<TableTemplate Items="@pets" Context="pet">
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

### <a name="generic-typed-components"></a>Componenti tipizzate generiche

Componenti basati su modelli sono spesso in modo generico tipizzati. Ad esempio, un componente di modello di visualizzazione elenco generico è utilizzabile per eseguire il rendering `IEnumerable<T>` valori. Per definire un componente generico, usare il `@typeparam` direttiva per specificare i parametri di tipo.

*Components/ListViewTemplate.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.cshtml)]

Quando si usano componenti tipizzate generiche, è possibile che il parametro di tipo viene dedotto se possibile:

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

In caso contrario, il parametro di tipo deve essere specificato in modo esplicito utilizzando un attributo che corrisponde al nome del parametro di tipo. Nell'esempio seguente, `TItem="Pet"` specifica il tipo:

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a>I parametri e valori di propagazione

In alcuni scenari, è poco pratica per propagare dati da un componente predecessore in un componente discendente utilizzando [parametri del componente](#component-parameters), soprattutto quando sono presenti diversi livelli di componente. Propagazione di valori e parametri di risolvono questo problema fornendo un modo pratico per un componente predecessore fornire un valore per tutti i suoi componenti discendente. Propagazione di valori e parametri forniscono anche un approccio per i componenti per il coordinamento.

### <a name="theme-example"></a>Esempio di tema

Di seguito *tema* esempio di app di esempio, il `ThemeInfo` classe specifica le informazioni sul tema per flusso verso il basso della gerarchia di componenti in modo che tutti i pulsanti all'interno di una determinata parte dell'app di condividere lo stesso stile.

*UIThemeClasses/ThemeInfo.cs*:

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

Un componente predecessore è possibile fornire un valore a catena mediante il componente di valore a catena. Il componente di propagazione valore esegue il wrapping di un sottoalbero della gerarchia di componente e viene fornito un valore singolo per tutti i componenti all'interno di quel sottoalbero.

Ad esempio, l'app di esempio specifica le informazioni sul tema (`ThemeInfo`) in uno dei layout dell'app come un parametro di propagazione per tutti i componenti che costituiscono il corpo di layout del `@Body` proprietà. `ButtonClass` viene assegnato un valore di `btn-success` nel componente del layout. Qualsiasi componente discendenti può utilizzare questa proprietà tramite la `ThemeInfo` oggetto CSS.

*Shared/CascadingValuesParametersLayout.cshtml*:

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="@theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@functions {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

Per rendere utilizzare valori di propagazione, i componenti dichiarare i parametri di propagazione tramite la `[CascadingParameter]` attributo o basata su un valore di nome di stringa:

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

Associazione con un valore di nome di stringa è rilevante se si hanno più valori di propagazione dello stesso tipo ed è necessario per differenziarle all'interno del sottoalbero stesso.

Valori di propagazione sono associati a parametri di propagazione per tipo.

Nelle app di esempio, il componente di propagazione valori parametri tema viene associato ai `ThemeInfo` valore CSS per un parametro di propagazione. Il parametro viene utilizzato per impostare la classe CSS per uno dei pulsanti visualizzati dal componente.

*Pages/CascadingValuesParametersTheme.cshtml*:

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" onclick="@IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" onclick="@IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@functions {
    private int currentCount = 0;

    [CascadingParameter] protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

### <a name="tabset-example"></a>Esempio TabSet

I parametri di propagazione consentono inoltre ai componenti di collaborare tra la gerarchia di componenti. Ad esempio, tenere presente quanto segue *TabSet* esempio nell'app di esempio.

L'app di esempio ha un `ITab` interfaccia che implementano con schede:

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

Il componente di propagazione valori parametri TabSet utilizza il componente impostato scheda, che contiene diversi componenti di scheda:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.cshtml?name=snippet_TabSet)]

I componenti di scheda figlio in modo esplicito non vengono passati come parametri per impostare la scheda. Al contrario, i componenti di scheda figlio fanno parte del contenuto figlio di un Set di scheda. Tuttavia, impostare la scheda ancora dovrebbe conoscere su ogni componente di scheda in modo che possa eseguire il rendering di intestazioni e la scheda attiva. Per consentire tale coordinamento senza richiedere codice aggiuntivo, il componente scheda impostata *stesso può fornire come valore di propagazione* che viene quindi prelevata dai componenti di scheda discendenti.

*Components/TabSet.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.cshtml)]

L'acquisizione di componenti scheda discendente che lo contiene scheda impostata come un parametro di propagazione, in modo che i componenti di scheda aggiungono scheda Imposta le coordinate nella quale scheda è attivo.

*Components/Tab.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.cshtml)]

## <a name="razor-templates"></a>Modelli Razor

Eseguire il rendering di frammenti possono essere definiti utilizzando la sintassi dei modelli Razor. I modelli Razor sono un modo per definire un frammento di codice dell'interfaccia utente e si presuppone il formato seguente:

```cshtml
@<tag>...</tag>
```

Nell'esempio seguente viene illustrato come specificare `RenderFragment` e `RenderFragment<T>` valori.

*RazorTemplates.cshtml*:

```cshtml
@{
    RenderFragment template = @<p>The time is @DateTime.Now.</p>;
    RenderFragment<Pet> petTemplate = (pet) => @<p>Your pet's name is @pet.Name.</p>;
}
```

Eseguire il rendering frammenti definiti usando Razor modelli possono essere passati come argomenti per componenti basati su modelli o sottoposto a rendering direttamente. Ad esempio, i modelli precedenti sono direttamente il rendering con il markup Razor seguente:

```cshtml
@template

@petTemplate(new Pet { Name = "Rex" })
```

Output sottoposto a rendering:

```
The time is 10/04/2018 01:26:52.

Your pet's name is Rex.
```

## <a name="manual-rendertreebuilder-logic"></a>Per la logica RenderTreeBuilder manuale

`Microsoft.AspNetCore.Components.RenderTree` fornisce metodi per la modifica di componenti e dagli elementi, inclusa la creazione di componenti manualmente in C# codice.

> [!NOTE]
> Uso di `RenderTreeBuilder` per creare componenti è uno scenario avanzato. Un componente in formato non valido (ad esempio, un tag di markup non chiusi) può causare un comportamento indefinito.

Si consideri il seguente componente di dettagli Pet (*PetDetails.razor* nei componenti di Razor; *PetDetails.cshtml* in Blazor), che può essere compilato manualmente in un altro componente:

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote<p>

@functions
{
    [Parameter]
    string PetDetailsQuote { get; set; }
}
```

Nell'esempio seguente, al ciclo impostata nel `CreateComponent` metodo genera tre componenti dettagli Pet. Quando si chiama `RenderTreeBuilder` metodi per creare i componenti (`OpenComponent` e `AddAttribute`), numeri di sequenza sono numeri di riga di codice sorgente. I componenti di Razor algoritmo differenza si basa sui numeri di sequenza corrispondenti alle righe distinte del codice, distinct non chiamare le chiamate. Quando si crea un componente con `RenderTreeBuilder` metodi, impostare come hardcoded gli argomenti per i numeri di sequenza. **Utilizzo di un calcolo o un contatore per generare il numero di sequenza può causare una riduzione delle prestazioni.**

Componente compilato (*BuiltContent.razor* nei componenti di Razor; *BuiltContent.cshtml* in Blazor):

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" onclick="@RenderComponent">
    Create three Pet Details components
</button>

@functions {
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
