---
title: Riferimento della sintassi Razor per ASP.NET Core
author: rick-anderson
description: Viene descritta la sintassi Razor
keywords: ASP.NET Core, Razor
ms.author: riande
manager: wpickett
ms.date: 07/04/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: 066fe3b2486c63bd4de2ccb865ad432a67846d77
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2017
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="49413-104">Sintassi Razor di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="49413-104">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="49413-105">Da [rottura Mullen uguale o Taylor](https://twitter.com/ntaylormullen) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="49413-105">By [Taylor Mullen](https://twitter.com/ntaylormullen) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-is-razor"></a><span data-ttu-id="49413-106">Che cos'è Razor?</span><span class="sxs-lookup"><span data-stu-id="49413-106">What is Razor?</span></span>

<span data-ttu-id="49413-107">Razor è una sintassi di markup per l'incorporamento di codice basato su server nelle pagine web.</span><span class="sxs-lookup"><span data-stu-id="49413-107">Razor is a markup syntax for embedding server based code into web pages.</span></span> <span data-ttu-id="49413-108">La sintassi Razor è costituita da markup Razor, c# e HTML.</span><span class="sxs-lookup"><span data-stu-id="49413-108">The Razor syntax consists of Razor markup, C# and HTML.</span></span> <span data-ttu-id="49413-109">File contenente Razor in genere hanno una *. cshtml* estensione di file.</span><span class="sxs-lookup"><span data-stu-id="49413-109">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="49413-110">Il rendering di HTML</span><span class="sxs-lookup"><span data-stu-id="49413-110">Rendering HTML</span></span>

<span data-ttu-id="49413-111">Il linguaggio Razor predefinito è HTML.</span><span class="sxs-lookup"><span data-stu-id="49413-111">The default Razor language is HTML.</span></span> <span data-ttu-id="49413-112">Il rendering di HTML da Razor non è diverso rispetto a un file HTML.</span><span class="sxs-lookup"><span data-stu-id="49413-112">Rendering HTML from Razor is no different than in an HTML file.</span></span> <span data-ttu-id="49413-113">Un file Razor con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="49413-113">A Razor file with the following markup:</span></span>

```html
<p>Hello World</p>
   ```

<span data-ttu-id="49413-114">Viene eseguito il rendering senza modifiche come `<p>Hello World</p>` dal server.</span><span class="sxs-lookup"><span data-stu-id="49413-114">Is rendered unchanged as `<p>Hello World</p>` by the server.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="49413-115">Sintassi Razor</span><span class="sxs-lookup"><span data-stu-id="49413-115">Razor syntax</span></span>

<span data-ttu-id="49413-116">Razor supporta c# e viene utilizzato il `@` simbolo per la transizione da HTML in c#.</span><span class="sxs-lookup"><span data-stu-id="49413-116">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="49413-117">Razor valuta le espressioni c# e li visualizza nell'output HTML.</span><span class="sxs-lookup"><span data-stu-id="49413-117">Razor evaluates C# expressions and renders them in the HTML output.</span></span> <span data-ttu-id="49413-118">Con Razor è possibile passare da HTML a C# o scegliere un markup specifico per Razor.</span><span class="sxs-lookup"><span data-stu-id="49413-118">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="49413-119">Quando un `@` simbolo è seguito da un [parola chiave riservata Razor](#razor-reserved-keywords) passa nel codice Razor specifico, in caso contrario esegue la transizione allo semplice in c#.</span><span class="sxs-lookup"><span data-stu-id="49413-119">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords) it transitions into Razor-specific markup, otherwise it transitions into plain C#.</span></span>

<a name=escape-at-label></a>

<span data-ttu-id="49413-120">HTML contenente `@` simboli potrebbe essere necessario utilizzare caratteri di escape con un secondo `@` simbolo.</span><span class="sxs-lookup"><span data-stu-id="49413-120">HTML containing `@` symbols may need to be escaped with a second `@` symbol.</span></span> <span data-ttu-id="49413-121">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="49413-121">For example:</span></span>

```html
<p>@@Username</p>
   ```

<span data-ttu-id="49413-122">renderebbe il codice HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="49413-122">would render the following HTML:</span></span>

```html
<p>@Username</p>
   ```

<a name=razor-email-ref></a>

<span data-ttu-id="49413-123">Gli attributi HTML e il contenuto che contiene gli indirizzi di posta elettronica non considerano il `@` simbolo come un carattere di transizione.</span><span class="sxs-lookup"><span data-stu-id="49413-123">HTML attributes and content containing email addresses don’t treat the `@` symbol as a transition character.</span></span>

   `<a href="mailto:Support@contoso.com">Support@contoso.com</a>`

## <a name="implicit-razor-expressions"></a><span data-ttu-id="49413-124">Espressioni implicite Razor</span><span class="sxs-lookup"><span data-stu-id="49413-124">Implicit Razor expressions</span></span>

<span data-ttu-id="49413-125">Le espressioni implicite Razor iniziano con `@` seguito dal codice c#.</span><span class="sxs-lookup"><span data-stu-id="49413-125">Implicit Razor expressions start with `@` followed by C# code.</span></span> <span data-ttu-id="49413-126">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="49413-126">For example:</span></span>

```html
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="49413-127">Fatta eccezione per il linguaggio c# `await` espressioni implicite (parola chiave) non devono contenere spazi.</span><span class="sxs-lookup"><span data-stu-id="49413-127">With the exception of the C# `await` keyword implicit expressions must not contain spaces.</span></span> <span data-ttu-id="49413-128">Ad esempio, è possibile intermingle spazi fino a quando l'istruzione c# è un chiaro finale:</span><span class="sxs-lookup"><span data-stu-id="49413-128">For example, you can intermingle spaces as long as the C# statement has a clear ending:</span></span>

```html
<p>@await DoSomething("hello", "world")</p>
```

## <a name="explicit-razor-expressions"></a><span data-ttu-id="49413-129">Espressioni Razor esplicite</span><span class="sxs-lookup"><span data-stu-id="49413-129">Explicit Razor expressions</span></span>

<span data-ttu-id="49413-130">Le espressioni esplicite Razor è costituito da un simbolo con parentesi bilanciato @.</span><span class="sxs-lookup"><span data-stu-id="49413-130">Explicit Razor expressions consists of an @ symbol with balanced parenthesis.</span></span> <span data-ttu-id="49413-131">Ad esempio, per eseguire il rendering ora dell'ultima settimana:</span><span class="sxs-lookup"><span data-stu-id="49413-131">For example, to render last week's time:</span></span>

```html
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="49413-132">Qualsiasi contenuto all'interno di @ parentesi () viene valutata e il rendering dell'output.</span><span class="sxs-lookup"><span data-stu-id="49413-132">Any content within the @() parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="49413-133">Le espressioni implicite, in genere, non possono contenere spazi.</span><span class="sxs-lookup"><span data-stu-id="49413-133">Implicit expressions generally cannot contain spaces.</span></span> <span data-ttu-id="49413-134">Nel codice seguente, ad esempio, una settimana non viene sottratto dall'ora corrente:</span><span class="sxs-lookup"><span data-stu-id="49413-134">For example, in the code below, one week is not subtracted from the current time:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact.cshtml?range=20)]

<span data-ttu-id="49413-135">Che esegue il rendering HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="49413-135">Which renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
   ```

<span data-ttu-id="49413-136">È possibile utilizzare un'espressione esplicita per la concatenazione di testo con un risultato dell'espressione:</span><span class="sxs-lookup"><span data-stu-id="49413-136">You can use an explicit expression to concatenate text with an expression result:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [5]}} -->

```none
@{
    var joe = new Person("Joe", 33);
 }

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="49413-137">Senza l'espressione esplicita, `<p>Age@joe.Age</p>` verrebbe considerato come un indirizzo di posta elettronica e `<p>Age@joe.Age</p>` verrebbe eseguito.</span><span class="sxs-lookup"><span data-stu-id="49413-137">Without the explicit expression, `<p>Age@joe.Age</p>` would be treated as an email address and `<p>Age@joe.Age</p>` would be rendered.</span></span> <span data-ttu-id="49413-138">Quando scritto in un'espressione esplicita, `<p>Age33</p>` viene eseguito il rendering.</span><span class="sxs-lookup"><span data-stu-id="49413-138">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

<a name=expression-encoding-label></a>

## <a name="expression-encoding"></a><span data-ttu-id="49413-139">Codifica di espressione</span><span class="sxs-lookup"><span data-stu-id="49413-139">Expression encoding</span></span>

<span data-ttu-id="49413-140">Le espressioni c# che restituiscono una stringa sono codificati in HTML.</span><span class="sxs-lookup"><span data-stu-id="49413-140">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="49413-141">Le espressioni c# che restituiscono `IHtmlContent` il rendering viene eseguito direttamente tramite *IHtmlContent.WriteTo*.</span><span class="sxs-lookup"><span data-stu-id="49413-141">C# expressions that evaluate to `IHtmlContent` are rendered directly through *IHtmlContent.WriteTo*.</span></span> <span data-ttu-id="49413-142">Le espressioni c# che non restituiscono *IHtmlContent* vengono convertiti in una stringa (da *ToString*) e codificati prima di essere sottoposti a rendering.</span><span class="sxs-lookup"><span data-stu-id="49413-142">C# expressions that don't evaluate to *IHtmlContent* are converted to a string (by *ToString*) and encoded before they are rendered.</span></span> <span data-ttu-id="49413-143">Ad esempio, il markup Razor seguente:</span><span class="sxs-lookup"><span data-stu-id="49413-143">For example, the following Razor markup:</span></span>

```html
@("<span>Hello World</span>")
   ```

<span data-ttu-id="49413-144">Esegue il rendering di questo HTML:</span><span class="sxs-lookup"><span data-stu-id="49413-144">Renders this HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
   ```

<span data-ttu-id="49413-145">Che il browser esegue il rendering come:</span><span class="sxs-lookup"><span data-stu-id="49413-145">Which the browser renders as:</span></span>

`<span>Hello World</span>`

<span data-ttu-id="49413-146">`HtmlHelper``Raw` output non viene codificato ma sottoposto a rendering come markup HTML.</span><span class="sxs-lookup"><span data-stu-id="49413-146">`HtmlHelper` `Raw` output is not encoded but rendered as HTML markup.</span></span>

>[!WARNING]
> <span data-ttu-id="49413-147">Utilizzando `HtmlHelper.Raw` utente unsanitized input è un rischio per la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="49413-147">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="49413-148">L'input dell'utente potrebbe contenere JavaScript dannoso o altri exploit.</span><span class="sxs-lookup"><span data-stu-id="49413-148">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="49413-149">La purificazione degli input dell'utente è difficile, evitare di utilizzare `HtmlHelper.Raw` sull'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="49413-149">Sanitizing user input is difficult, avoid using `HtmlHelper.Raw` on user input.</span></span>

<span data-ttu-id="49413-150">Il markup Razor seguente:</span><span class="sxs-lookup"><span data-stu-id="49413-150">The following Razor markup:</span></span>

```html
@Html.Raw("<span>Hello World</span>")
   ```

<span data-ttu-id="49413-151">Esegue il rendering di questo HTML:</span><span class="sxs-lookup"><span data-stu-id="49413-151">Renders this HTML:</span></span>

```html
<span>Hello World</span>
   ```

<a name=razor-code-blocks-label></a>

## <a name="razor-code-blocks"></a><span data-ttu-id="49413-152">Blocchi di codice Razor</span><span class="sxs-lookup"><span data-stu-id="49413-152">Razor code blocks</span></span>

<span data-ttu-id="49413-153">Blocchi di codice Razor iniziano con `@` e sono racchiusi tra `{}`.</span><span class="sxs-lookup"><span data-stu-id="49413-153">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="49413-154">A differenza delle espressioni, il codice c# all'interno di blocchi di codice non viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="49413-154">Unlike expressions, C# code inside code blocks is not rendered.</span></span> <span data-ttu-id="49413-155">Blocchi di codice e le espressioni in una pagina Razor condividono lo stesso ambito e vengono definite in ordine (vale a dire le dichiarazioni in un blocco di codice sarà nell'ambito di espressioni e blocchi di codice successivi).</span><span class="sxs-lookup"><span data-stu-id="49413-155">Code blocks and expressions in a Razor page share the same scope and are defined in order (that is, declarations in a code block will be in scope for later code blocks and expressions).</span></span>

```none
@{
    var output = "Hello World";
}

<p>The rendered result: @output</p>
```

<span data-ttu-id="49413-156">Il rendering:</span><span class="sxs-lookup"><span data-stu-id="49413-156">Would render:</span></span>

```html
<p>The rendered result: Hello World</p>
   ```

<a name=implicit-transitions-label></a>

### <a name="implicit-transitions"></a><span data-ttu-id="49413-157">Transizioni implicite</span><span class="sxs-lookup"><span data-stu-id="49413-157">Implicit transitions</span></span>

<span data-ttu-id="49413-158">È la lingua predefinita in un blocco di codice c#, ma è possibile passare nuovamente in formato HTML.</span><span class="sxs-lookup"><span data-stu-id="49413-158">The default language in a code block is C#, but you can transition back to HTML.</span></span> <span data-ttu-id="49413-159">HTML all'interno di un blocco di codice passerà nuovamente nel rendering HTML:</span><span class="sxs-lookup"><span data-stu-id="49413-159">HTML within a code block will transition back into rendering HTML:</span></span>

```none
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

<a name=explicit-delimited-transition-label></a>

### <a name="explicit-delimited-transition"></a><span data-ttu-id="49413-160">Transizione delimitata esplicita</span><span class="sxs-lookup"><span data-stu-id="49413-160">Explicit delimited transition</span></span>

<span data-ttu-id="49413-161">Per definire una sottosezione di un blocco di codice che deve eseguire il rendering HTML, racchiudere i caratteri da sottoporre a rendering con Razor `<text>` tag:</span><span class="sxs-lookup"><span data-stu-id="49413-161">To define a sub-section of a code block that should render HTML, surround the characters to be rendered with the Razor `<text>` tag:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [4]}} -->

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="49413-162">In genere possibile utilizzare questo approccio quando si desidera eseguire il rendering di HTML che non è racchiuso tra tag HTML.</span><span class="sxs-lookup"><span data-stu-id="49413-162">You generally use this approach when you want to render HTML that is not surrounded by an HTML tag.</span></span> <span data-ttu-id="49413-163">Senza un tag HTML o Razor, si verifica un errore di runtime Razor.</span><span class="sxs-lookup"><span data-stu-id="49413-163">Without an HTML or Razor tag, you get a Razor runtime error.</span></span>

