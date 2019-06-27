---
title: Guida di riferimento della sintassi Razor per ASP.NET Core
author: rick-anderson
description: Informazioni sulla sintassi di markup Razor per l'incorporamento di codice basato su server in pagine Web.
ms.author: riande
ms.date: 06/12/2019
uid: mvc/views/razor
ms.openlocfilehash: 87c5b97a653c139b8b79f4270e0d9d0081815433
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034933"
---
# <a name="razor-syntax-reference-for-aspnet-core"></a><span data-ttu-id="ef9ae-103">Guida di riferimento della sintassi Razor per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ef9ae-103">Razor syntax reference for ASP.NET Core</span></span>

<span data-ttu-id="ef9ae-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen) e [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="ef9ae-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="ef9ae-105">Razor è una sintassi di markup per l'incorporamento di codice basato su server in pagine Web.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="ef9ae-106">La sintassi Razor è costituita da markup Razor, C# e HTML.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="ef9ae-107">I file contenenti Razor in genere presentano l'estensione *cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-107">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="ef9ae-108">Rendering di HTML</span><span class="sxs-lookup"><span data-stu-id="ef9ae-108">Rendering HTML</span></span>

<span data-ttu-id="ef9ae-109">Il linguaggio Razor predefinito è HTML.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-109">The default Razor language is HTML.</span></span> <span data-ttu-id="ef9ae-110">Il rendering HTML dal markup Razor non è diverso dal rendering HTML da un file HTML.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-110">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="ef9ae-111">Il rendering del markup HTML nei file Razor con estensione *cshtml* viene eseguito dal server senza modifiche.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-111">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="ef9ae-112">Sintassi Razor</span><span class="sxs-lookup"><span data-stu-id="ef9ae-112">Razor syntax</span></span>

<span data-ttu-id="ef9ae-113">Razor supporta C# e usa il simbolo `@` per la transizione da HTML a C#.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-113">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="ef9ae-114">Razor valuta le espressioni C# e ne esegue il rendering nell'output HTML.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-114">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="ef9ae-115">Quando un simbolo `@` è seguito da una [parola chiave riservata Razor](#razor-reserved-keywords), la transizione avviene in un markup specifico per Razor,</span><span class="sxs-lookup"><span data-stu-id="ef9ae-115">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="ef9ae-116">in caso contrario in C# semplice.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-116">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="ef9ae-117">Per usare un simbolo `@` come carattere di escape nel markup Razor, usare un secondo simbolo `@`:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-117">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="ef9ae-118">Il rendering del codice viene eseguito in HTML con un solo simbolo `@`:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-118">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="ef9ae-119">Gli attributi e il contenuto HTML contenenti indirizzi di posta elettronica non considerano il simbolo `@` come un carattere di transizione.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-119">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="ef9ae-120">Gli indirizzi di posta elettronica nell'esempio seguente non vengono modificati dall'analisi Razor:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-120">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="ef9ae-121">Espressioni implicite Razor</span><span class="sxs-lookup"><span data-stu-id="ef9ae-121">Implicit Razor expressions</span></span>

<span data-ttu-id="ef9ae-122">Le espressioni implicite Razor iniziano con `@` seguito dal codice C#:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-122">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="ef9ae-123">Fatta eccezione per la parola chiave `await` di C#, le espressioni implicite non devono contenere spazi.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-123">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="ef9ae-124">Se l'istruzione C# ha una fine chiaramente definita, possono coesistere spazi:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-124">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="ef9ae-125">Le espressioni implicite **non possono** contenere generics C#, poiché i caratteri all'interno delle parentesi (`<>`) vengono interpretati come un tag HTML.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-125">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="ef9ae-126">Il codice seguente **non** è valido:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-126">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="ef9ae-127">Il codice precedente genera un errore del compilatore simile a uno dei seguenti:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-127">The preceding code generates a compiler error similar to one of the following:</span></span>

* <span data-ttu-id="ef9ae-128">L'elemento "int" non è stato chiuso.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-128">The "int" element wasn't closed.</span></span> <span data-ttu-id="ef9ae-129">Tutti gli elementi devono essere a chiusura automatica o avere un tag di fine corrispondente.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-129">All elements must be either self-closing or have a matching end tag.</span></span>
* <span data-ttu-id="ef9ae-130">Non è possibile convertire il gruppo di metodi 'GenericMethod' nel tipo non delegato 'object'.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-130">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="ef9ae-131">Si intendeva chiamare il metodo?'</span><span class="sxs-lookup"><span data-stu-id="ef9ae-131">Did you intend to invoke the method?\`</span></span>

<span data-ttu-id="ef9ae-132">Le chiamate di metodo generiche devono essere racchiuse in un'[espressione esplicita Razor](#explicit-razor-expressions) o in un [blocco di codice Razor](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="ef9ae-132">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="ef9ae-133">Espressioni esplicite Razor</span><span class="sxs-lookup"><span data-stu-id="ef9ae-133">Explicit Razor expressions</span></span>

