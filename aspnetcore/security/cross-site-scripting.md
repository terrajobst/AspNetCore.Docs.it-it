---
title: Impedisci l'esecuzione di script tra siti (XSS) in ASP.NET Core
author: rick-anderson
description: Informazioni sulle tecniche e sugli script tra siti (XSS) per risolvere questa vulnerabilità in un'app ASP.NET Core.
ms.author: riande
ms.date: 10/02/2018
uid: security/cross-site-scripting
ms.openlocfilehash: 1d6f605dc336d8768b8a47e4995f119d198a61af
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667979"
---
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a><span data-ttu-id="3df1a-103">Impedisci l'esecuzione di script tra siti (XSS) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3df1a-103">Prevent Cross-Site Scripting (XSS) in ASP.NET Core</span></span>

<span data-ttu-id="3df1a-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3df1a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3df1a-105">Il cross-site scripting (XSS) è una vulnerabilità della sicurezza che consente a un utente malintenzionato di inserire gli script sul lato client (in genere JavaScript) nelle pagine Web.</span><span class="sxs-lookup"><span data-stu-id="3df1a-105">Cross-Site Scripting (XSS) is a security vulnerability which enables an attacker to place client side scripts (usually JavaScript) into web pages.</span></span> <span data-ttu-id="3df1a-106">Quando gli altri utenti caricano pagine interessate, gli script dell'utente malintenzionato vengono eseguiti, consentendo all'autore dell'attacco di rubare cookie e token di sessione, modificare il contenuto della pagina Web tramite la manipolazione DOM o reindirizzare il browser a un'altra pagina.</span><span class="sxs-lookup"><span data-stu-id="3df1a-106">When other users load affected pages the attacker's scripts will run, enabling the attacker to steal cookies and session tokens, change the contents of the web page through DOM manipulation or redirect the browser to another page.</span></span> <span data-ttu-id="3df1a-107">In genere, le vulnerabilità XSS si verificano quando un'applicazione accetta l'input dell'utente e la restituisce a una pagina senza convalidare, codificare o eseguire l'escape.</span><span class="sxs-lookup"><span data-stu-id="3df1a-107">XSS vulnerabilities generally occur when an application takes user input and outputs it to a page without validating, encoding or escaping it.</span></span>

## <a name="protecting-your-application-against-xss"></a><span data-ttu-id="3df1a-108">Protezione dell'applicazione da XSS</span><span class="sxs-lookup"><span data-stu-id="3df1a-108">Protecting your application against XSS</span></span>

<span data-ttu-id="3df1a-109">A un livello di base XSS funziona facendo in modo che l'applicazione inserisca un tag `<script>` nella pagina di cui è stato eseguito il rendering oppure inserendo un evento `On*` in un elemento.</span><span class="sxs-lookup"><span data-stu-id="3df1a-109">At a basic level XSS works by tricking your application into inserting a `<script>` tag into your rendered page, or by inserting an `On*` event into an element.</span></span> <span data-ttu-id="3df1a-110">Per evitare di introdurre XSS nella propria applicazione, gli sviluppatori devono utilizzare i seguenti passaggi di prevenzione.</span><span class="sxs-lookup"><span data-stu-id="3df1a-110">Developers should use the following prevention steps to avoid introducing XSS into their application.</span></span>

1. <span data-ttu-id="3df1a-111">Non inserire mai i dati non attendibili nell'input HTML, a meno che non si segua il resto dei passaggi riportati di seguito.</span><span class="sxs-lookup"><span data-stu-id="3df1a-111">Never put untrusted data into your HTML input, unless you follow the rest of the steps below.</span></span> <span data-ttu-id="3df1a-112">I dati non attendibili sono i dati che possono essere controllati da un utente malintenzionato, input di form HTML, stringhe di query, intestazioni HTTP, persino dati originati da un database, perché un utente malintenzionato potrebbe violare il database anche se non è in grado di violare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3df1a-112">Untrusted data is any data that may be controlled by an attacker, HTML form inputs, query strings, HTTP headers, even data sourced from a database as an attacker may be able to breach your database even if they cannot breach your application.</span></span>

2. <span data-ttu-id="3df1a-113">Prima di inserire dati non attendibili all'interno di un elemento HTML, assicurarsi che sia codificato in HTML.</span><span class="sxs-lookup"><span data-stu-id="3df1a-113">Before putting untrusted data inside an HTML element ensure it's HTML encoded.</span></span> <span data-ttu-id="3df1a-114">La codifica HTML accetta caratteri come &lt; e li modifica in un formato sicuro come &amp;lt;</span><span class="sxs-lookup"><span data-stu-id="3df1a-114">HTML encoding takes characters such as &lt; and changes them into a safe form like &amp;lt;</span></span>