<a name=explicit-line-transition-with-label></a>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="49413-164">Transizione riga esplicita con`@:`</span><span class="sxs-lookup"><span data-stu-id="49413-164">Explicit Line Transition with `@:`</span></span>

<span data-ttu-id="49413-165">Per visualizzare il resto di un'intera riga in formato HTML all'interno di un blocco di codice, utilizzare il `@:` sintassi:</span><span class="sxs-lookup"><span data-stu-id="49413-165">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [4]}} -->

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="49413-166">Senza il `@:` nel codice precedente, si otterrebbe un Razor errore di runtime.</span><span class="sxs-lookup"><span data-stu-id="49413-166">Without the `@:` in the code above, you'd get a Razor run time error.</span></span>

<a name=control-structures-razor-label></a>

## <a name="control-structures"></a><span data-ttu-id="49413-167">Strutture di controllo</span><span class="sxs-lookup"><span data-stu-id="49413-167">Control Structures</span></span>

<span data-ttu-id="49413-168">Strutture di controllo sono un'estensione dei blocchi di codice.</span><span class="sxs-lookup"><span data-stu-id="49413-168">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="49413-169">Tutti gli aspetti dei blocchi di codice (una transizione di markup, c# inline) inoltre si applicano alle seguenti strutture.</span><span class="sxs-lookup"><span data-stu-id="49413-169">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures.</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="49413-170">Istruzioni condizionali `@if`, `else if`, `else` e`@switch`</span><span class="sxs-lookup"><span data-stu-id="49413-170">Conditionals `@if`, `else if`, `else` and `@switch`</span></span>

