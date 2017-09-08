---
title: Riferimento della sintassi Razor per ASP.NET Core
author: rick-anderson
description: Viene descritta la sintassi Razor
keywords: ASP.NET Core, Razor
ms.author: riande
manager: wpickett
ms.date: 07/4/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: 7648bc2ac7b9efd1653725cda749d6cd271bae77
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="razor-syntax-for-aspnet-core"></a>Sintassi Razor di ASP.NET Core

Da [rottura Mullen uguale o Taylor](https://twitter.com/ntaylormullen) e [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-is-razor"></a>Che cos'è Razor?

Razor è una sintassi di markup per l'incorporamento di codice basato su server nelle pagine web. La sintassi Razor è costituita da markup Razor, c# e HTML. File contenente Razor in genere hanno una *. cshtml* estensione di file.

## <a name="rendering-html"></a>Il rendering di HTML

Il linguaggio Razor predefinito è HTML. Il rendering di HTML da Razor non è diverso rispetto a un file HTML. Un file Razor con il markup seguente:

```html
<p>Hello World</p>
   ```

Viene eseguito il rendering senza modifiche come `<p>Hello World</p>` dal server.

## <a name="razor-syntax"></a>Sintassi Razor

Razor supporta c# e viene utilizzato il `@` simbolo per la transizione da HTML in c#. Razor valuta le espressioni c# e li visualizza nell'output HTML. In c# o in codice Razor specifico Razor può eseguire la transizione da HTML. Quando un `@` simbolo è seguito da un [parola chiave riservata Razor](#razor-reserved-keywords) passa nel codice Razor specifico, in caso contrario esegue la transizione allo semplice in c#.

<a name=escape-at-label></a>

HTML contenente `@` simboli potrebbe essere necessario utilizzare caratteri di escape con un secondo `@` simbolo. Ad esempio:

```html
<p>@@Username</p>
   ```

renderebbe il codice HTML seguente:

```html
<p>@Username</p>
   ```

<a name=razor-email-ref></a>

Gli attributi HTML e il contenuto che contiene gli indirizzi di posta elettronica non considerano il `@` simbolo come un carattere di transizione.

   `<a href="mailto:Support@contoso.com">Support@contoso.com</a>`

## <a name="implicit-razor-expressions"></a>Espressioni implicite Razor

Le espressioni implicite Razor iniziano con `@` seguito dal codice c#. Ad esempio:

```html
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

Fatta eccezione per il linguaggio c# `await` espressioni implicite (parola chiave) non devono contenere spazi. Ad esempio, è possibile intermingle spazi fino a quando l'istruzione c# è un chiaro finale:

```html
<p>@await DoSomething("hello", "world")</p>
```

## <a name="explicit-razor-expressions"></a>Espressioni Razor esplicite

Le espressioni esplicite Razor è costituito da un simbolo con parentesi bilanciato @. Ad esempio, per eseguire il rendering ora dell'ultima settimana:

```html
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

Qualsiasi contenuto all'interno di @ parentesi () viene valutata e il rendering dell'output.

Le espressioni implicite, in genere, non possono contenere spazi. Nel codice seguente, ad esempio, una settimana non viene sottratto dall'ora corrente:

[!code-html[Principale](razor/sample/Views/Home/Contact.cshtml?range=20)]

Che esegue il rendering HTML seguente:

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
   ```

È possibile utilizzare un'espressione esplicita per la concatenazione di testo con un risultato dell'espressione:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [5]}} -->

```none
@{
    var joe = new Person("Joe", 33);
 }

<p>Age@(joe.Age)</p>
```

Senza l'espressione esplicita, `<p>Age@joe.Age</p>` verrebbe considerato come un indirizzo di posta elettronica e `<p>Age@joe.Age</p>` verrebbe eseguito. Quando scritto in un'espressione esplicita, `<p>Age33</p>` viene eseguito il rendering.

<a name=expression-encoding-label></a>

## <a name="expression-encoding"></a>Codifica di espressione

Le espressioni c# che restituiscono una stringa sono codificati in HTML. Le espressioni c# che restituiscono `IHtmlContent` il rendering viene eseguito direttamente tramite *IHtmlContent.WriteTo*. Le espressioni c# che non restituiscono *IHtmlContent* vengono convertiti in una stringa (da *ToString*) e codificati prima di essere sottoposti a rendering. Ad esempio, il markup Razor seguente:

```html
@("<span>Hello World</span>")
   ```

Esegue il rendering di questo HTML:

```html
&lt;span&gt;Hello World&lt;/span&gt;
   ```

Che il browser esegue il rendering come:

`<span>Hello World</span>`

`HtmlHelper``Raw` output non viene codificato ma sottoposto a rendering come markup HTML.

>[!WARNING]
> Utilizzando `HtmlHelper.Raw` utente unsanitized input è un rischio per la sicurezza. L'input dell'utente potrebbe contenere JavaScript dannoso o altri exploit. La purificazione degli input dell'utente è difficile, evitare di utilizzare `HtmlHelper.Raw` sull'input dell'utente.

Il markup Razor seguente:

```html
@Html.Raw("<span>Hello World</span>")
   ```

Esegue il rendering di questo HTML:

```html
<span>Hello World</span>
   ```

<a name=razor-code-blocks-label></a>

## <a name="razor-code-blocks"></a>Blocchi di codice Razor

Blocchi di codice Razor iniziano con `@` e sono racchiusi tra `{}`. A differenza delle espressioni, il codice c# all'interno di blocchi di codice non viene eseguito. Blocchi di codice e le espressioni in una pagina Razor condividono lo stesso ambito e vengono definite in ordine (vale a dire le dichiarazioni in un blocco di codice sarà nell'ambito di espressioni e blocchi di codice successivi).

```none
@{
    var output = "Hello World";
}

<p>The rendered result: @output</p>
```

Il rendering:

```html
<p>The rendered result: Hello World</p>
   ```

<a name=implicit-transitions-label></a>

### <a name="implicit-transitions"></a>Transizioni implicite

È la lingua predefinita in un blocco di codice c#, ma è possibile passare nuovamente in formato HTML. HTML all'interno di un blocco di codice passerà nuovamente nel rendering HTML:

```none
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

<a name=explicit-delimited-transition-label></a>

### <a name="explicit-delimited-transition"></a>Transizione delimitata esplicita

Per definire una sottosezione di un blocco di codice che deve eseguire il rendering HTML, racchiudere i caratteri da sottoporre a rendering con Razor `<text>` tag:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [4]}} -->

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

