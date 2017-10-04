---
title: Riferimento della sintassi Razor per ASP.NET Core
author: guardrex
description: Informazioni sulla sintassi Razor per l'incorporamento di codice basato su server in pagine Web.
keywords: Direttive Razor di ASP.NET Core, Razor,
ms.author: riande
manager: wpickett
ms.date: 09/29/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: 532e278597a0029b5bae93068af5b7b147c35688
ms.sourcegitcommit: e45f8912ce32b4071bf7e83b8f8315cd8bba3520
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2017
---
# <a name="razor-syntax-for-aspnet-core"></a>Sintassi Razor di ASP.NET Core

Da [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), e [rottura Mullen uguale o Taylor](https://twitter.com/ntaylormullen)

Razor è una sintassi di markup per l'incorporamento di codice basato su server in pagine Web. La sintassi Razor è costituita da Razor markup, c# e HTML. File contenente Razor in genere hanno una *. cshtml* estensione di file.

## <a name="rendering-html"></a>Il rendering di HTML

Il linguaggio Razor predefinito è HTML. Per il rendering HTML dal tag Razor non è diversa dal rendering di HTML da un file HTML. Se si inserisce il markup HTML in un *. cshtml* file Razor, ne viene eseguito il rendering dal server senza modificato.

## <a name="razor-syntax"></a>Sintassi Razor

Razor supporta c# e viene utilizzato il `@` simbolo per la transizione da HTML in c#. Razor valuta le espressioni c# e li visualizza nell'output HTML.

Quando un `@` simbolo è seguito da un [parola chiave riservata Razor](#razor-reserved-keywords), passa nel codice Razor specifico. In caso contrario, esegue la transizione allo semplice in c#.

A escape un `@` simbolo nel codice Razor, usare un secondo `@` simbolo:

```cshtml
<p>@@Username</p>
```

Il codice viene eseguito il rendering in HTML con un singolo `@` simbolo:

```html
<p>@Username</p>
```

Gli attributi HTML e il contenuto che contiene gli indirizzi di posta elettronica non considerano il `@` simbolo come un carattere di transizione. Nell'esempio seguente gli indirizzi di posta elettronica sono invariati da Razor l'analisi:

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a>Espressioni implicite Razor

Le espressioni implicite Razor iniziano con `@` seguito dal codice c#:

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

Fatta eccezione per il linguaggio c# `await` (parola chiave), le espressioni implicite non devono contenere spazi. Se l'istruzione c# è un chiaro finale, è possibile intermingle spazi:

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

## <a name="explicit-razor-expressions"></a>Espressioni Razor esplicite

Costituiti da espressioni Razor esplicite un `@` simbolo con parentesi bilanciato. Per visualizzare l'ora dell'ultima settimana, viene utilizzato il markup Razor seguente:

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

Qualsiasi contenuto all'interno di `@()` parentesi viene valutata e il rendering dell'output.

Le espressioni implicite, descritte nella sezione precedente, in genere non possono contenere spazi. Nel codice seguente, una settimana non viene sottratto dall'ora corrente:

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

Il codice viene eseguito il rendering HTML seguente:

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

È possibile utilizzare un'espressione esplicita per la concatenazione di testo con un risultato dell'espressione:

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

Senza l'espressione esplicita, `<p>Age@joe.Age</p>` viene considerato come un indirizzo di posta elettronica e `<p>Age@joe.Age</p>` viene eseguito il rendering. Quando scritto in un'espressione esplicita, `<p>Age33</p>` viene eseguito il rendering.

## <a name="expression-encoding"></a>Codifica di espressione

Le espressioni c# che restituiscono una stringa sono codificati in HTML. Le espressioni c# che restituiscono `IHtmlContent` il rendering viene eseguito direttamente tramite `IHtmlContent.WriteTo`. Le espressioni c# che non restituiscono `IHtmlContent` vengono convertiti in una stringa da `ToString` e codificati prima vengono sottoposti a rendering.

```cshtml
@("<span>Hello World</span>")
```

Il codice viene eseguito il rendering HTML seguente:

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

Il codice HTML viene visualizzato nel browser come:

```
<span>Hello World</span>
```

`HtmlHelper.Raw`output non è codificato ma sottoposto a rendering come markup HTML.

> [!WARNING]
> Utilizzando `HtmlHelper.Raw` utente unsanitized input è un rischio per la sicurezza. L'input dell'utente potrebbe contenere JavaScript dannoso o altri exploit. La purificazione degli input dell'utente è difficile. Evitare di utilizzare `HtmlHelper.Raw` con l'input dell'utente.

```cshtml
@Html.Raw("<span>Hello World</span>")
```

Il codice viene eseguito il rendering HTML seguente:

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a>Blocchi di codice Razor

Blocchi di codice Razor iniziano con `@` e sono racchiusi tra `{}`. A differenza delle espressioni, non viene eseguito il rendering di codice c# all'interno di blocchi di codice. Blocchi di codice e le espressioni in una vista condividono lo stesso ambito e vengono definite nell'ordine:

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

Il codice viene eseguito il rendering HTML seguente:

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a>Transizioni implicite

È la lingua predefinita in un blocco di codice c#, ma è possibile passare al HTML:

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a>Transizione delimitata esplicita

Per definire una sottosezione di un blocco di codice che deve eseguire il rendering HTML, Racchiudi tra i caratteri per il rendering Razor  **\<testo >** tag:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

Utilizzare questo approccio quando si desidera eseguire il rendering di HTML che non è racchiuso tra tag HTML. Senza un tag HTML o Razor, viene visualizzato un errore di runtime Razor.

Il  **\<testo >** tag è utile anche per controllare gli spazi vuoti durante il rendering del contenuto. Solo il contenuto tra il  **\<testo >** viene eseguito il rendering di tag e senza spazi prima o dopo il  **\<testo >** tag viene visualizzato nell'output HTML.

### <a name="explicit-line-transition-with-"></a>Transizione riga esplicita con @:

Per visualizzare il resto di un'intera riga in formato HTML all'interno di un blocco di codice, utilizzare il `@:` sintassi:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

Senza il `@:` nel codice, viene visualizzato un errore di runtime Razor.

## <a name="control-structures"></a>Strutture di controllo

Strutture di controllo sono un'estensione dei blocchi di codice. Tutti gli aspetti dei blocchi di codice (una transizione di markup, c# inline) inoltre si applicano alle seguenti strutture.

### <a name="conditionals-if-else-if-else-and-switch"></a>Istruzioni condizionali @if, altrimenti, else e@switch

`@if`controlli durante l'esecuzione di codice:

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

`else`e `else if` non richiedono la `@` simbolo:

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

È possibile utilizzare un'istruzione switch simile al seguente:

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

### <a name="looping-for-foreach-while-and-do-while"></a>Ciclo @for, @foreach, @while, e @do mentre

È possibile eseguire il rendering HTML basato su modelli con istruzioni di controllo di ciclo. Per eseguire il rendering di un elenco di persone:

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

È possibile utilizzare una delle seguenti istruzioni ciclo:

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

### <a name="compound-using"></a>Composta@using

In c#, un `using` istruzione viene utilizzata per verificare che un oggetto è stato eliminato. In Razor, lo stesso meccanismo viene utilizzato per creare l'helper HTML che contiene contenuto aggiuntivo. Ad esempio, è possibile utilizzare l'helper HTML per eseguire il rendering di un tag form con il `@using` istruzione:

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

È inoltre possibile eseguire azioni a livello di ambito con [gli helper di Tag](xref:mvc/views/tag-helpers/intro).

### <a name="try-catch-finally"></a>@try, catch, finally

Gestione delle eccezioni è simile a c#:

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

Razor è in grado di proteggere le sezioni critiche con le istruzioni di blocco:

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>Commenti

Razor supporta commenti in c# e il codice HTML:

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

Il codice viene eseguito il rendering HTML seguente:

```html
<!-- HTML comment -->
```

Commenti Razor vengono rimossi dal server prima del rendering della pagina Web. Utilizza Razor `@*  *@` per delimitare i commenti. Il codice seguente viene impostato come commento, in modo il server non esegue il rendering di tag:

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

Direttive Razor vengono rappresentate tramite espressioni implicite con seguenti le parole chiave riservate di `@` simbolo. Una direttiva cambia in genere la modalità di una vista viene analizzata o Abilita funzionalità diverse.

Comprendere come Razor genera il codice per una vista, è facile comprendere il funzionano delle direttive.

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

Il codice genera una classe simile al seguente:

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

Più avanti in questo articolo, la sezione [visualizzazione classe Razor c# generato per una vista](#viewing-the-razor-c-class-generated-for-a-view) viene illustrato come visualizzare la classe generata.

### <a name="using"></a>@using

Il `@using` direttiva aggiunge c# `using` direttiva alla visualizzazione generata:

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

Il `@model` direttiva specifica il tipo di modello passato a una visualizzazione:

```cshtml
@model TypeNameOfModel
```

Se si crea un'applicazione ASP.NET MVC di base con account utente individuali, il *Views/Account/Login.cshtml* vista contiene la dichiarazione di modello seguente:

```cshtml
@model LoginViewModel
```

La classe generata eredita da `RazorPage<dynamic>`:

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

Razor espone un `Model` proprietà per l'accesso al modello passato alla visualizzazione:

```cshtml
<div>The Login Email: @Model.Email</div>
```

Il `@model` direttiva specifica il tipo di questa proprietà. Specifica la direttiva di `T` in `RazorPage<T>` che generato classe che la visualizzazione deriva da. Se non si specifica il `@model` direttiva, il `Model` proprietà è di tipo `dynamic`. Il valore del modello viene passato dal controller alla visualizzazione. Vedere [fortemente tipizzate modelli e @model (parola chiave)](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-keyword-label) per ulteriori informazioni.

### <a name="inherits"></a>@inherits

Il `@inherits` direttiva offre il controllo completo della classe che eredita la vista:

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

Di seguito è un tipo di pagina Razor personalizzato:

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

Il `CustomText` viene visualizzato in una vista:

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

Il codice viene eseguito il rendering HTML seguente:

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

Non è possibile utilizzare `@model` e `@inherits` nella stessa visualizzazione. È possibile avere `@inherits` in un *viewimports. cshtml* file che l'importazione nella visualizzazione:

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

Di seguito è riportato un esempio di una visualizzazione fortemente tipizzata:

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

Se "rick@contoso.com" viene passato nel modello, la vista genera il markup HTML seguente:

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject

Il `@inject` direttiva consente di inserire un servizio dal [contenitore dei servizi](xref:fundamentals/dependency-injection) nella visualizzazione. Vedere [Dependency injection nelle viste](xref:mvc/views/dependency-injection) per ulteriori informazioni.

### <a name="functions"></a>@functions

Il `@functions` direttiva consente di aggiungere contenuto a livello di funzione in una visualizzazione:

```cshtml
@functions { // C# Code }
```

Ad esempio:

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

Il codice genera il markup HTML seguente:

```html
<div>From method: Hello</div>
```

Il codice seguente è la classe generata Razor c#:

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

Il `@section` direttiva viene utilizzata in combinazione con il [layout](xref:mvc/views/layout) per abilitare le viste per il rendering del contenuto in diverse parti della pagina HTML. Vedere [sezioni](xref:mvc/views/layout#layout-sections-label) per ulteriori informazioni.

## <a name="tag-helpers"></a>Helper di tag

Esistono tre direttive che riguardano [gli helper di Tag](xref:mvc/views/tag-helpers/intro).

| Direttiva | Funzione |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | Rende disponibili per una vista gli helper di Tag. |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | Rimuove gli helper di Tag aggiunti in precedenza da una vista. |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | Specifica un prefisso di tag per abilitare il supporto di Helper di Tag e rendere esplicito l'utilizzo di Helper di Tag. |

## <a name="razor-reserved-keywords"></a>Parole chiave riservate Razor

### <a name="razor-keywords"></a>Parole chiave Razor

* pagina (richiede ASP.NET Core 2.0 e versioni successive)
* funzioni
* eredita
* modello
* section
* helper (attualmente non supportate da ASP.NET Core)

Parole chiave Razor sono precedute da `@(Razor Keyword)` (ad esempio, `@(functions)`).

### <a name="c-razor-keywords"></a>Parole chiave c# Razor

* case
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

Parole chiave c# Razor devono essere doppio carattere di escape con `@(@C# Razor Keyword)` (ad esempio, `@(@case)`). Il primo `@` ignora il parser Razor. Il secondo `@` ignora il parser del linguaggio c#.

### <a name="reserved-keywords-not-used-by-razor"></a>Parole chiave riservate non utilizzate da Razor

* namespace
* classe

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a>Visualizzazione della classe c# Razor generata per una vista

Aggiungere la classe seguente al progetto ASP.NET MVC di base:

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

Eseguire l'override di `RazorTemplateEngine` aggiunto da MVC con la `CustomTemplateEngine` classe:

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

Impostare un punto di interruzione per il `return csharpDocument` istruzione di `CustomTemplateEngine`. Quando l'esecuzione del programma si interrompe al punto di interruzione, visualizzare il valore di `generatedCode`.

![Visualizzazione di generatedCode Visualizzatore di testo](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a>Le ricerche di visualizzazione e distinzione maiuscole/minuscole

Il motore di visualizzazione Razor esegue ricerche tra maiuscole e minuscole per le viste. Tuttavia, la ricerca effettiva è determinata dal file system sottostante:

* File di origine in base: 
  * Nei sistemi operativi con i sistemi di file tra maiuscole e minuscole (ad esempio, Windows), le ricerche di file fisico provider si trovano tra maiuscole e minuscole. Ad esempio, `return View("Test")` comporta corrispondenze per */Views/Home/Test.cshtml*, */Views/home/test.cshtml*e qualsiasi altra variante di maiuscole e minuscole.
  * Nei sistemi di file tra maiuscole e minuscole (ad esempio Linux, OS x e con `EmbeddedFileProvider`), le ricerche sono tra maiuscole e minuscole. Ad esempio, `return View("Test")` specificamente corrispondenze */Views/Home/Test.cshtml*.
* Precompilata viste: con il Core di ASP.Net 2.0 e versioni successivo, la ricerca di viste precompilate è tra maiuscole e minuscole in tutti i sistemi operativi. Il comportamento è identico al comportamento del provider di file fisico in Windows. Se due visualizzazioni precompilate differiscono solo nel caso, il risultato di ricerca è non deterministico.

Gli sviluppatori sono invitati a corrispondere le maiuscole e minuscole dei nomi di file e directory per le maiuscole e minuscole dei nomi di area, controller e azione. In questo modo che le distribuzioni disponibili le visualizzazioni indipendentemente dal file system sottostante.