<span data-ttu-id="49413-171">Il `@if` famiglia controlla quando viene eseguito codice:</span><span class="sxs-lookup"><span data-stu-id="49413-171">The `@if` family controls when code runs:</span></span>

```none
@if (value % 2 == 0)
{
    <p>The value was even</p>
}
```

<span data-ttu-id="49413-172">`else`e `else if` non richiedono la `@` simbolo:</span><span class="sxs-lookup"><span data-stu-id="49413-172">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="49413-173">È possibile utilizzare un'istruzione switch simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="49413-173">You can use a switch statement like this:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="49413-174">Ciclo `@for`, `@foreach`, `@while`, e`@do while`</span><span class="sxs-lookup"><span data-stu-id="49413-174">Looping `@for`, `@foreach`, `@while`, and `@do while`</span></span>

<span data-ttu-id="49413-175">È possibile eseguire il rendering HTML basato su modelli con istruzioni di controllo di ciclo.</span><span class="sxs-lookup"><span data-stu-id="49413-175">You can render templated HTML with looping control statements.</span></span> <span data-ttu-id="49413-176">Ad esempio, per eseguire il rendering di un elenco di persone:</span><span class="sxs-lookup"><span data-stu-id="49413-176">For example, to render a list of people:</span></span>

```none
@{
    var people = new Person[]
    {
          new Person("John", 33),
          new Person("Doe", 41),
    };
}
```