<span data-ttu-id="ef9ae-134">Le espressioni esplicite Razor sono costituite da un simbolo `@` con parentesi bilanciate.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-134">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="ef9ae-135">Per eseguire il rendering dell'ora dell'ultima settimana, viene usato il markup Razor seguente:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-135">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="ef9ae-136">Qualsiasi contenuto all'interno delle parentesi `@()` viene valutato e sottoposto a rendering nell'output.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-136">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="ef9ae-137">Le espressioni implicite, descritte nella sezione precedente, in genere non possono contenere spazi.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-137">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="ef9ae-138">Nel codice seguente, una settimana non viene sottratta dall'ora corrente:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-138">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="ef9ae-139">Il codice esegue il rendering dell'HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-139">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="ef9ae-140">È possibile usare le espressioni esplicite per concatenare testo con un risultato dell'espressione:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-140">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="ef9ae-141">Senza l'espressione esplicita, `<p>Age@joe.Age</p>` viene considerato come un indirizzo di posta elettronica e `<p>Age@joe.Age</p>` viene sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-141">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="ef9ae-142">Se viene scritto come espressione esplicita, `<p>Age33</p>` viene sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-142">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

<span data-ttu-id="ef9ae-143">È possibile usare le espressioni esplicite per eseguire il rendering dell'output da metodi generici in file con estensione *cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-143">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="ef9ae-144">Il markup seguente illustra come correggere l'errore riportato in precedenza, causato dalle parentesi quadre di un oggetto generico C#.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-144">The following markup shows how to correct the error shown earlier caused by the brackets of a C# generic.</span></span> <span data-ttu-id="ef9ae-145">Il codice viene scritto come un'espressione esplicita:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-145">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="ef9ae-146">Codifica di espressioni</span><span class="sxs-lookup"><span data-stu-id="ef9ae-146">Expression encoding</span></span>

<span data-ttu-id="ef9ae-147">Le espressioni C# che restituiscono una stringa sono codificate in HTML.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-147">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="ef9ae-148">Le espressioni C# che restituiscono `IHtmlContent` vengono sottoposte a rendering direttamente tramite `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-148">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="ef9ae-149">Le espressioni C# che non restituiscono `IHtmlContent` vengono convertite in una stringa da `ToString` e codificate prima di essere sottoposte a rendering.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-149">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="ef9ae-150">Il codice esegue il rendering dell'HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-150">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="ef9ae-151">Il codice HTML viene visualizzato nel browser come:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-151">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="ef9ae-152">L'output `HtmlHelper.Raw` non è codificato ma viene sottoposto a rendering come markup HTML.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-152">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="ef9ae-153">L'uso di `HtmlHelper.Raw` su input utente non purificato costituisce un rischio per la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-153">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="ef9ae-154">L'input utente potrebbe contenere JavaScript dannoso o altri attacchi.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-154">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="ef9ae-155">La purificazione degli input utente è difficile.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-155">Sanitizing user input is difficult.</span></span> <span data-ttu-id="ef9ae-156">Evitare l'uso di `HtmlHelper.Raw` con l'input utente.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-156">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="ef9ae-157">Il codice esegue il rendering dell'HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-157">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="ef9ae-158">Blocchi di codice Razor</span><span class="sxs-lookup"><span data-stu-id="ef9ae-158">Razor code blocks</span></span>

<span data-ttu-id="ef9ae-159">I blocchi di codice Razor iniziano con `@` e sono racchiusi tra `{}`.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-159">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="ef9ae-160">A differenza delle espressioni, il codice C# all'interno di blocchi di codice non viene sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-160">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="ef9ae-161">I blocchi di codice e le espressioni in una visualizzazione condividono lo stesso ambito e vengono definiti in ordine:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-161">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="ef9ae-162">Il codice esegue il rendering dell'HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-162">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ef9ae-163">Nei blocchi di codice dichiarare [funzioni locali](/dotnet/csharp/programming-guide/classes-and-structs/local-functions) con markup da usare come metodi per la creazione di modelli:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-163">In code blocks, declare [local functions](/dotnet/csharp/programming-guide/classes-and-structs/local-functions) with markup to serve as templating methods:</span></span>

```cshtml
@{
    void RenderName(string name)
    {
        <p>Name: <strong>@name</strong></p>
    }

    RenderName("Mahatma Gandhi");
    RenderName("Martin Luther King, Jr.");
}
```

<span data-ttu-id="ef9ae-164">Il codice esegue il rendering dell'HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-164">The code renders the following HTML:</span></span>

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

::: moniker-end

### <a name="implicit-transitions"></a><span data-ttu-id="ef9ae-165">Transizioni implicite</span><span class="sxs-lookup"><span data-stu-id="ef9ae-165">Implicit transitions</span></span>

<span data-ttu-id="ef9ae-166">Il linguaggio predefinito in un blocco di codice è C#, ma la pagina Razor può eseguire la transizione a HTML:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-166">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="ef9ae-167">Transizione esplicita delimitata</span><span class="sxs-lookup"><span data-stu-id="ef9ae-167">Explicit delimited transition</span></span>

<span data-ttu-id="ef9ae-168">Per definire una sottosezione di un blocco di codice che deve eseguire il rendering HTML, racchiudere i caratteri per il rendering all'interno del tag Razor **\<text>** :</span><span class="sxs-lookup"><span data-stu-id="ef9ae-168">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="ef9ae-169">Usare questo approccio per eseguire il rendering di HTML che non è racchiuso tra tag HTML.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-169">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="ef9ae-170">Senza un tag HTML o Razor, si verifica un errore di runtime Razor.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-170">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="ef9ae-171">Il tag **\<text>** è utile per controllare gli spazi vuoti durante il rendering del contenuto:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-171">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="ef9ae-172">Solo il contenuto tra il tag **\<text>** viene sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-172">Only the content between the **\<text>** tag is rendered.</span></span>
* <span data-ttu-id="ef9ae-173">Non vengono visualizzati spazi vuoti prima o dopo il tag **\<text>** nell'output HTML.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-173">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="ef9ae-174">Transizione riga esplicita con @:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-174">Explicit Line Transition with @:</span></span>