In genere possibile utilizzare questo approccio quando si desidera eseguire il rendering di HTML che non è racchiuso tra tag HTML. Senza un tag HTML o Razor, si verifica un errore di runtime Razor.

<a name=explicit-line-transition-with-label></a>

### <a name="explicit-line-transition-with-"></a>Transizione riga esplicita con`@:`

Per visualizzare il resto di un'intera riga in formato HTML all'interno di un blocco di codice, utilizzare il `@:` sintassi:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [4]}} -->

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

Senza il `@:` nel codice precedente, si otterrebbe un Razor errore di runtime.

<a name=control-structures-razor-label></a>

## <a name="control-structures"></a>Strutture di controllo

Strutture di controllo sono un'estensione dei blocchi di codice. Tutti gli aspetti dei blocchi di codice (una transizione di markup, c# inline) inoltre si applicano alle seguenti strutture.

### <a name="conditionals-if-else-if-else-and-switch"></a>Istruzioni condizionali `@if`, `else if`, `else` e`@switch`

Il `@if` famiglia controlla quando viene eseguito codice:

```none
@if (value % 2 == 0)
{
    <p>The value was even</p>
}
```

`else`e `else if` non richiedono la `@` simbolo:

```none
@if (value % 2 == 0)
{
    <p>The value was even</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value was not large and is odd.</p>
}
```

È possibile utilizzare un'istruzione switch simile al seguente:

```none
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number was not 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a>Ciclo `@for`, `@foreach`, `@while`, e`@do while`

È possibile eseguire il rendering HTML basato su modelli con istruzioni di controllo di ciclo. Ad esempio, per eseguire il rendering di un elenco di persone:

```none
@{
    var people = new Person[]
    {
          new Person("John", 33),
          new Person("Doe", 41),
    };
}
```

È possibile utilizzare una delle seguenti istruzioni ciclo:

`@for`

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```none
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```none
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

```none
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a>Composta`@using`

In c# un utilizzando istruzione viene utilizzata per verificare che un oggetto è stato eliminato. In Razor lo stesso meccanismo è utilizzabile per creare l'helper HTML che contiene contenuto aggiuntivo. Ad esempio, che possiamo utilizzare l'helper HTML per eseguire il rendering di un tag form con il `@using` istruzione:

```none
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" name="Email" value="" />
        <button type="submit"> Register </button>
    </div>
}
```

È inoltre possibile eseguire azioni a livello di ambito come il precedente con [gli helper di Tag](tag-helpers/index.md).

### <a name="try-catch-finally"></a>`@try`, `catch`, `finally`

Gestione delle eccezioni è simile a c#:

[!code-html[Principale](razor/sample/Views/Home/Contact7.cshtml)]