3. <span data-ttu-id="3df1a-115">Prima di inserire dati non attendibili in un attributo HTML, assicurarsi che sia codificato in HTML.</span><span class="sxs-lookup"><span data-stu-id="3df1a-115">Before putting untrusted data into an HTML attribute ensure it's HTML encoded.</span></span> <span data-ttu-id="3df1a-116">La codifica dell'attributo HTML è un superset della codifica HTML e codifica caratteri aggiuntivi come "and".</span><span class="sxs-lookup"><span data-stu-id="3df1a-116">HTML attribute encoding is a superset of HTML encoding and encodes additional characters such as " and '.</span></span>

4. <span data-ttu-id="3df1a-117">Prima di inserire dati non attendibili in JavaScript, inserire i dati in un elemento HTML il cui contenuto viene recuperato in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="3df1a-117">Before putting untrusted data into JavaScript place the data in an HTML element whose contents you retrieve at runtime.</span></span> <span data-ttu-id="3df1a-118">Se ciò non è possibile, assicurarsi che i dati siano codificati in JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3df1a-118">If this isn't possible, then ensure the data is JavaScript encoded.</span></span> <span data-ttu-id="3df1a-119">La codifica JavaScript accetta caratteri pericolosi per JavaScript e li sostituisce con la relativa esadecimale, ad esempio &lt; verrebbe codificata come `\u003C`.</span><span class="sxs-lookup"><span data-stu-id="3df1a-119">JavaScript encoding takes dangerous characters for JavaScript and replaces them with their hex, for example &lt; would be encoded as `\u003C`.</span></span>

5. <span data-ttu-id="3df1a-120">Prima di inserire dati non attendibili in una stringa di query URL, assicurarsi che l'URL sia codificato.</span><span class="sxs-lookup"><span data-stu-id="3df1a-120">Before putting untrusted data into a URL query string ensure it's URL encoded.</span></span>

## <a name="html-encoding-using-razor"></a><span data-ttu-id="3df1a-121">Codifica HTML con Razor</span><span class="sxs-lookup"><span data-stu-id="3df1a-121">HTML Encoding using Razor</span></span>

<span data-ttu-id="3df1a-122">Il motore Razor usato in MVC codifica automaticamente tutto l'output originato dalle variabili, a meno che non si lavori molto duramente per impedirlo.</span><span class="sxs-lookup"><span data-stu-id="3df1a-122">The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.</span></span> <span data-ttu-id="3df1a-123">Usa le regole di codifica degli attributi HTML quando si usa la direttiva *@* .</span><span class="sxs-lookup"><span data-stu-id="3df1a-123">It uses HTML attribute encoding rules whenever you use the *@* directive.</span></span> <span data-ttu-id="3df1a-124">Poiché la codifica degli attributi HTML è un superset della codifica HTML, questo significa che non è necessario preoccuparsi se è necessario usare la codifica HTML o la codifica degli attributi HTML.</span><span class="sxs-lookup"><span data-stu-id="3df1a-124">As HTML attribute encoding is a superset of HTML encoding this means you don't have to concern yourself with whether you should use HTML encoding or HTML attribute encoding.</span></span> <span data-ttu-id="3df1a-125">È necessario assicurarsi di usare @ solo in un contesto HTML, non quando si tenta di inserire input non attendibile direttamente in JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3df1a-125">You must ensure that you only use @ in an HTML context, not when attempting to insert untrusted input directly into JavaScript.</span></span> <span data-ttu-id="3df1a-126">Gli helper Tag codificano inoltre l'input usato nei parametri tag.</span><span class="sxs-lookup"><span data-stu-id="3df1a-126">Tag helpers will also encode input you use in tag parameters.</span></span>

<span data-ttu-id="3df1a-127">Prendere in considerazione la visualizzazione Razor seguente:</span><span class="sxs-lookup"><span data-stu-id="3df1a-127">Take the following Razor view:</span></span>

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