<span data-ttu-id="ef9ae-175">Per eseguire il rendering del resto di un'intera riga come HTML all'interno di un blocco di codice, usare la sintassi `@:`:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-175">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="ef9ae-176">Senza il simbolo `@:` nel codice, viene generato un errore di runtime Razor.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-176">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="ef9ae-177">Avviso: La presenza di caratteri `@` aggiuntivi in un file Razor può causare errori del compilatore nelle istruzioni successive del blocco.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-177">Warning: Extra `@` characters in a Razor file can cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="ef9ae-178">Questi errori del compilatore possono essere difficili da comprendere perché l'errore effettivo si verifica prima dell'errore segnalato.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-178">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="ef9ae-179">Questo errore è comune dopo la combinazione di più espressioni implicite/esplicite in un singolo blocco di codice.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-179">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="ef9ae-180">Strutture di controllo</span><span class="sxs-lookup"><span data-stu-id="ef9ae-180">Control structures</span></span>

<span data-ttu-id="ef9ae-181">Le strutture di controllo sono un'estensione dei blocchi di codice.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-181">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="ef9ae-182">Tutti gli aspetti dei blocchi di codice (transizione al markup, C# inline) sono validi anche per le strutture seguenti:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-182">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="ef9ae-183">Istruzioni condizionali @if, else if, else e @switch</span><span class="sxs-lookup"><span data-stu-id="ef9ae-183">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="ef9ae-184">`@if` controlla quando viene eseguito il codice:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-184">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="ef9ae-185">`else` e `else if` non richiedono il simbolo `@`:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-185">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="ef9ae-186">Nel markup seguente viene illustrato come usare un'istruzione switch:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-186">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="ef9ae-187">Ciclo @for, @foreach, @while, e @do while</span><span class="sxs-lookup"><span data-stu-id="ef9ae-187">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="ef9ae-188">È possibile eseguire il rendering di HTML basato su modelli con le istruzioni di controllo ciclo.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-188">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="ef9ae-189">Per eseguire il rendering di un elenco di persone:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-189">To render a list of people:</span></span>

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

<span data-ttu-id="ef9ae-190">Sono supportate le seguenti istruzioni di ciclo:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-190">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="ef9ae-191">Istruzione @using composita</span><span class="sxs-lookup"><span data-stu-id="ef9ae-191">Compound @using</span></span>

<span data-ttu-id="ef9ae-192">In C# viene usata un'istruzione `using` per verificare che un oggetto sia stato eliminato.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-192">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="ef9ae-193">In Razor lo stesso meccanismo viene usato per creare gli helper HTML che includono contenuto aggiuntivo.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-193">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="ef9ae-194">Nel codice seguente, l'helper HTML esegue il rendering di un tag di modulo con l'istruzione `@using`:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-194">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>

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

<span data-ttu-id="ef9ae-195">Con gli [helper tag](xref:mvc/views/tag-helpers/intro) è possibile eseguire azioni a livello di ambito.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-195">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="ef9ae-196">@try, catch, finally</span><span class="sxs-lookup"><span data-stu-id="ef9ae-196">@try, catch, finally</span></span>

<span data-ttu-id="ef9ae-197">La gestione delle eccezioni è simile a C#:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-197">Exception handling is similar to C#:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="ef9ae-198">Razor è in grado di proteggere le sezioni critiche con le istruzioni di lock:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-198">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="ef9ae-199">Commenti</span><span class="sxs-lookup"><span data-stu-id="ef9ae-199">Comments</span></span>

<span data-ttu-id="ef9ae-200">Razor supporta i commenti in C# e HTML:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-200">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="ef9ae-201">Il codice esegue il rendering dell'HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-201">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="ef9ae-202">I commenti Razor vengono rimossi dal server prima che la pagina Web venga sottoposta a rendering.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-202">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="ef9ae-203">Razor usa `@*  *@` per delimitare i commenti.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-203">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="ef9ae-204">Il codice seguente è commentato, pertanto il server non esegue il rendering di alcun markup:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-204">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="ef9ae-205">Direttive</span><span class="sxs-lookup"><span data-stu-id="ef9ae-205">Directives</span></span>