### `@lock`

Razor è in grado di proteggere le sezioni critiche con le istruzioni di blocco:

```none
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>Commenti

Razor supporta commenti in c# e HTML. Il markup seguente:

```none
@{
    /* C# comment. */
    // Another C# comment.
}
<!-- HTML comment -->
```

Viene eseguito il rendering dal server come:

```none
<!-- HTML comment -->
```

Commenti Razor vengono rimossi dal server prima del rendering della pagina. Utilizza Razor `@*  *@` per delimitare i commenti. Il codice seguente viene impostato come commento, in modo il server non eseguirà il rendering di tag:

```none
 @*
 @{
     /* C# comment. */
     // Another C# comment.
 }
 <!-- HTML comment -->
*@
```

<a name=razor-directives-label></a>

## <a name="directives"></a>Direttive

Direttive Razor vengono rappresentate tramite espressioni implicite con seguenti le parole chiave riservate di `@` simbolo. Una direttiva verrà in genere modificare il modo in cui che viene analizzata una pagina o abilitare la funzionalità diverse all'interno della pagina Razor.

Informazioni sulle modalità Razor genera il codice per una vista renderà più facile da comprendere il funzionano delle direttive. Una pagina Razor viene utilizzata per generare un file c#. Ad esempio, questa pagina Razor:

[!code-html[Principale](razor/sample/Views/Home/Contact8.cshtml)]

Genera una classe simile al seguente:

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Hello World";

        WriteLiteral("/r/n<div>Output: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

[Visualizzazione della classe c# Razor generata per una vista](#razor-customcompilationservice-label) viene illustrato come visualizzare la classe generata.

### `@using`

Il `@using` direttiva aggiungerà c# `using` alla pagina razor generato:

[!code-html[Principale](razor/sample/Views/Home/Contact9.cshtml)]

### `@model`

Il `@model` direttiva specifica il tipo di modello passato alla pagina Razor. Viene usata la sintassi seguente:

```none
@model TypeNameOfModel
   ```

Se si crea un'applicazione ASP.NET MVC di base con l'account utente individuali, ad esempio il *Views/Account/Login.cshtml* visualizzazione Razor contiene la dichiarazione di modello seguente:

```csharp
@model LoginViewModel
   ```

Nell'esempio precedente, classe eredita la classe generata `RazorPage<dynamic>`. Aggiungendo un `@model` è possibile controllare ciò che viene ereditata. Esempio:

```csharp
@model LoginViewModel
   ```

La classe seguente genera l'errore

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
   ```

Pagine Razor espongono un `Model` proprietà per l'accesso al modello passato alla pagina.

```html
<div>The Login Email: @Model.Email</div>
   ```