<span data-ttu-id="49413-177">È possibile utilizzare una delle seguenti istruzioni ciclo:</span><span class="sxs-lookup"><span data-stu-id="49413-177">You can use any of the following looping statements:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="49413-178">Composta`@using`</span><span class="sxs-lookup"><span data-stu-id="49413-178">Compound `@using`</span></span>

<span data-ttu-id="49413-179">In c# un utilizzando istruzione viene utilizzata per verificare che un oggetto è stato eliminato.</span><span class="sxs-lookup"><span data-stu-id="49413-179">In C# a using statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="49413-180">In Razor lo stesso meccanismo è utilizzabile per creare l'helper HTML che contiene contenuto aggiuntivo.</span><span class="sxs-lookup"><span data-stu-id="49413-180">In Razor this same mechanism can be used to create HTML helpers that contain additional content.</span></span> <span data-ttu-id="49413-181">Ad esempio, che possiamo utilizzare l'helper HTML per eseguire il rendering di un tag form con il `@using` istruzione:</span><span class="sxs-lookup"><span data-stu-id="49413-181">For instance, we can utilize HTML Helpers to render a form tag with the `@using` statement:</span></span>

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

<span data-ttu-id="49413-182">È inoltre possibile eseguire azioni a livello di ambito come il precedente con [gli helper di Tag](tag-helpers/index.md).</span><span class="sxs-lookup"><span data-stu-id="49413-182">You can also perform scope level actions like the above with [Tag Helpers](tag-helpers/index.md).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="49413-183">`@try`, `catch`, `finally`</span><span class="sxs-lookup"><span data-stu-id="49413-183">`@try`, `catch`, `finally`</span></span>

<span data-ttu-id="49413-184">Gestione delle eccezioni è simile a c#:</span><span class="sxs-lookup"><span data-stu-id="49413-184">Exception handling is similar to  C#:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact7.cshtml)]

### `@lock`

<span data-ttu-id="49413-185">Razor è in grado di proteggere le sezioni critiche con le istruzioni di blocco:</span><span class="sxs-lookup"><span data-stu-id="49413-185">Razor has the capability to protect critical sections with lock statements:</span></span>