<span data-ttu-id="3df1a-128">Questa visualizzazione restituisce il contenuto della variabile *untrustedInput* .</span><span class="sxs-lookup"><span data-stu-id="3df1a-128">This view outputs the contents of the *untrustedInput* variable.</span></span> <span data-ttu-id="3df1a-129">Questa variabile include alcuni caratteri usati negli attacchi XSS, in particolare &lt;"e &gt;.</span><span class="sxs-lookup"><span data-stu-id="3df1a-129">This variable includes some characters which are used in XSS attacks, namely &lt;, " and &gt;.</span></span> <span data-ttu-id="3df1a-130">L'analisi dell'origine Mostra l'output sottoposto a rendering codificato come:</span><span class="sxs-lookup"><span data-stu-id="3df1a-130">Examining the source shows the rendered output encoded as:</span></span>

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> <span data-ttu-id="3df1a-131">ASP.NET Core MVC fornisce una classe `HtmlString` che non viene codificata automaticamente al momento dell'output.</span><span class="sxs-lookup"><span data-stu-id="3df1a-131">ASP.NET Core MVC provides an `HtmlString` class which isn't automatically encoded upon output.</span></span> <span data-ttu-id="3df1a-132">Questa operazione non deve mai essere utilizzata in combinazione con un input non attendibile, in quanto verrà esposta una vulnerabilità XSS.</span><span class="sxs-lookup"><span data-stu-id="3df1a-132">This should never be used in combination with untrusted input as this will expose an XSS vulnerability.</span></span>

## <a name="javascript-encoding-using-razor"></a><span data-ttu-id="3df1a-133">Codifica JavaScript con Razor</span><span class="sxs-lookup"><span data-stu-id="3df1a-133">JavaScript Encoding using Razor</span></span>

<span data-ttu-id="3df1a-134">In alcuni casi potrebbe essere necessario inserire un valore in JavaScript da elaborare nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="3df1a-134">There may be times you want to insert a value into JavaScript to process in your view.</span></span> <span data-ttu-id="3df1a-135">Per eseguire questa operazione è possibile procedere in due modi:</span><span class="sxs-lookup"><span data-stu-id="3df1a-135">There are two ways to do this.</span></span> <span data-ttu-id="3df1a-136">Il modo più sicuro per inserire valori consiste nell'inserire il valore in un attributo di dati di un tag e recuperarlo nel codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3df1a-136">The safest way to insert values is to place the value in a data attribute of a tag and retrieve it in your JavaScript.</span></span> <span data-ttu-id="3df1a-137">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="3df1a-137">For example:</span></span>

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   <div
       id="injectedData"
       data-untrustedinput="@untrustedInput" />

   <script>
     var injectedData = document.getElementById("injectedData");

     // All clients
     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     // HTML 5 clients only
     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="3df1a-138">Verrà generato il codice HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="3df1a-138">This will produce the following HTML</span></span>

```html
<div
     id="injectedData"
     data-untrustedinput="&lt;&quot;123&quot;&gt;" />

   <script>
     var injectedData = document.getElementById("injectedData");

     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="3df1a-139">Che, quando viene eseguito, eseguirà il rendering degli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="3df1a-139">Which, when it runs, will render the following:</span></span>

```
<"123">
   <"123">
```

<span data-ttu-id="3df1a-140">È anche possibile chiamare direttamente il codificatore JavaScript:</span><span class="sxs-lookup"><span data-stu-id="3df1a-140">You can also call the JavaScript encoder directly:</span></span>

```cshtml
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
```

<span data-ttu-id="3df1a-141">Verrà eseguito il rendering nel browser nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="3df1a-141">This will render in the browser as follows:</span></span>

```html
<script>
    document.write("\u003C\u0022123\u0022\u003E");