Il `@model` direttiva specificato il tipo di questa proprietà (specificando il `T` in `RazorPage<T>` da cui deriva la classe generata per pagina). Se non si specifica il `@model` direttiva di `Model` proprietà è di tipo `dynamic`. Il valore del modello viene passato dal controller alla visualizzazione. Vedere [fortemente tipizzate modelli e @model (parola chiave)](../../tutorials/first-mvc-app/adding-model.md#strongly-typed-models-keyword-label) per ulteriori informazioni.

### `@inherits`

Il `@inherits` direttiva offre il controllo completo della classe che eredita la pagina Razor:

```none
@inherits TypeNameOfClassToInheritFrom
   ```

Supponiamo, ad esempio, che si è verificato il seguente tipo di pagina Razor personalizzato:

[!code-csharp[Principale](razor/sample/Classes/CustomRazorPage.cs)]

Genera il seguente Razor `<div>Custom text: Hello World</div>`.

[!code-html[Principale](razor/sample/Views/Home/Contact10.cshtml)]

Non è possibile utilizzare `@model` e `@inherits` nella stessa pagina. È possibile avere `@inherits` in un *viewimports. cshtml* file che importa la pagina Razor. Ad esempio, se la visualizzazione Razor importati seguenti *viewimports. cshtml* file:

[!code-html[Principale](razor/sample/Views/_ViewImportsModel.cshtml)]

La seguente pagina Razor fortemente tipizzata

[!code-html[Principale](razor/sample/Views/Home/Login1.cshtml)]

Genera il markup HTML:

```none
<div>The Login Email: Rick@contoso.com</div>
<div>Custom text: Hello World</div>
```

Quando viene passato "[Rick@contoso.com](mailto:Rick@contoso.com)" nel modello:

   Vedere [Layout](layout.md) per ulteriori informazioni.

### `@inject`

Il `@inject` direttiva consente di inserire un servizio dal [contenitore dei servizi](../../fundamentals/dependency-injection.md) nella pagina Razor per l'utilizzo. Vedere [Dependency injection nelle viste](dependency-injection.md).

<a name="functions"></a>

### `@functions`

Il `@functions` direttiva consente di aggiungere il contenuto a livello di funzione Razor. La sintassi è:

```none
@functions { // C# Code }
   ```

Ad esempio:

[!code-html[Principale](razor/sample/Views/Home/Contact6.cshtml)]

Genera il markup HTML seguente:

```none
<div>From method: Hello</div>
   ```

Generato Razor c# è simile a:

[!code-csharp[Principale](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### `@section`

Il `@section` direttiva viene utilizzata in combinazione con il [pagina layout](layout.md) per abilitare le viste per il rendering del contenuto in parti diverse della pagina HTML di cui è stato eseguito rendering. Vedere [sezioni](layout.md#layout-sections-label) per ulteriori informazioni.

## <a name="tag-helpers"></a>Helper di tag

Nell'esempio [gli helper di Tag](tag-helpers/index.md) direttive sono descritte in dettaglio i collegamenti forniti.

* [@addTagHelper](tag-helpers/intro.md#add-helper-label)
* [@removeTagHelper](tag-helpers/intro.md#remove-razor-directives-label)
* [@tagHelperPrefix](tag-helpers/intro.md#prefix-razor-directives-label)

<a name=razor-reserved-keywords-label></a>

## <a name="razor-reserved-keywords"></a>Parole chiave riservate Razor

### <a name="razor-keywords"></a>Parole chiave Razor

* pagina (richiede ASP.NET Core 2.0 e versioni successive)
* funzioni
* eredita
* modello
* section
* helper (non supportato da ASP.NET Core.)

Parole chiave Razor possono essere applicata anche con `@(Razor Keyword)`, ad esempio `@(functions)`. Vedere l'esempio completo seguente.

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

Parole chiave c# Razor devono essere doppie precedute `@(@C# Razor Keyword)`, ad esempio `@(@case)`. Il primo `@` ignora il parser Razor, il secondo `@` ignora il parser del linguaggio c#. Vedere l'esempio completo seguente.

### <a name="reserved-keywords-not-used-by-razor"></a>Parole chiave riservate non utilizzate da Razor

* namespace
* classe

<a name=razor-customcompilationservice-label></a>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a>Visualizzazione della classe c# Razor generata per una vista

Aggiungere la classe seguente al progetto ASP.NET MVC di base:

[!code-csharp[Principale](razor/sample/Services/CustomCompilationService.cs)]

Eseguire l'override di `ICompilationService` aggiunti da MVC con la classe precedente.

[!code-csharp[Principale](razor/sample/Startup.cs?highlight=4&range=29-33)]

Impostare un punto di interruzione per il `Compile` metodo `CustomCompilationService` e visualizzazione `compilationContent`.

![Visualizzazione di compilationContent Visualizzatore di testo](razor/_static/tvr.png)

<a name="case"></a>
## <a name="view-lookups-and-case-sensitivity"></a>Le ricerche di visualizzazione e distinzione maiuscole/minuscole

Il motore di visualizzazione Razor esegue ricerche tra maiuscole e minuscole per le viste. Tuttavia, la ricerca effettiva è determinata dall'origine sottostante:

* File di origine in base: 

    * Nei sistemi operativi con sistemi di file tra maiuscole e minuscole (ad esempio Windows), le ricerche di file fisico provider si trovano tra maiuscole e minuscole. Ad esempio `return View("Test")` comporterebbe `/Views/Home/Test.cshtml`, `/Views/home/test.cshtml` e tutte le altre varianti di maiuscole e minuscole verrebbero individuate.
    * Nei sistemi di file tra maiuscole e minuscole, che include Linux, OSX e `EmbeddedFileProvider`, le ricerche sono tra maiuscole e minuscole. Ad esempio, `return View("Test")` specificamente cercherebbe `/Views/Home/Test.cshtml`.
        
* Precompilata visualizzazioni:

   * Componenti di base di ASP.Net 2.0 e versioni successive, la ricerca di viste precompilate è tra maiuscole e minuscole in tutti i sistemi operativi. Il comportamento è identico al comportamento del provider di file fisico in Windows. 
   Nota: Se due visualizzazioni precompilate differiscono solo nel caso, il risultato di ricerca è non deterministico.

Gli sviluppatori sono invitati a corrispondere le maiuscole e minuscole dei nomi di file e directory per le maiuscole e minuscole dei nomi di area, controller e dell'azione. In tal modo che le distribuzioni rimangono indipendente del file system sottostante.