```none
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="49413-186">Commenti</span><span class="sxs-lookup"><span data-stu-id="49413-186">Comments</span></span>

<span data-ttu-id="49413-187">Razor supporta commenti in c# e HTML.</span><span class="sxs-lookup"><span data-stu-id="49413-187">Razor supports C# and HTML comments.</span></span> <span data-ttu-id="49413-188">Il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="49413-188">The following markup:</span></span>

```none
@{
    /* C# comment. */
    // Another C# comment.
}
<!-- HTML comment -->
```

<span data-ttu-id="49413-189">Viene eseguito il rendering dal server come:</span><span class="sxs-lookup"><span data-stu-id="49413-189">Is rendered by the server as:</span></span>

```none
<!-- HTML comment -->
```

<span data-ttu-id="49413-190">Commenti Razor vengono rimossi dal server prima del rendering della pagina.</span><span class="sxs-lookup"><span data-stu-id="49413-190">Razor comments are removed by the server before the page is rendered.</span></span> <span data-ttu-id="49413-191">Utilizza Razor `@*  *@` per delimitare i commenti.</span><span class="sxs-lookup"><span data-stu-id="49413-191">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="49413-192">Il codice seguente viene impostato come commento, in modo il server non eseguirà il rendering di tag:</span><span class="sxs-lookup"><span data-stu-id="49413-192">The following code is commented out, so the server will not render any markup:</span></span>

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

## <a name="directives"></a><span data-ttu-id="49413-193">Direttive</span><span class="sxs-lookup"><span data-stu-id="49413-193">Directives</span></span>

<span data-ttu-id="49413-194">Direttive Razor vengono rappresentate tramite espressioni implicite con seguenti le parole chiave riservate di `@` simbolo.</span><span class="sxs-lookup"><span data-stu-id="49413-194">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="49413-195">Una direttiva verrà in genere modificare il modo in cui che viene analizzata una pagina o abilitare la funzionalità diverse all'interno della pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="49413-195">A directive will typically change the way a page is parsed or enable different functionality within your Razor page.</span></span>

<span data-ttu-id="49413-196">Informazioni sulle modalità Razor genera il codice per una vista renderà più facile da comprendere il funzionano delle direttive.</span><span class="sxs-lookup"><span data-stu-id="49413-196">Understanding how Razor generates code for a view will make it easier to understand how directives work.</span></span> <span data-ttu-id="49413-197">Una pagina Razor viene utilizzata per generare un file c#.</span><span class="sxs-lookup"><span data-stu-id="49413-197">A Razor page is used to generate a C# file.</span></span> <span data-ttu-id="49413-198">Ad esempio, questa pagina Razor:</span><span class="sxs-lookup"><span data-stu-id="49413-198">For example, this Razor page:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="49413-199">Genera una classe simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="49413-199">Generates a class similar to the following:</span></span>

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

<span data-ttu-id="49413-200">[Visualizzazione della classe c# Razor generata per una vista](#razor-customcompilationservice-label) viene illustrato come visualizzare la classe generata.</span><span class="sxs-lookup"><span data-stu-id="49413-200">[Viewing the Razor C# class generated for a view](#razor-customcompilationservice-label) explains how to view this generated class.</span></span>

### `@using`

<span data-ttu-id="49413-201">Il `@using` direttiva aggiungerà c# `using` alla pagina razor generato:</span><span class="sxs-lookup"><span data-stu-id="49413-201">The `@using` directive will add the c# `using` directive to the generated razor page:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact9.cshtml)]

### `@model`

<span data-ttu-id="49413-202">Il `@model` direttiva specifica il tipo di modello passato alla pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="49413-202">The `@model` directive specifies the type of the model passed to the Razor page.</span></span> <span data-ttu-id="49413-203">Viene usata la sintassi seguente:</span><span class="sxs-lookup"><span data-stu-id="49413-203">It uses the following syntax:</span></span>

```none
@model TypeNameOfModel
   ```

<span data-ttu-id="49413-204">Se si crea un'applicazione ASP.NET MVC di base con l'account utente individuali, ad esempio il *Views/Account/Login.cshtml* visualizzazione Razor contiene la dichiarazione di modello seguente:</span><span class="sxs-lookup"><span data-stu-id="49413-204">For example, if you create an ASP.NET Core MVC app with individual user accounts, the *Views/Account/Login.cshtml* Razor view contains the following model declaration:</span></span>

```csharp
@model LoginViewModel
   ```

<span data-ttu-id="49413-205">Nell'esempio precedente, classe eredita la classe generata `RazorPage<dynamic>`.</span><span class="sxs-lookup"><span data-stu-id="49413-205">In the preceding class example, the class generated inherits from `RazorPage<dynamic>`.</span></span> <span data-ttu-id="49413-206">Aggiungendo un `@model` è possibile controllare ciò che viene ereditata.</span><span class="sxs-lookup"><span data-stu-id="49413-206">By adding an `@model` you control what’s inherited.</span></span> <span data-ttu-id="49413-207">Esempio:</span><span class="sxs-lookup"><span data-stu-id="49413-207">For example</span></span>

```csharp
@model LoginViewModel
   ```

<span data-ttu-id="49413-208">La classe seguente genera l'errore</span><span class="sxs-lookup"><span data-stu-id="49413-208">Generates the following class</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
   ```

<span data-ttu-id="49413-209">Pagine Razor espongono un `Model` proprietà per l'accesso al modello passato alla pagina.</span><span class="sxs-lookup"><span data-stu-id="49413-209">Razor pages expose a `Model` property for accessing the model passed to the page.</span></span>

```html
<div>The Login Email: @Model.Email</div>
   ```

