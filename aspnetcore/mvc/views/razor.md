---
title: Guida di riferimento della sintassi Razor per ASP.NET Core
author: rick-anderson
description: Informazioni sulla sintassi di markup Razor per l'incorporamento di codice basato su server in pagine Web.
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/razor
ms.openlocfilehash: 224c855b355b8ecde36377bba6966edec251af6a
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33962492"
---
# <a name="razor-syntax-reference-for-aspnet-core"></a>Guida di riferimento della sintassi Razor per ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen) e [Dan Vicarel](https://github.com/Rabadash8820)

Razor è una sintassi di markup per l'incorporamento di codice basato su server in pagine Web. La sintassi Razor è costituita da markup Razor, C# e HTML. I file contenenti Razor in genere presentano l'estensione *cshtml*.

## <a name="rendering-html"></a>Rendering di HTML

Il linguaggio Razor predefinito è HTML. Il rendering HTML dal markup Razor non è diverso dal rendering HTML da un file HTML. Il rendering del markup HTML nei file Razor con estensione *cshtml* viene eseguito dal server senza modifiche.

## <a name="razor-syntax"></a>Sintassi Razor

Razor supporta C# e usa il simbolo `@` per la transizione da HTML a C#. Razor valuta le espressioni C# e ne esegue il rendering nell'output HTML.