<span data-ttu-id="ef9ae-206">Le direttive Razor sono rappresentate da espressioni implicite con parole chiave riservate dopo il simbolo `@`.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-206">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="ef9ae-207">Una direttiva cambia in genere il modo in cui viene analizzata una visualizzazione o abilita funzionalità diverse.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-207">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="ef9ae-208">Comprendere come Razor genera il codice per una visualizzazione facilita la comprensione del funzionamento delle direttive.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-208">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="ef9ae-209">Il codice genera una classe simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-209">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="ef9ae-210">La sezione [Ispezionare la classe C# Razor generata per una visualizzazione](#inspect-the-razor-c-class-generated-for-a-view) più avanti in questo articolo illustra come visualizzare la classe generata.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-210">Later in this article, the section [Inspect the Razor C# class generated for a view](#inspect-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

<a name="using"></a>

### <a name="using"></a>@using

<span data-ttu-id="ef9ae-211">La direttiva `@using` aggiunge la direttiva C# `using` alla visualizzazione generata:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-211">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="ef9ae-212">La direttiva `@model` specifica il tipo del modello passato a una visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-212">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="ef9ae-213">In un'app ASP.NET Core MVC creata con account utente individuali, la visualizzazione *Views/Account/Login.cshtml* contiene la dichiarazione di modello seguente:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-213">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="ef9ae-214">La classe generata eredita da `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-214">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="ef9ae-215">Razor espone una proprietà `Model` per l'accesso al modello passato alla visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-215">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="ef9ae-216">La direttiva `@model` specifica il tipo di questa proprietà.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-216">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="ef9ae-217">La direttiva specifica l'elemento `T` in `RazorPage<T>` che ha generato la classe da cui deriva la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-217">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="ef9ae-218">Se la direttiva `@model` non è specificata, la proprietà `Model` è di tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-218">If the `@model` directive isn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="ef9ae-219">Il valore del modello viene passato dal controller alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-219">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="ef9ae-220">Per altre informazioni, vedere [Modelli fortemente tipizzati e parola chiave &commat;model](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span><span class="sxs-lookup"><span data-stu-id="ef9ae-220">For more information, see [Strongly typed models and the &commat;model keyword](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="ef9ae-221">La direttiva `@inherits` offre il controllo completo della classe che viene ereditata dalla visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-221">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="ef9ae-222">Il codice seguente è un tipo di pagina Razor personalizzato:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-222">The following code is a custom Razor page type:</span></span>

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="ef9ae-223">L'elemento `CustomText` viene inserito in una visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-223">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="ef9ae-224">Il codice esegue il rendering dell'HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-224">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="ef9ae-225">È possibile usare `@model` e `@inherits` nella stessa visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-225">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="ef9ae-226">`@inherits` può trovarsi in un file *_ViewImports.cshtml* che viene importato dalla visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-226">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="ef9ae-227">Il codice seguente è un esempio di visualizzazione fortemente tipizzata:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-227">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="ef9ae-228">Se "rick@contoso.com" viene passato nel modello, la visualizzazione genera il markup HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-228">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject

<span data-ttu-id="ef9ae-229">La direttiva `@inject` consente alla pagina Razor di inserire un servizio dal [contenitore di servizi](xref:fundamentals/dependency-injection) in una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-229">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="ef9ae-230">Per altre informazioni, vedere [Inserimento di dipendenze in visualizzazioni](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ef9ae-230">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="ef9ae-231">La direttiva `@functions` consente a una pagina Razor di aggiungere un blocco di codice C# a una visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-231">The `@functions` directive enables a Razor Page to add a C# code block to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="ef9ae-232">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-232">For example:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="ef9ae-233">Il codice genera il markup HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-233">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="ef9ae-234">Il codice seguente è la classe C# Razor generata:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-234">The following code is the generated Razor C# class:</span></span>

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ef9ae-235">I metodi `@functions` fungono da metodi per la creazione di modelli quando includono markup:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-235">`@functions` methods serve as templating methods when they have markup:</span></span>

```cshtml
@{
    RenderName("Mahatma Gandhi");
    RenderName("Martin Luther King, Jr.");
}

@functions {
    private void RenderName(string name)
    {
        <p>Name: <strong>@name</strong></p>
    }
}
```

<span data-ttu-id="ef9ae-236">Il codice esegue il rendering dell'HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-236">The code renders the following HTML:</span></span>

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

::: moniker-end

### <a name="attribute"></a>@attribute

<span data-ttu-id="ef9ae-237">La direttiva `@attribute` aggiunge l'attributo specificato alla classe della pagina o della visualizzazione generata.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-237">The `@attribute` directive adds the given attribute to the class of the generated page or view.</span></span> <span data-ttu-id="ef9ae-238">L'esempio seguente aggiunge l'attributo `[Authorize]`:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-238">The following example adds the `[Authorize]` attribute:</span></span>

```cshtml
@attribute [Authorize]
```

> [!WARNING]
> <span data-ttu-id="ef9ae-239">Nella versione di ASP.NET Core 3.0 Preview 6 esiste un problema noto a causa del quale le direttive `@attribute` non funzionano nei file *\_Imports.razor* e *\_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-239">In ASP.NET Core 3.0 Preview 6 release, there's a known issue where `@attribute` directives don't work in *\_Imports.razor* and *\_ViewImports.cshtml* files.</span></span> <span data-ttu-id="ef9ae-240">Questo problema verrà risolto nella versione Preview 7.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-240">This will be addressed in the Preview 7 release.</span></span>

### <a name="namespace"></a>@namespace

<span data-ttu-id="ef9ae-241">La direttiva `@namespace` imposta lo spazio dei nomi della classe della pagina o della visualizzazione generata:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-241">The `@namespace` directive sets the namespace of the class of the generated page or view:</span></span>

```cshtml
@namespace Your.Namespace.Here
```

<span data-ttu-id="ef9ae-242">Se una pagina o una visualizzazione importa API con una direttiva `@namespace`, lo spazio dei nomi del file originale viene impostato in relazione a tale spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-242">If a page or view imports API with an `@namespace` directive, the original file's namespace is set relative to that namespace.</span></span> 

<span data-ttu-id="ef9ae-243">Se *MyApp/Pages/\_ViewImports.cshtml* contiene `@namespace Hello.World`, lo spazio dei nomi delle pagine o delle visualizzazioni che importano lo spazio dei nomi `Hello.World` viene impostato come illustrato nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-243">If *MyApp/Pages/\_ViewImports.cshtml* contains `@namespace Hello.World`, the namespace of pages or views that import the `Hello.World` namespace is set as shown in the following table.</span></span>

| <span data-ttu-id="ef9ae-244">Pagina (o visualizzazione)</span><span class="sxs-lookup"><span data-stu-id="ef9ae-244">Page (or view)</span></span>                     | <span data-ttu-id="ef9ae-245">Spazio dei nomi</span><span class="sxs-lookup"><span data-stu-id="ef9ae-245">Namespace</span></span>               |
| ---------------------------------- | ----------------------- |
| <span data-ttu-id="ef9ae-246">*MyApp/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ef9ae-246">*MyApp/Pages/Index.cshtml*</span></span>         | `Hello.World`           |
| <span data-ttu-id="ef9ae-247">*MyApp/Pages/MorePages/Bar.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ef9ae-247">*MyApp/Pages/MorePages/Bar.cshtml*</span></span> | `Hello.World.MorePages` |

<span data-ttu-id="ef9ae-248">Se più file di importazione includono la direttiva `@namespace`, viene usato il file più vicino alla pagina o alla visualizzazione nella catena di directory.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-248">If multiple import files have the `@namespace` directive, the file closest to the page or view in the directory chain is used.</span></span>

### <a name="section"></a>@section

<span data-ttu-id="ef9ae-249">La direttiva `@section` viene usata in combinazione con il [layout](xref:mvc/views/layout) per abilitare le pagine o le visualizzazioni per il rendering del contenuto in diverse parti della pagina HTML.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-249">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable pages or views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="ef9ae-250">Per altre informazioni, vedere [Sezioni](xref:mvc/views/layout#layout-sections-label).</span><span class="sxs-lookup"><span data-stu-id="ef9ae-250">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="templated-razor-delegates"></a><span data-ttu-id="ef9ae-251">Delegati Razor basati su modelli</span><span class="sxs-lookup"><span data-stu-id="ef9ae-251">Templated Razor delegates</span></span>

<span data-ttu-id="ef9ae-252">I modelli Razor consentono di definire un frammento di codice dell'interfaccia utente con il formato seguente:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-252">Razor templates allow you to define a UI snippet with the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="ef9ae-253">L'esempio seguente illustra come specificare un delegato Razor basato su modelli come <xref:System.Func%602>.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-253">The following example illustrates how to specify a templated Razor delegate as a <xref:System.Func%602>.</span></span> <span data-ttu-id="ef9ae-254">Per il parametro del metodo incapsulato dal delegato viene specificato il [tipo dinamico](/dotnet/csharp/programming-guide/types/using-type-dynamic).</span><span class="sxs-lookup"><span data-stu-id="ef9ae-254">The [dynamic type](/dotnet/csharp/programming-guide/types/using-type-dynamic) is specified for the parameter of the method that the delegate encapsulates.</span></span> <span data-ttu-id="ef9ae-255">Come valore restituito del delegato viene specificato un [tipo di oggetto](/dotnet/csharp/language-reference/keywords/object).</span><span class="sxs-lookup"><span data-stu-id="ef9ae-255">An [object type](/dotnet/csharp/language-reference/keywords/object) is specified as the return value of the delegate.</span></span> <span data-ttu-id="ef9ae-256">Il modello viene usato con un oggetto <xref:System.Collections.Generic.List%601> di `Pet` dotato della proprietà `Name`.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-256">The template is used with a <xref:System.Collections.Generic.List%601> of `Pet` that has a `Name` property.</span></span>

```csharp
public class Pet
{
    public string Name { get; set; }
}
```

```cshtml
@{
    Func<dynamic, object> petTemplate = @<p>You have a pet named <strong>@item.Name</strong>.</p>;

    var pets = new List<Pet>
    {
        new Pet { Name = "Rin Tin Tin" },
        new Pet { Name = "Mr. Bigglesworth" },
        new Pet { Name = "K-9" }
    };
}
```

<span data-ttu-id="ef9ae-257">Il rendering del modello viene eseguito con `pets` in un'istruzione `foreach`:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-257">The template is rendered with `pets` supplied by a `foreach` statement:</span></span>

```cshtml
@foreach (var pet in pets)
{
    @petTemplate(pet)
}
```

<span data-ttu-id="ef9ae-258">Output sottoposto a rendering:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-258">Rendered output:</span></span>

```html
<p>You have a pet named <strong>Rin Tin Tin</strong>.</p>
<p>You have a pet named <strong>Mr. Bigglesworth</strong>.</p>
<p>You have a pet named <strong>K-9</strong>.</p>
```

<span data-ttu-id="ef9ae-259">È anche possibile specificare un modello Razor inline come argomento di un metodo.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-259">You can also supply an inline Razor template as an argument to a method.</span></span> <span data-ttu-id="ef9ae-260">Nell'esempio seguente, il metodo `Repeat` riceve un modello Razor.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-260">In the following example, the `Repeat` method receives a Razor template.</span></span> <span data-ttu-id="ef9ae-261">Il metodo usa il modello per generare contenuto HTML tramite ripetizioni di elementi (item) ricavati da un elenco:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-261">The method uses the template to produce HTML content with repeats of items supplied from a list:</span></span>

```cshtml
@using Microsoft.AspNetCore.Html

@functions {
    public static IHtmlContent Repeat(IEnumerable<dynamic> items, int times,
        Func<dynamic, IHtmlContent> template)
    {
        var html = new HtmlContentBuilder();

        foreach (var item in items)
        {
            for (var i = 0; i < times; i++)
            {
                html.AppendHtml(template(item));
            }
        }

        return html;
    }
}
```

<span data-ttu-id="ef9ae-262">Usando l'elenco di animali domestici (pets) dell'esempio precedente, il metodo `Repeat` viene chiamato con:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-262">Using the list of pets from the prior example, the `Repeat` method is called with:</span></span>

* <span data-ttu-id="ef9ae-263"><xref:System.Collections.Generic.List%601> di `Pet`.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-263"><xref:System.Collections.Generic.List%601> of `Pet`.</span></span>
* <span data-ttu-id="ef9ae-264">Numero di ripetizioni di ogni animale domestico.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-264">Number of times to repeat each pet.</span></span>
* <span data-ttu-id="ef9ae-265">Modello inline da usare per gli elementi elenco di un elenco non ordinato.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-265">Inline template to use for the list items of an unordered list.</span></span>

```cshtml
<ul>
    @Repeat(pets, 3, @<li>@item.Name</li>)
</ul>
```

<span data-ttu-id="ef9ae-266">Output sottoposto a rendering:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-266">Rendered output:</span></span>

```html
<ul>
    <li>Rin Tin Tin</li>
    <li>Rin Tin Tin</li>
    <li>Rin Tin Tin</li>
    <li>Mr. Bigglesworth</li>
    <li>Mr. Bigglesworth</li>
    <li>Mr. Bigglesworth</li>
    <li>K-9</li>
    <li>K-9</li>
    <li>K-9</li>
</ul>
```

## <a name="tag-helpers"></a><span data-ttu-id="ef9ae-267">Helper tag</span><span class="sxs-lookup"><span data-stu-id="ef9ae-267">Tag Helpers</span></span>

<span data-ttu-id="ef9ae-268">Esistono tre direttive che riguardano gli [helper tag](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="ef9ae-268">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="ef9ae-269">Direttiva</span><span class="sxs-lookup"><span data-stu-id="ef9ae-269">Directive</span></span> | <span data-ttu-id="ef9ae-270">Funzione</span><span class="sxs-lookup"><span data-stu-id="ef9ae-270">Function</span></span> |
| --------- | -------- |
| [<span data-ttu-id="ef9ae-271">&commat;addTagHelper</span><span class="sxs-lookup"><span data-stu-id="ef9ae-271">&commat;addTagHelper</span></span>](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="ef9ae-272">Rende gli helper tag disponibili per una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-272">Makes Tag Helpers available to a view.</span></span> |
| [<span data-ttu-id="ef9ae-273">&commat;removeTagHelper</span><span class="sxs-lookup"><span data-stu-id="ef9ae-273">&commat;removeTagHelper</span></span>](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="ef9ae-274">Rimuove gli helper tag aggiunti in precedenza da una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-274">Removes Tag Helpers previously added from a view.</span></span> |
| [<span data-ttu-id="ef9ae-275">&commat;tagHelperPrefix</span><span class="sxs-lookup"><span data-stu-id="ef9ae-275">&commat;tagHelperPrefix</span></span>](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="ef9ae-276">Specifica un prefisso del tag per abilitare il supporto dell'helper tag e renderne esplicito l'uso.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-276">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="ef9ae-277">Parole chiave riservate Razor</span><span class="sxs-lookup"><span data-stu-id="ef9ae-277">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="ef9ae-278">Parole chiave Razor</span><span class="sxs-lookup"><span data-stu-id="ef9ae-278">Razor keywords</span></span>

* <span data-ttu-id="ef9ae-279">page (richiede ASP.NET Core 2.0 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="ef9ae-279">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="ef9ae-280">namespace</span><span class="sxs-lookup"><span data-stu-id="ef9ae-280">namespace</span></span>
* <span data-ttu-id="ef9ae-281">functions</span><span class="sxs-lookup"><span data-stu-id="ef9ae-281">functions</span></span>
* <span data-ttu-id="ef9ae-282">inherits</span><span class="sxs-lookup"><span data-stu-id="ef9ae-282">inherits</span></span>
* <span data-ttu-id="ef9ae-283">model</span><span class="sxs-lookup"><span data-stu-id="ef9ae-283">model</span></span>
* <span data-ttu-id="ef9ae-284">section</span><span class="sxs-lookup"><span data-stu-id="ef9ae-284">section</span></span>
* <span data-ttu-id="ef9ae-285">helper (attualmente non supportata da ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="ef9ae-285">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="ef9ae-286">Le parole chiave Razor sono precedute dal carattere di escape `@(Razor Keyword)` (ad esempio, `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="ef9ae-286">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="ef9ae-287">Parole chiave Razor C#</span><span class="sxs-lookup"><span data-stu-id="ef9ae-287">C# Razor keywords</span></span>

* <span data-ttu-id="ef9ae-288">case</span><span class="sxs-lookup"><span data-stu-id="ef9ae-288">case</span></span>
* <span data-ttu-id="ef9ae-289">do</span><span class="sxs-lookup"><span data-stu-id="ef9ae-289">do</span></span>
* <span data-ttu-id="ef9ae-290">default</span><span class="sxs-lookup"><span data-stu-id="ef9ae-290">default</span></span>
* <span data-ttu-id="ef9ae-291">for</span><span class="sxs-lookup"><span data-stu-id="ef9ae-291">for</span></span>
* <span data-ttu-id="ef9ae-292">foreach</span><span class="sxs-lookup"><span data-stu-id="ef9ae-292">foreach</span></span>
* <span data-ttu-id="ef9ae-293">if</span><span class="sxs-lookup"><span data-stu-id="ef9ae-293">if</span></span>
* <span data-ttu-id="ef9ae-294">else</span><span class="sxs-lookup"><span data-stu-id="ef9ae-294">else</span></span>
* <span data-ttu-id="ef9ae-295">lock</span><span class="sxs-lookup"><span data-stu-id="ef9ae-295">lock</span></span>
* <span data-ttu-id="ef9ae-296">switch</span><span class="sxs-lookup"><span data-stu-id="ef9ae-296">switch</span></span>
* <span data-ttu-id="ef9ae-297">try</span><span class="sxs-lookup"><span data-stu-id="ef9ae-297">try</span></span>
* <span data-ttu-id="ef9ae-298">catch</span><span class="sxs-lookup"><span data-stu-id="ef9ae-298">catch</span></span>
* <span data-ttu-id="ef9ae-299">finally</span><span class="sxs-lookup"><span data-stu-id="ef9ae-299">finally</span></span>
* <span data-ttu-id="ef9ae-300">using</span><span class="sxs-lookup"><span data-stu-id="ef9ae-300">using</span></span>
* <span data-ttu-id="ef9ae-301">while</span><span class="sxs-lookup"><span data-stu-id="ef9ae-301">while</span></span>

<span data-ttu-id="ef9ae-302">Le parole chiave Razor C# devono essere precedute dal doppio carattere di escape `@(@C# Razor Keyword)` (ad esempio, `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="ef9ae-302">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="ef9ae-303">Il primo `@` è il carattere di escape del parser Razor.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-303">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="ef9ae-304">Il secondo `@` è il carattere di escape del parser C#.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-304">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="ef9ae-305">Parole chiave riservate non usate da Razor</span><span class="sxs-lookup"><span data-stu-id="ef9ae-305">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="ef9ae-306">classe</span><span class="sxs-lookup"><span data-stu-id="ef9ae-306">class</span></span>

## <a name="inspect-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="ef9ae-307">Ispezionare la classe C# Razor generata per una visualizzazione</span><span class="sxs-lookup"><span data-stu-id="ef9ae-307">Inspect the Razor C# class generated for a view</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ef9ae-308">Con .NET Core SDK 2.1 o versioni successive, il [SDK Razor](xref:razor-pages/sdk) gestisce la compilazione dei file Razor.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-308">With .NET Core SDK 2.1 or later, the [Razor SDK](xref:razor-pages/sdk) handles compilation of Razor files.</span></span> <span data-ttu-id="ef9ae-309">Quando si compila un progetto, il SDK Razor genera una directory *obj/<build_configuration>/<target_framework_moniker>/Razor* nella radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-309">When building a project, the Razor SDK generates an *obj/<build_configuration>/<target_framework_moniker>/Razor* directory in the project root.</span></span> <span data-ttu-id="ef9ae-310">La struttura di directory all'interno della directory *Razor* rispecchia la struttura di directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-310">The directory structure within the *Razor* directory mirrors the project's directory structure.</span></span>

<span data-ttu-id="ef9ae-311">Si consideri la seguente struttura di directory in un progetto Razor Pages ASP.NET Core 2.1 destinato a .NET Core 2.1:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-311">Consider the following directory structure in an ASP.NET Core 2.1 Razor Pages project targeting .NET Core 2.1:</span></span>

* <span data-ttu-id="ef9ae-312">**Areas/**</span><span class="sxs-lookup"><span data-stu-id="ef9ae-312">**Areas/**</span></span>
  * <span data-ttu-id="ef9ae-313">**Admin/**</span><span class="sxs-lookup"><span data-stu-id="ef9ae-313">**Admin/**</span></span>
    * <span data-ttu-id="ef9ae-314">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="ef9ae-314">**Pages/**</span></span>
      * <span data-ttu-id="ef9ae-315">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ef9ae-315">*Index.cshtml*</span></span>
      * <span data-ttu-id="ef9ae-316">*Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="ef9ae-316">*Index.cshtml.cs*</span></span>
* <span data-ttu-id="ef9ae-317">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="ef9ae-317">**Pages/**</span></span>
  * <span data-ttu-id="ef9ae-318">**Shared/**</span><span class="sxs-lookup"><span data-stu-id="ef9ae-318">**Shared/**</span></span>
    * <span data-ttu-id="ef9ae-319">*_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ef9ae-319">*_Layout.cshtml*</span></span>
  * <span data-ttu-id="ef9ae-320">*_ViewImports.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ef9ae-320">*_ViewImports.cshtml*</span></span>
  * <span data-ttu-id="ef9ae-321">*_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ef9ae-321">*_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="ef9ae-322">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ef9ae-322">*Index.cshtml*</span></span>
  * <span data-ttu-id="ef9ae-323">*Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="ef9ae-323">*Index.cshtml.cs*</span></span>

<span data-ttu-id="ef9ae-324">La compilazione del progetto nella configurazione *Debug* produce la seguente directory *obj*:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-324">Building the project in *Debug* configuration yields the following *obj* directory:</span></span>

* <span data-ttu-id="ef9ae-325">**obj/**</span><span class="sxs-lookup"><span data-stu-id="ef9ae-325">**obj/**</span></span>
  * <span data-ttu-id="ef9ae-326">**Debug/**</span><span class="sxs-lookup"><span data-stu-id="ef9ae-326">**Debug/**</span></span>
    * <span data-ttu-id="ef9ae-327">**netcoreapp2.1/**</span><span class="sxs-lookup"><span data-stu-id="ef9ae-327">**netcoreapp2.1/**</span></span>
      * <span data-ttu-id="ef9ae-328">**Razor/**</span><span class="sxs-lookup"><span data-stu-id="ef9ae-328">**Razor/**</span></span>
        * <span data-ttu-id="ef9ae-329">**Areas/**</span><span class="sxs-lookup"><span data-stu-id="ef9ae-329">**Areas/**</span></span>
          * <span data-ttu-id="ef9ae-330">**Admin/**</span><span class="sxs-lookup"><span data-stu-id="ef9ae-330">**Admin/**</span></span>
            * <span data-ttu-id="ef9ae-331">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="ef9ae-331">**Pages/**</span></span>
              * <span data-ttu-id="ef9ae-332">*Index.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="ef9ae-332">*Index.g.cshtml.cs*</span></span>
        * <span data-ttu-id="ef9ae-333">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="ef9ae-333">**Pages/**</span></span>
          * <span data-ttu-id="ef9ae-334">**Shared/**</span><span class="sxs-lookup"><span data-stu-id="ef9ae-334">**Shared/**</span></span>
            * <span data-ttu-id="ef9ae-335">*_Layout.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="ef9ae-335">*_Layout.g.cshtml.cs*</span></span>
          * <span data-ttu-id="ef9ae-336">*_ViewImports.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="ef9ae-336">*_ViewImports.g.cshtml.cs*</span></span>
          * <span data-ttu-id="ef9ae-337">*_ViewStart.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="ef9ae-337">*_ViewStart.g.cshtml.cs*</span></span>
          * <span data-ttu-id="ef9ae-338">*Index.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="ef9ae-338">*Index.g.cshtml.cs*</span></span>

<span data-ttu-id="ef9ae-339">Per visualizzare la classe generata per *Pages/Index.cshtml* aprire *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-339">To view the generated class for *Pages/Index.cshtml*, open *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="ef9ae-340">Aggiungere la classe seguente al progetto ASP.NET Core MVC:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-340">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="ef9ae-341">In `Startup.ConfigureServices` eseguire l'override dell'elemento `RazorTemplateEngine` aggiunto da MVC con la classe `CustomTemplateEngine`:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-341">In `Startup.ConfigureServices`, override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="ef9ae-342">Impostare un punto di interruzione per l'istruzione `return csharpDocument;` di `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-342">Set a breakpoint on the `return csharpDocument;` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="ef9ae-343">Quando l'esecuzione del programma si interrompe nel punto di interruzione, visualizzare il valore di `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-343">When program execution stops at the breakpoint, view the value of `generatedCode`.</span></span>

![Visualizzazione nel visualizzatore testo del codice generato](razor/_static/tvr.png)

::: moniker-end

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="ef9ae-345">Visualizzazione di ricerche e distinzione tra maiuscole e minuscole</span><span class="sxs-lookup"><span data-stu-id="ef9ae-345">View lookups and case sensitivity</span></span>

<span data-ttu-id="ef9ae-346">Il motore di visualizzazione Razor esegue ricerche con distinzione tra maiuscole e minuscole per le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-346">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="ef9ae-347">La ricerca effettiva è tuttavia determinata dal file system sottostante:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-347">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="ef9ae-348">Origine basata su file:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-348">File based source:</span></span>
  * <span data-ttu-id="ef9ae-349">Nei sistemi operativi con file system che non fanno distinzione tra maiuscole e minuscole (ad esempio, Windows), le ricerche del provider di file fisici non eseguono la distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-349">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="ef9ae-350">Ad esempio, `return View("Test")` comporta corrispondenze per */Views/Home/Test.cshtml*, */Views/home/test.cshtml* e qualsiasi altra variante di maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-350">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="ef9ae-351">Nei file system che fanno distinzione tra maiuscole e minuscole (ad esempio Linux, OS x e con `EmbeddedFileProvider`), le ricerche eseguono la distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-351">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="ef9ae-352">Ad esempio, `return View("Test")` trova specificamente le corrispondenze per */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-352">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="ef9ae-353">Visualizzazioni precompilate: Con ASP.NET Core 2.0 e versioni successive, la ricerca di visualizzazioni precompilate non esegue la distinzione tra maiuscole e minuscole in tutti i sistemi operativi.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-353">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="ef9ae-354">Questo comportamento è identico al comportamento del provider di file fisici in Windows.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-354">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="ef9ae-355">Se due visualizzazioni precompilate differiscono solo nelle lettere maiuscole e minuscole, il risultato di ricerca è non deterministico.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-355">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="ef9ae-356">Gli sviluppatori sono invitati a far corrispondere le maiuscole e minuscole dei nomi di file e directory per quanto riguarda le maiuscole e minuscole di:</span><span class="sxs-lookup"><span data-stu-id="ef9ae-356">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

* <span data-ttu-id="ef9ae-357">Nomi di area, controller e azione.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-357">Area, controller, and action names.</span></span>
* <span data-ttu-id="ef9ae-358">Pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-358">Razor Pages.</span></span>

<span data-ttu-id="ef9ae-359">La distinzione tra maiuscole e minuscole assicura che le distribuzioni trovino le proprie visualizzazioni indipendentemente dal file system sottostante.</span><span class="sxs-lookup"><span data-stu-id="ef9ae-359">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