<span data-ttu-id="49413-210">Il `@model` direttiva specificato il tipo di questa proprietà (specificando il `T` in `RazorPage<T>` da cui deriva la classe generata per pagina).</span><span class="sxs-lookup"><span data-stu-id="49413-210">The `@model` directive specified the type of this property (by specifying the `T` in `RazorPage<T>` that the generated class for your page derives from).</span></span> <span data-ttu-id="49413-211">Se non si specifica il `@model` direttiva di `Model` proprietà è di tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="49413-211">If you don't specify the `@model` directive the `Model` property will be of type `dynamic`.</span></span> <span data-ttu-id="49413-212">Il valore del modello viene passato dal controller alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="49413-212">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="49413-213">Vedere [fortemente tipizzate modelli e @model (parola chiave)](../../tutorials/first-mvc-app/adding-model.md#strongly-typed-models-keyword-label) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="49413-213">See [Strongly typed models and the @model keyword](../../tutorials/first-mvc-app/adding-model.md#strongly-typed-models-keyword-label) for more information.</span></span>

### `@inherits`

<span data-ttu-id="49413-214">Il `@inherits` direttiva offre il controllo completo della classe che eredita la pagina Razor:</span><span class="sxs-lookup"><span data-stu-id="49413-214">The `@inherits` directive gives you full control of the class your Razor page inherits:</span></span>

```none
@inherits TypeNameOfClassToInheritFrom
   ```

<span data-ttu-id="49413-215">Supponiamo, ad esempio, che si è verificato il seguente tipo di pagina Razor personalizzato:</span><span class="sxs-lookup"><span data-stu-id="49413-215">For instance, let’s say we had the following custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="49413-216">Genera il seguente Razor `<div>Custom text: Hello World</div>`.</span><span class="sxs-lookup"><span data-stu-id="49413-216">The following Razor would generate `<div>Custom text: Hello World</div>`.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="49413-217">Non è possibile utilizzare `@model` e `@inherits` nella stessa pagina.</span><span class="sxs-lookup"><span data-stu-id="49413-217">You can't use `@model` and `@inherits` on the same page.</span></span> <span data-ttu-id="49413-218">È possibile avere `@inherits` in un *viewimports. cshtml* file che importa la pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="49413-218">You can have `@inherits` in a *_ViewImports.cshtml* file that the Razor page imports.</span></span> <span data-ttu-id="49413-219">Ad esempio, se la visualizzazione Razor importati seguenti *viewimports. cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="49413-219">For example, if your Razor view imported the following *_ViewImports.cshtml* file:</span></span>