Quando un simbolo `@` è seguito da una [parola chiave riservata Razor](#razor-reserved-keywords), la transizione avviene in un markup specifico per Razor, in caso contrario in C# semplice.

Per usare un simbolo `@` come carattere di escape nel markup Razor, usare un secondo simbolo `@`:

```cshtml
<p>@@Username</p>
```

Il rendering del codice viene eseguito in HTML con un solo simbolo `@`:

```html
<p>@Username</p>
```

Gli attributi e il contenuto HTML contenenti indirizzi di posta elettronica non considerano il simbolo `@` come un carattere di transizione. Gli indirizzi di posta elettronica nell'esempio seguente non vengono modificati dall'analisi Razor:

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a>Espressioni implicite Razor

Le espressioni implicite Razor iniziano con `@` seguito dal codice C#:

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

Fatta eccezione per la parola chiave `await` di C#, le espressioni implicite non devono contenere spazi. Se l'istruzione C# ha una fine chiaramente definita, possono coesistere spazi:

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

Le espressioni implicite **non possono** contenere generics C#, poiché i caratteri all'interno delle parentesi (`<>`) vengono interpretati come un tag HTML. Il codice seguente **non** è valido:

```cshtml
<p>@GenericMethod<int>()</p>
```

Il codice precedente genera un errore del compilatore simile a uno dei seguenti:

 * L'elemento "int" non è stato chiuso. Tutti gli elementi devono essere a chiusura automatica o avere un tag di fine corrispondente.
 *  Non è possibile convertire il gruppo di metodi 'GenericMethod' nel tipo non delegato 'object'. Si intendeva chiamare il metodo?' 
 
Le chiamate di metodo generiche devono essere racchiuse in un'[espressione esplicita Razor](#explicit-razor-expressions) o in un [blocco di codice Razor](#razor-code-blocks).

## <a name="explicit-razor-expressions"></a>Espressioni esplicite Razor

Le espressioni esplicite Razor sono costituite da un simbolo `@` con parentesi bilanciate. Per eseguire il rendering dell'ora dell'ultima settimana, viene usato il markup Razor seguente:

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

Qualsiasi contenuto all'interno delle parentesi `@()` viene valutato e sottoposto a rendering nell'output.

Le espressioni implicite, descritte nella sezione precedente, in genere non possono contenere spazi. Nel codice seguente, una settimana non viene sottratta dall'ora corrente:

[!code-cshtml[](razor/sample/Views/Home/Contact.cshtml?range=17)]

Il codice esegue il rendering dell'HTML seguente:

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

È possibile usare le espressioni esplicite per concatenare testo con un risultato dell'espressione:

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

Senza l'espressione esplicita, `<p>Age@joe.Age</p>` viene considerato come un indirizzo di posta elettronica e `<p>Age@joe.Age</p>` viene sottoposto a rendering. Se viene scritto come espressione esplicita, `<p>Age33</p>` viene sottoposto a rendering.

È possibile usare le espressioni esplicite per eseguire il rendering dell'output da metodi generici in file con estensione *cshtml*. Il markup seguente illustra come correggere l'errore riportato in precedenza, causato dalle parentesi quadre di un oggetto generico C#. Il codice viene scritto come un'espressione esplicita:

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a>Codifica di espressioni

Le espressioni C# che restituiscono una stringa sono codificate in HTML. Le espressioni C# che restituiscono `IHtmlContent` vengono sottoposte a rendering direttamente tramite `IHtmlContent.WriteTo`. Le espressioni C# che non restituiscono `IHtmlContent` vengono convertite in una stringa da `ToString` e codificate prima di essere sottoposte a rendering.

```cshtml
@("<span>Hello World</span>")
```

Il codice esegue il rendering dell'HTML seguente:

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

Il codice HTML viene visualizzato nel browser come:

```
<span>Hello World</span>
```

L'output `HtmlHelper.Raw` non è codificato ma viene sottoposto a rendering come markup HTML.

> [!WARNING]
> L'uso di `HtmlHelper.Raw` su input utente non purificato costituisce un rischio per la sicurezza. L'input utente potrebbe contenere JavaScript dannoso o altri attacchi. La purificazione degli input utente è difficile. Evitare l'uso di `HtmlHelper.Raw` con l'input utente.

```cshtml
@Html.Raw("<span>Hello World</span>")
```

Il codice esegue il rendering dell'HTML seguente:

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a>Blocchi di codice Razor

I blocchi di codice Razor iniziano con `@` e sono racchiusi tra `{}`. A differenza delle espressioni, il codice C# all'interno di blocchi di codice non viene sottoposto a rendering. I blocchi di codice e le espressioni in una visualizzazione condividono lo stesso ambito e vengono definiti in ordine:

```cshtml
@{
    var quote = "The future depends on what you do today. - Mahatma Gandhi";
}

<p>@quote</p>

@{
    quote = "Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.";
}

<p>@quote</p>
```

Il codice esegue il rendering dell'HTML seguente:

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a>Transizioni implicite

Il linguaggio predefinito in un blocco di codice è C#, ma la pagina Razor può eseguire la transizione a HTML:

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a>Transizione esplicita delimitata

Per definire una sottosezione di un blocco di codice che deve eseguire il rendering HTML, racchiudere i caratteri per il rendering all'interno del tag Razor **\<text>**:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

Usare questo approccio per eseguire il rendering di HTML che non è racchiuso tra tag HTML. Senza un tag HTML o Razor, si verifica un errore di runtime Razor.

Il tag **\<text>** è utile per controllare gli spazi vuoti durante il rendering del contenuto:

* Solo il contenuto tra il tag **\<text>** viene sottoposto a rendering. 
* Non vengono visualizzati spazi vuoti prima o dopo il tag **\<text>** nell'output HTML.

### <a name="explicit-line-transition-with-"></a>Transizione riga esplicita con @:

Per eseguire il rendering del resto di un'intera riga come HTML all'interno di un blocco di codice, usare la sintassi `@:`:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

Senza il simbolo `@:` nel codice, viene generato un errore di runtime Razor.

Avviso: la presenza di caratteri `@` aggiuntivi in un file Razor può causare errori del compilatore nelle istruzioni successive del blocco. Questi errori del compilatore possono essere difficili da comprendere perché l'errore effettivo si verifica prima dell'errore segnalato. Questo errore è comune dopo la combinazione di più espressioni implicite/esplicite in un singolo blocco di codice.

## <a name="control-structures"></a>Strutture di controllo

Le strutture di controllo sono un'estensione dei blocchi di codice. Tutti gli aspetti dei blocchi di codice (transizione al markup, C# inline) sono validi anche per le strutture seguenti:

### <a name="conditionals-if-else-if-else-and-switch"></a>Istruzioni condizionali @if, else if, else e @switch

`@if` controlla quando viene eseguito il codice:

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

`else` e `else if` non richiedono il simbolo `@`:

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value is odd and small.</p>
}
```

Nel markup seguente viene illustrato come usare un'istruzione switch:

```cshtml
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number wasn't 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a>Ciclo @for, @foreach, @while, e @do while

È possibile eseguire il rendering di HTML basato su modelli con le istruzioni di controllo ciclo. Per eseguire il rendering di un elenco di persone:

```cshtml
@{
    var people = new Person[]
    {
          new Person("Weston", 33),
          new Person("Johnathon", 41),
          ...
    };
}
```

Sono supportate le seguenti istruzioni di ciclo:

`@for`

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```cshtml
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```cshtml
@{ var i = 0; }
@while (i < people.Length)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
}
```

`@do while`

```cshtml
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a>Istruzione @using composita

In C# viene usata un'istruzione `using` per verificare che un oggetto sia stato eliminato. In Razor lo stesso meccanismo viene usato per creare gli helper HTML che includono contenuto aggiuntivo. Nel codice seguente, l'helper HTML esegue il rendering di un tag di modulo con l'istruzione `@using`:


```cshtml
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

Con gli [helper tag](xref:mvc/views/tag-helpers/intro) è possibile eseguire azioni a livello di ambito.

### <a name="try-catch-finally"></a>@try, catch, finally

La gestione delle eccezioni è simile a C#:

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

Razor è in grado di proteggere le sezioni critiche con le istruzioni di blocco:

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>Commenti

Razor supporta i commenti in C# e HTML:

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

Il codice esegue il rendering dell'HTML seguente:

```html
<!-- HTML comment -->
```

I commenti Razor vengono rimossi dal server prima che la pagina Web venga sottoposta a rendering. Razor usa `@*  *@` per delimitare i commenti. Il codice seguente è commentato, pertanto il server non esegue il rendering di alcun markup:

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a>Direttive

Le direttive Razor sono rappresentate da espressioni implicite con parole chiave riservate dopo il simbolo `@`. Una direttiva cambia in genere il modo in cui viene analizzata una visualizzazione o abilita funzionalità diverse.

Comprendere come Razor genera il codice per una visualizzazione facilita la comprensione del funzionamento delle direttive.

[!code-html[](razor/sample/Views/Home/Contact8.cshtml)]

Il codice genera una classe simile alla seguente:

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Getting old ain't for wimps! - Anonymous";

        WriteLiteral("/r/n<div>Quote of the Day: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

Nella sezione [Visualizzazione della classe C# Razor generata per una visualizzazione](#viewing-the-razor-c-class-generated-for-a-view) più avanti in questo articolo viene illustrato come visualizzare la classe generata.

<a name="using"></a>
### <a name="using"></a>@using

La direttiva `@using` aggiunge la direttiva C# `using` alla visualizzazione generata:

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

La direttiva `@model` specifica il tipo del modello passato a una visualizzazione:

```cshtml
@model TypeNameOfModel
```

In un'app ASP.NET MVC di base creata con account utente individuali, la visualizzazione *Views/Account/Login.cshtml* contiene la dichiarazione di modello seguente:

```cshtml
@model LoginViewModel
```

La classe generata eredita da `RazorPage<dynamic>`:

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

Razor espone una proprietà `Model` per l'accesso al modello passato alla visualizzazione:

```cshtml
<div>The Login Email: @Model.Email</div>
```

La direttiva `@model` specifica il tipo di questa proprietà. La direttiva specifica l'elemento `T` in `RazorPage<T>` che ha generato la classe da cui deriva la visualizzazione. Se la direttiva `@model` non è specificata, la proprietà `Model` è di tipo `dynamic`. Il valore del modello viene passato dal controller alla visualizzazione. Per altre informazioni, vedere [Modelli fortemente tipizzati e parola chiave &commat;model](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).

### <a name="inherits"></a>@inherits

La direttiva `@inherits` offre il controllo completo della classe che viene ereditata dalla visualizzazione:

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

Il codice seguente è un tipo di pagina Razor personalizzato:

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

L'elemento `CustomText` viene inserito in una visualizzazione:

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

Il codice esegue il rendering dell'HTML seguente:

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 È possibile usare `@model` e `@inherits` nella stessa visualizzazione. `@inherits` può trovarsi in un file *_ViewImports.cshtml* che viene importato dalla visualizzazione:

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

Il codice seguente è un esempio di visualizzazione fortemente tipizzata:

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

Se "rick@contoso.com" viene passato nel modello, la visualizzazione genera il markup HTML seguente:

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


La direttiva `@inject` consente alla pagina Razor di inserire un servizio dal [contenitore di servizi](xref:fundamentals/dependency-injection) in una visualizzazione. Per altre informazioni, vedere [Inserimento di dipendenze in visualizzazioni](xref:mvc/views/dependency-injection).

### <a name="functions"></a>@functions

La direttiva `@functions` consente a una pagina Razor di aggiungere un blocco di codice C# a una visualizzazione:

```cshtml
@functions { // C# Code }
```

Ad esempio:

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

Il codice genera il markup HTML seguente:

```html
<div>From method: Hello</div>
```

Il codice seguente è la classe C# Razor generata:

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

La direttiva `@section` viene usata in combinazione con il [layout](xref:mvc/views/layout) per abilitare le visualizzazioni per il rendering del contenuto in diverse parti della pagina HTML. Per altre informazioni, vedere [Sezioni](xref:mvc/views/layout#layout-sections-label).

## <a name="tag-helpers"></a>Helper tag

Esistono tre direttive che riguardano gli [helper tag](xref:mvc/views/tag-helpers/intro).

| Direttiva | Funzione |
| --------- | -------- |
| [&commat;addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | Rende gli helper tag disponibili per una visualizzazione. |
| [&commat;removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | Rimuove gli helper tag aggiunti in precedenza da una visualizzazione. |
| [&commat;tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | Specifica un prefisso del tag per abilitare il supporto dell'helper tag e renderne esplicito l'uso. |

## <a name="razor-reserved-keywords"></a>Parole chiave riservate Razor

### <a name="razor-keywords"></a>Parole chiave Razor

* page (richiede ASP.NET Core 2.0 e versioni successive)
* namespace
* funzioni
* eredita
* modello
* section
* helper (attualmente non supportata da ASP.NET Core)

Le parole chiave Razor sono precedute dal carattere di escape `@(Razor Keyword)` (ad esempio, `@(functions)`).

### <a name="c-razor-keywords"></a>Parole chiave Razor C#

* maiuscole e minuscole
* do
* default
* for
* foreach
* if
* else
* blocco
* switch
* try
* catch
* finally
* utilizzo
* while

Le parole chiave Razor C# devono essere precedute dal doppio carattere di escape `@(@C# Razor Keyword)` (ad esempio, `@(@case)`). Il primo `@` è il carattere di escape del parser Razor. Il secondo `@` è il carattere di escape del parser C#.

### <a name="reserved-keywords-not-used-by-razor"></a>Parole chiave riservate non usate da Razor

* classe

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a>Visualizzazione della classe C# Razor generata per una visualizzazione

Aggiungere la classe seguente al progetto ASP.NET Core MVC:

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

Eseguire l'override dell'elemento `RazorTemplateEngine` aggiunto da MVC con la classe `CustomTemplateEngine`:

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

Impostare un punto di interruzione per l'istruzione `return csharpDocument` di `CustomTemplateEngine`. Quando l'esecuzione del programma si interrompe al punto di interruzione, visualizzare il valore di `generatedCode`.

![Visualizzazione nel visualizzatore testo del codice generato](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a>Visualizzazione di ricerche e distinzione tra maiuscole e minuscole

Il motore di visualizzazione Razor esegue ricerche con distinzione tra maiuscole e minuscole per le visualizzazioni. La ricerca effettiva è tuttavia determinata dal file system sottostante:

* Origine basata su file: 
  * Nei sistemi operativi con file system che non fanno distinzione tra maiuscole e minuscole (ad esempio, Windows), le ricerche del provider di file fisici non eseguono la distinzione tra maiuscole e minuscole. Ad esempio, `return View("Test")` comporta corrispondenze per */Views/Home/Test.cshtml*, */Views/home/test.cshtml* e qualsiasi altra variante di maiuscole e minuscole.
  * Nei file system che fanno distinzione tra maiuscole e minuscole (ad esempio Linux, OS x e con `EmbeddedFileProvider`), le ricerche eseguono la distinzione tra maiuscole e minuscole. Ad esempio, `return View("Test")` trova specificamente le corrispondenze per */Views/Home/Test.cshtml*.
* Visualizzazioni precompilate: con ASP.NET Core 2.0 e versioni successive, la ricerca di visualizzazioni precompilate non esegue la distinzione tra maiuscole e minuscole in tutti i sistemi operativi. Questo comportamento è identico al comportamento del provider di file fisici in Windows. Se due visualizzazioni precompilate differiscono solo nelle lettere maiuscole e minuscole, il risultato di ricerca è non deterministico.

Gli sviluppatori sono invitati a far corrispondere le maiuscole e minuscole dei nomi di file e directory per quanto riguarda le maiuscole e minuscole di:

    * Nomi di area, controller e azione. 
    * Pagine Razor.
    
La distinzione tra maiuscole e minuscole assicura che le distribuzioni trovino le proprie visualizzazioni indipendentemente dal file system sottostante.