</script>
```

>[!WARNING]
> <span data-ttu-id="3df1a-142">Non concatenare input non attendibile in JavaScript per creare elementi DOM.</span><span class="sxs-lookup"><span data-stu-id="3df1a-142">Don't concatenate untrusted input in JavaScript to create DOM elements.</span></span> <span data-ttu-id="3df1a-143">È consigliabile utilizzare `createElement()` e assegnare i valori di proprietà in modo appropriato, ad esempio `node.TextContent=`, oppure utilizzare `element.SetAttribute()`/`element[attribute]=` altrimenti si esporrà a XSS basati su DOM.</span><span class="sxs-lookup"><span data-stu-id="3df1a-143">You should use `createElement()` and assign property values appropriately such as `node.TextContent=`, or use `element.SetAttribute()`/`element[attribute]=` otherwise you expose yourself to DOM-based XSS.</span></span>

## <a name="accessing-encoders-in-code"></a><span data-ttu-id="3df1a-144">Accesso ai codificatori nel codice</span><span class="sxs-lookup"><span data-stu-id="3df1a-144">Accessing encoders in code</span></span>

<span data-ttu-id="3df1a-145">I codificatori HTML, JavaScript e URL sono disponibili per il codice in due modi, è possibile inserirli tramite l' [inserimento di dipendenze](xref:fundamentals/dependency-injection) oppure è possibile usare i codificatori predefiniti contenuti nello spazio dei nomi `System.Text.Encodings.Web`.</span><span class="sxs-lookup"><span data-stu-id="3df1a-145">The HTML, JavaScript and URL encoders are available to your code in two ways, you can inject them via [dependency injection](xref:fundamentals/dependency-injection) or you can use the default encoders contained in the `System.Text.Encodings.Web` namespace.</span></span> <span data-ttu-id="3df1a-146">Se si usano i codificatori predefiniti, qualsiasi utente applicato agli intervalli di caratteri per essere considerato sicuro non avrà effetto. i codificatori predefiniti utilizzano le regole di codifica più sicure possibili.</span><span class="sxs-lookup"><span data-stu-id="3df1a-146">If you use the default encoders then any  you applied to character ranges to be treated as safe won't take effect - the default encoders use the safest encoding rules possible.</span></span>

<span data-ttu-id="3df1a-147">Per usare i codificatori configurabili tramite i, i costruttori devono prendere un parametro *HtmlEncoder*, *JavaScriptEncoder* e *UrlEncoder* in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="3df1a-147">To use the configurable encoders via DI your constructors should take an *HtmlEncoder*, *JavaScriptEncoder* and *UrlEncoder* parameter as appropriate.</span></span> <span data-ttu-id="3df1a-148">ad esempio:</span><span class="sxs-lookup"><span data-stu-id="3df1a-148">For example;</span></span>

```csharp
public class HomeController : Controller
   {
       HtmlEncoder _htmlEncoder;
       JavaScriptEncoder _javaScriptEncoder;
       UrlEncoder _urlEncoder;

       public HomeController(HtmlEncoder htmlEncoder,
                             JavaScriptEncoder javascriptEncoder,
                             UrlEncoder urlEncoder)
       {
           _htmlEncoder = htmlEncoder;
           _javaScriptEncoder = javascriptEncoder;
           _urlEncoder = urlEncoder;
       }
   }
   ```

## <a name="encoding-url-parameters"></a><span data-ttu-id="3df1a-149">Parametri URL di codifica</span><span class="sxs-lookup"><span data-stu-id="3df1a-149">Encoding URL Parameters</span></span>

<span data-ttu-id="3df1a-150">Se si desidera compilare una stringa di query URL con input non attendibile come valore, utilizzare il `UrlEncoder` per codificare il valore.</span><span class="sxs-lookup"><span data-stu-id="3df1a-150">If you want to build a URL query string with untrusted input as a value use the `UrlEncoder` to encode the value.</span></span> <span data-ttu-id="3df1a-151">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="3df1a-151">For example,</span></span>

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

<span data-ttu-id="3df1a-152">Dopo la codifica, la variabile valore conterrà `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span><span class="sxs-lookup"><span data-stu-id="3df1a-152">After encoding the encodedValue variable will contain `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span></span> <span data-ttu-id="3df1a-153">Spazi, virgolette, punteggiatura e altri caratteri unsafe verranno codificati in percentuale nel valore esadecimale. ad esempio, un carattere di spazio diventerà %20.</span><span class="sxs-lookup"><span data-stu-id="3df1a-153">Spaces, quotes, punctuation and other unsafe characters will be percent encoded to their hexadecimal value, for example a space character will become %20.</span></span>

>[!WARNING]
> <span data-ttu-id="3df1a-154">Non usare input non attendibile come parte di un percorso URL.</span><span class="sxs-lookup"><span data-stu-id="3df1a-154">Don't use untrusted input as part of a URL path.</span></span> <span data-ttu-id="3df1a-155">Passare sempre un input non attendibile come valore stringa di query.</span><span class="sxs-lookup"><span data-stu-id="3df1a-155">Always pass untrusted input as a query string value.</span></span>

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a><span data-ttu-id="3df1a-156">Personalizzazione dei codificatori</span><span class="sxs-lookup"><span data-stu-id="3df1a-156">Customizing the Encoders</span></span>

<span data-ttu-id="3df1a-157">Per impostazione predefinita, i codificatori utilizzano un elenco sicuro limitato all'intervallo Unicode Basic Latin e codificano tutti i caratteri al di fuori di tale intervallo come equivalenti del codice carattere.</span><span class="sxs-lookup"><span data-stu-id="3df1a-157">By default encoders use a safe list limited to the Basic Latin Unicode range and encode all characters outside of that range as their character code equivalents.</span></span> <span data-ttu-id="3df1a-158">Questo comportamento interessa anche Razor TagHelper e il rendering di HtmlHelper perché utilizzerà i codificatori per restituire le stringhe.</span><span class="sxs-lookup"><span data-stu-id="3df1a-158">This behavior also affects Razor TagHelper and HtmlHelper rendering as it will use the encoders to output your strings.</span></span>

<span data-ttu-id="3df1a-159">Il motivo è quello di proteggersi da bug sconosciuti o futuri (i bug del browser precedenti hanno attivato l'analisi in base all'elaborazione di caratteri non in lingua inglese).</span><span class="sxs-lookup"><span data-stu-id="3df1a-159">The reasoning behind this is to protect against unknown or future browser bugs (previous browser bugs have tripped up parsing based on the processing of non-English characters).</span></span> <span data-ttu-id="3df1a-160">Se il sito Web USA in modo intensivo caratteri non latini, come il cinese, il cirillico o altri, probabilmente non è il comportamento desiderato.</span><span class="sxs-lookup"><span data-stu-id="3df1a-160">If your web site makes heavy use of non-Latin characters, such as Chinese, Cyrillic or others this is probably not the behavior you want.</span></span>

<span data-ttu-id="3df1a-161">È possibile personalizzare gli elenchi di sicurezza del codificatore in modo da includere gli intervalli Unicode appropriati per l'applicazione durante l'avvio, in `ConfigureServices()`.</span><span class="sxs-lookup"><span data-stu-id="3df1a-161">You can customize the encoder safe lists to include Unicode ranges appropriate to your application during startup, in `ConfigureServices()`.</span></span>

<span data-ttu-id="3df1a-162">Se ad esempio si usa la configurazione predefinita, è possibile usare un HtmlHelper Razor come segue;</span><span class="sxs-lookup"><span data-stu-id="3df1a-162">For example, using the default configuration you might use a Razor HtmlHelper like so;</span></span>

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

<span data-ttu-id="3df1a-163">Quando si visualizza l'origine della pagina Web, si noterà che è stato eseguito il rendering come segue, con il testo cinese codificato;</span><span class="sxs-lookup"><span data-stu-id="3df1a-163">When you view the source of the web page you will see it has been rendered as follows, with the Chinese text encoded;</span></span>

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

<span data-ttu-id="3df1a-164">Per ampliare i caratteri considerati sicuri dal codificatore, inserire la riga seguente nel metodo `ConfigureServices()` in `startup.cs`;</span><span class="sxs-lookup"><span data-stu-id="3df1a-164">To widen the characters treated as safe by the encoder you would insert the following line into the `ConfigureServices()` method in `startup.cs`;</span></span>

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

<span data-ttu-id="3df1a-165">In questo esempio l'elenco Safe viene ampliato per includere l'intervallo Unicode CjkUnifiedIdeographs.</span><span class="sxs-lookup"><span data-stu-id="3df1a-165">This example widens the safe list to include the Unicode Range CjkUnifiedIdeographs.</span></span> <span data-ttu-id="3df1a-166">L'output di cui è stato eseguito il rendering è ora</span><span class="sxs-lookup"><span data-stu-id="3df1a-166">The rendered output would now become</span></span>

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

<span data-ttu-id="3df1a-167">Gli intervalli di elenchi sicuri vengono specificati come grafici di codice Unicode e non per le lingue.</span><span class="sxs-lookup"><span data-stu-id="3df1a-167">Safe list ranges are specified as Unicode code charts, not languages.</span></span> <span data-ttu-id="3df1a-168">Lo [standard Unicode](https://unicode.org/) include un elenco di [grafici di codice](https://www.unicode.org/charts/index.html) che è possibile utilizzare per trovare il grafico contenente i caratteri.</span><span class="sxs-lookup"><span data-stu-id="3df1a-168">The [Unicode standard](https://unicode.org/) has a list of [code charts](https://www.unicode.org/charts/index.html) you can use to find the chart containing your characters.</span></span> <span data-ttu-id="3df1a-169">Ogni codificatore, HTML, JavaScript e URL, deve essere configurato separatamente.</span><span class="sxs-lookup"><span data-stu-id="3df1a-169">Each encoder, Html, JavaScript and Url, must be configured separately.</span></span>

> [!NOTE]
> <span data-ttu-id="3df1a-170">La personalizzazione dell'elenco di sicurezza influiscono solo sui codificatori originati tramite l'inserimento DI dipendenze.</span><span class="sxs-lookup"><span data-stu-id="3df1a-170">Customization of the safe list only affects encoders sourced via DI.</span></span> <span data-ttu-id="3df1a-171">Se si accede direttamente a un codificatore tramite `System.Text.Encodings.Web.*Encoder.Default` il valore predefinito, verrà utilizzato solo il tipo di sicurezza Basic Latin.</span><span class="sxs-lookup"><span data-stu-id="3df1a-171">If you directly access an encoder via `System.Text.Encodings.Web.*Encoder.Default` then the default, Basic Latin only safelist will be used.</span></span>

## <a name="where-should-encoding-take-place"></a><span data-ttu-id="3df1a-172">Dove dovrebbe avvenire la codifica?</span><span class="sxs-lookup"><span data-stu-id="3df1a-172">Where should encoding take place?</span></span>

<span data-ttu-id="3df1a-173">La pratica generalmente accettata è che la codifica avvenga nel punto di output e che i valori codificati non devono mai essere archiviati in un database.</span><span class="sxs-lookup"><span data-stu-id="3df1a-173">The general accepted practice is that encoding takes place at the point of output and encoded values should never be stored in a database.</span></span> <span data-ttu-id="3df1a-174">La codifica nel punto di output consente di modificare l'utilizzo dei dati, ad esempio da HTML a un valore query string.</span><span class="sxs-lookup"><span data-stu-id="3df1a-174">Encoding at the point of output allows you to change the use of data, for example, from HTML to a query string value.</span></span> <span data-ttu-id="3df1a-175">Consente inoltre di cercare facilmente i dati senza dover codificare i valori prima di eseguire ricerca e di sfruttare i vantaggi di eventuali modifiche o correzioni di errori apportate agli encoder.</span><span class="sxs-lookup"><span data-stu-id="3df1a-175">It also enables you to easily search your data without having to encode values before searching and allows you to take advantage of any changes or bug fixes made to encoders.</span></span>

## <a name="validation-as-an-xss-prevention-technique"></a><span data-ttu-id="3df1a-176">Convalida come tecnica di prevenzione XSS</span><span class="sxs-lookup"><span data-stu-id="3df1a-176">Validation as an XSS prevention technique</span></span>

<span data-ttu-id="3df1a-177">La convalida può essere uno strumento utile per limitare gli attacchi XSS.</span><span class="sxs-lookup"><span data-stu-id="3df1a-177">Validation can be a useful tool in limiting XSS attacks.</span></span> <span data-ttu-id="3df1a-178">Ad esempio, una stringa numerica contenente solo i caratteri 0-9 non attiverà un attacco XSS.</span><span class="sxs-lookup"><span data-stu-id="3df1a-178">For example, a numeric string containing only the characters 0-9 won't trigger an XSS attack.</span></span> <span data-ttu-id="3df1a-179">La convalida diventa più complessa quando si accetta codice HTML nell'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="3df1a-179">Validation becomes more complicated when accepting HTML in user input.</span></span> <span data-ttu-id="3df1a-180">L'analisi dell'input HTML è difficile, se non è impossibile.</span><span class="sxs-lookup"><span data-stu-id="3df1a-180">Parsing HTML input is difficult, if not impossible.</span></span> <span data-ttu-id="3df1a-181">Markdown, abbinato a un parser che rimuove il codice HTML incorporato, è un'opzione più sicura per l'accettazione di un input avanzato.</span><span class="sxs-lookup"><span data-stu-id="3df1a-181">Markdown, coupled with a parser that strips embedded HTML, is a safer option for accepting rich input.</span></span> <span data-ttu-id="3df1a-182">Non utilizzare mai solo la convalida.</span><span class="sxs-lookup"><span data-stu-id="3df1a-182">Never rely on validation alone.</span></span> <span data-ttu-id="3df1a-183">Codificare sempre l'input non attendibile prima dell'output, indipendentemente dalla convalida o dalla purificazione eseguita.</span><span class="sxs-lookup"><span data-stu-id="3df1a-183">Always encode untrusted input before output, no matter what validation or sanitization has been performed.</span></span>