[!code-html[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="49413-220">La seguente pagina Razor fortemente tipizzata</span><span class="sxs-lookup"><span data-stu-id="49413-220">The following strongly typed Razor page</span></span>

[!code-html[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="49413-221">Genera il markup HTML:</span><span class="sxs-lookup"><span data-stu-id="49413-221">Generates this HTML markup:</span></span>

```none
<div>The Login Email: Rick@contoso.com</div>
<div>Custom text: Hello World</div>
```

<span data-ttu-id="49413-222">Quando viene passato "[Rick@contoso.com](mailto:Rick@contoso.com)" nel modello:</span><span class="sxs-lookup"><span data-stu-id="49413-222">When passed "[Rick@contoso.com](mailto:Rick@contoso.com)" in the model:</span></span>

   <span data-ttu-id="49413-223">Vedere [Layout](layout.md) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="49413-223">See [Layout](layout.md) for more information.</span></span>

### `@inject`

<span data-ttu-id="49413-224">Il `@inject` direttiva consente di inserire un servizio dal [contenitore dei servizi](../../fundamentals/dependency-injection.md) nella pagina Razor per l'utilizzo.</span><span class="sxs-lookup"><span data-stu-id="49413-224">The `@inject` directive enables you to inject a service from your [service container](../../fundamentals/dependency-injection.md)  into your Razor page for use.</span></span> <span data-ttu-id="49413-225">Vedere [Dependency injection nelle viste](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="49413-225">See [Dependency injection into views](dependency-injection.md).</span></span>

<a name="functions"></a>

### `@functions`

<span data-ttu-id="49413-226">Il `@functions` direttiva consente di aggiungere il contenuto a livello di funzione Razor.</span><span class="sxs-lookup"><span data-stu-id="49413-226">The `@functions` directive enables you to add function level content to your Razor page.</span></span> <span data-ttu-id="49413-227">La sintassi è:</span><span class="sxs-lookup"><span data-stu-id="49413-227">The syntax is:</span></span>

```none
@functions { // C# Code }
   ```

<span data-ttu-id="49413-228">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="49413-228">For example:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="49413-229">Genera il markup HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="49413-229">Generates the following HTML markup:</span></span>

```none
<div>From method: Hello</div>
   ```

<span data-ttu-id="49413-230">Generato Razor c# è simile a:</span><span class="sxs-lookup"><span data-stu-id="49413-230">The generated Razor C# looks like:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### `@section`

<span data-ttu-id="49413-231">Il `@section` direttiva viene utilizzata in combinazione con il [pagina layout](layout.md) per abilitare le viste per il rendering del contenuto in parti diverse della pagina HTML di cui è stato eseguito rendering.</span><span class="sxs-lookup"><span data-stu-id="49413-231">The `@section` directive is used in conjunction with the [layout page](layout.md) to enable views to render content in different parts of the rendered HTML page.</span></span> <span data-ttu-id="49413-232">Vedere [sezioni](layout.md#layout-sections-label) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="49413-232">See [Sections](layout.md#layout-sections-label) for more information.</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="49413-233">Helper di tag</span><span class="sxs-lookup"><span data-stu-id="49413-233">Tag Helpers</span></span>

<span data-ttu-id="49413-234">Nell'esempio [gli helper di Tag](tag-helpers/index.md) direttive sono descritte in dettaglio i collegamenti forniti.</span><span class="sxs-lookup"><span data-stu-id="49413-234">The following [Tag Helpers](tag-helpers/index.md) directives are detailed in the links provided.</span></span>

* [@addTagHelper](tag-helpers/intro.md#add-helper-label)
* [@removeTagHelper](tag-helpers/intro.md#remove-razor-directives-label)
* [@tagHelperPrefix](tag-helpers/intro.md#prefix-razor-directives-label)

<a name=razor-reserved-keywords-label></a>

## <a name="razor-reserved-keywords"></a><span data-ttu-id="49413-235">Parole chiave riservate Razor</span><span class="sxs-lookup"><span data-stu-id="49413-235">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="49413-236">Parole chiave Razor</span><span class="sxs-lookup"><span data-stu-id="49413-236">Razor keywords</span></span>

* <span data-ttu-id="49413-237">pagina (richiede ASP.NET Core 2.0 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="49413-237">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="49413-238">funzioni</span><span class="sxs-lookup"><span data-stu-id="49413-238">functions</span></span>
* <span data-ttu-id="49413-239">eredita</span><span class="sxs-lookup"><span data-stu-id="49413-239">inherits</span></span>
* <span data-ttu-id="49413-240">modello</span><span class="sxs-lookup"><span data-stu-id="49413-240">model</span></span>
* <span data-ttu-id="49413-241">section</span><span class="sxs-lookup"><span data-stu-id="49413-241">section</span></span>
* <span data-ttu-id="49413-242">helper (non supportato da ASP.NET Core.)</span><span class="sxs-lookup"><span data-stu-id="49413-242">helper   (Not supported by ASP.NET Core.)</span></span>

<span data-ttu-id="49413-243">Parole chiave Razor possono essere applicata anche con `@(Razor Keyword)`, ad esempio `@(functions)`.</span><span class="sxs-lookup"><span data-stu-id="49413-243">Razor keywords can be escaped with `@(Razor Keyword)`, for example `@(functions)`.</span></span> <span data-ttu-id="49413-244">Vedere l'esempio completo seguente.</span><span class="sxs-lookup"><span data-stu-id="49413-244">See the complete sample below.</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="49413-245">Parole chiave c# Razor</span><span class="sxs-lookup"><span data-stu-id="49413-245">C# Razor keywords</span></span>

* <span data-ttu-id="49413-246">case</span><span class="sxs-lookup"><span data-stu-id="49413-246">case</span></span>
* <span data-ttu-id="49413-247">do</span><span class="sxs-lookup"><span data-stu-id="49413-247">do</span></span>
* <span data-ttu-id="49413-248">default</span><span class="sxs-lookup"><span data-stu-id="49413-248">default</span></span>
* <span data-ttu-id="49413-249">for</span><span class="sxs-lookup"><span data-stu-id="49413-249">for</span></span>
* <span data-ttu-id="49413-250">foreach</span><span class="sxs-lookup"><span data-stu-id="49413-250">foreach</span></span>
* <span data-ttu-id="49413-251">if</span><span class="sxs-lookup"><span data-stu-id="49413-251">if</span></span>
* <span data-ttu-id="49413-252">else</span><span class="sxs-lookup"><span data-stu-id="49413-252">else</span></span>
* <span data-ttu-id="49413-253">blocco</span><span class="sxs-lookup"><span data-stu-id="49413-253">lock</span></span>
* <span data-ttu-id="49413-254">switch</span><span class="sxs-lookup"><span data-stu-id="49413-254">switch</span></span>
* <span data-ttu-id="49413-255">try</span><span class="sxs-lookup"><span data-stu-id="49413-255">try</span></span>
* <span data-ttu-id="49413-256">catch</span><span class="sxs-lookup"><span data-stu-id="49413-256">catch</span></span>
* <span data-ttu-id="49413-257">finally</span><span class="sxs-lookup"><span data-stu-id="49413-257">finally</span></span>
* <span data-ttu-id="49413-258">utilizzo</span><span class="sxs-lookup"><span data-stu-id="49413-258">using</span></span>
* <span data-ttu-id="49413-259">while</span><span class="sxs-lookup"><span data-stu-id="49413-259">while</span></span>

<span data-ttu-id="49413-260">Parole chiave c# Razor devono essere doppie precedute `@(@C# Razor Keyword)`, ad esempio `@(@case)`.</span><span class="sxs-lookup"><span data-stu-id="49413-260">C# Razor keywords need to be double escaped with `@(@C# Razor Keyword)`, for example `@(@case)`.</span></span> <span data-ttu-id="49413-261">Il primo `@` ignora il parser Razor, il secondo `@` ignora il parser del linguaggio c#.</span><span class="sxs-lookup"><span data-stu-id="49413-261">The first `@` escapes the Razor parser, the second `@` escapes the C# parser.</span></span> <span data-ttu-id="49413-262">Vedere l'esempio completo seguente.</span><span class="sxs-lookup"><span data-stu-id="49413-262">See the complete sample below.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="49413-263">Parole chiave riservate non utilizzate da Razor</span><span class="sxs-lookup"><span data-stu-id="49413-263">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="49413-264">namespace</span><span class="sxs-lookup"><span data-stu-id="49413-264">namespace</span></span>
* <span data-ttu-id="49413-265">classe</span><span class="sxs-lookup"><span data-stu-id="49413-265">class</span></span>

<a name=razor-customcompilationservice-label></a>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="49413-266">Visualizzazione della classe c# Razor generata per una vista</span><span class="sxs-lookup"><span data-stu-id="49413-266">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="49413-267">Aggiungere la classe seguente al progetto ASP.NET MVC di base:</span><span class="sxs-lookup"><span data-stu-id="49413-267">Add the following class to your ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Services/CustomCompilationService.cs)]

<span data-ttu-id="49413-268">Eseguire l'override di `ICompilationService` aggiunti da MVC con la classe precedente.</span><span class="sxs-lookup"><span data-stu-id="49413-268">Override the `ICompilationService` added by MVC with the above class;</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=29-33)]

<span data-ttu-id="49413-269">Impostare un punto di interruzione per il `Compile` metodo `CustomCompilationService` e visualizzazione `compilationContent`.</span><span class="sxs-lookup"><span data-stu-id="49413-269">Set a break point on the `Compile` method of `CustomCompilationService` and view `compilationContent`.</span></span>

![Visualizzazione di compilationContent Visualizzatore di testo](razor/_static/tvr.png)

<a name="case"></a>
## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="49413-271">Le ricerche di visualizzazione e distinzione maiuscole/minuscole</span><span class="sxs-lookup"><span data-stu-id="49413-271">View lookups and case sensitivity</span></span>

<span data-ttu-id="49413-272">Il motore di visualizzazione Razor esegue ricerche tra maiuscole e minuscole per le viste.</span><span class="sxs-lookup"><span data-stu-id="49413-272">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="49413-273">Tuttavia, la ricerca effettiva è determinata dall'origine sottostante:</span><span class="sxs-lookup"><span data-stu-id="49413-273">However, the actual lookup is determined by the underlying source:</span></span>

* <span data-ttu-id="49413-274">File di origine in base:</span><span class="sxs-lookup"><span data-stu-id="49413-274">File based source:</span></span> 

    * <span data-ttu-id="49413-275">Nei sistemi operativi con sistemi di file tra maiuscole e minuscole (ad esempio Windows), le ricerche di file fisico provider si trovano tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="49413-275">On operating systems with case insensitive file systems (like Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="49413-276">Ad esempio `return View("Test")` comporterebbe `/Views/Home/Test.cshtml`, `/Views/home/test.cshtml` e tutte le altre varianti di maiuscole e minuscole verrebbero individuate.</span><span class="sxs-lookup"><span data-stu-id="49413-276">For example `return View("Test")` would result in `/Views/Home/Test.cshtml`, `/Views/home/test.cshtml` and all other casing variants would be discovered.</span></span>
    * <span data-ttu-id="49413-277">Nei sistemi di file tra maiuscole e minuscole, che include Linux, OSX e `EmbeddedFileProvider`, le ricerche sono tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="49413-277">On case sensitive file systems, which includes Linux, OSX and `EmbeddedFileProvider`, lookups are case sensitive.</span></span> <span data-ttu-id="49413-278">Ad esempio, `return View("Test")` specificamente cercherebbe `/Views/Home/Test.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="49413-278">For example, `return View("Test")` would specifically look for `/Views/Home/Test.cshtml`.</span></span>
        
* <span data-ttu-id="49413-279">Precompilata visualizzazioni:</span><span class="sxs-lookup"><span data-stu-id="49413-279">Precompiled views:</span></span>

   * <span data-ttu-id="49413-280">Componenti di base di ASP.Net 2.0 e versioni successive, la ricerca di viste precompilate è tra maiuscole e minuscole in tutti i sistemi operativi.</span><span class="sxs-lookup"><span data-stu-id="49413-280">With ASP.Net Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="49413-281">Il comportamento è identico al comportamento del provider di file fisico in Windows.</span><span class="sxs-lookup"><span data-stu-id="49413-281">The behavior is identical to physical file provider's behavior on Windows.</span></span> 
   <span data-ttu-id="49413-282">Nota: Se due visualizzazioni precompilate differiscono solo nel caso, il risultato di ricerca è non deterministico.</span><span class="sxs-lookup"><span data-stu-id="49413-282">Note: If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="49413-283">Gli sviluppatori sono invitati a corrispondere le maiuscole e minuscole dei nomi di file e directory per le maiuscole e minuscole dei nomi di area, controller e dell'azione.</span><span class="sxs-lookup"><span data-stu-id="49413-283">Developers are encouraged to match the casing of file and directory names to the casing of area, controller and action names.</span></span> <span data-ttu-id="49413-284">In tal modo che le distribuzioni rimangono indipendente del file system sottostante.</span><span class="sxs-lookup"><span data-stu-id="49413-284">This would ensure your deployments remain agnostic of the underlying file system.</span></span>
