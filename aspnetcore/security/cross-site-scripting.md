---
title: Impedendo Cross-Site Scripting
author: rick-anderson
description: "Questo documento introduce Cross-Site Scripting (XSS) e le tecniche per risolvere questa vulnerabilità in un'applicazione ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cross-site-scripting
ms.openlocfilehash: 3aaab9d4fecd3f0d0da6a0df4d83bee090b329ea
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="preventing-cross-site-scripting"></a><span data-ttu-id="a62e7-103">Impedendo Cross-Site Scripting</span><span class="sxs-lookup"><span data-stu-id="a62e7-103">Preventing Cross-Site Scripting</span></span>

<span data-ttu-id="a62e7-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a62e7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a62e7-105">Cross-Site Scripting (XSS) è una vulnerabilità di sicurezza che consente a un utente malintenzionato di inserire gli script lato client (in genere JavaScript) nelle pagine web.</span><span class="sxs-lookup"><span data-stu-id="a62e7-105">Cross-Site Scripting (XSS) is a security vulnerability which enables an attacker to place client side scripts (usually JavaScript) into web pages.</span></span> <span data-ttu-id="a62e7-106">Quando altri utenti di caricare pagine interessate verranno eseguiti gli script di utenti malintenzionati, consentendo all'utente malintenzionato di rubare i cookie e token di sessione, modificare il contenuto della pagina web mediante modifica DOM o reindirizzare il browser a un'altra pagina.</span><span class="sxs-lookup"><span data-stu-id="a62e7-106">When other users load affected pages the attackers scripts will run, enabling the attacker to steal cookies and session tokens, change the contents of the web page through DOM manipulation or redirect the browser to another page.</span></span> <span data-ttu-id="a62e7-107">Vulnerabilità XSS si verificano in genere quando si accetta l'input dell'utente e lo restituisce in una pagina senza convalida, la codifica o caratteri di escape un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a62e7-107">XSS vulnerabilities generally occur when an application takes user input and outputs it in a page without validating, encoding or escaping it.</span></span>

## <a name="protecting-your-application-against-xss"></a><span data-ttu-id="a62e7-108">Proteggere l'applicazione rispetto a XSS</span><span class="sxs-lookup"><span data-stu-id="a62e7-108">Protecting your application against XSS</span></span>

<span data-ttu-id="a62e7-109">In una base XSS livello funziona da indurre l'applicazione in cui inserire un `<script>` tag nella pagina sottoposta a rendering o inserendo un `On*` evento in un elemento.</span><span class="sxs-lookup"><span data-stu-id="a62e7-109">At a basic level XSS works by tricking your application into inserting a `<script>` tag into your rendered page, or by inserting an `On*` event into an element.</span></span> <span data-ttu-id="a62e7-110">Gli sviluppatori devono utilizzare i seguenti passaggi di prevenzione per evitare di introdurre XSS nelle proprie applicazioni.</span><span class="sxs-lookup"><span data-stu-id="a62e7-110">Developers should use the following prevention steps to avoid introducing XSS into their application.</span></span>

1. <span data-ttu-id="a62e7-111">Non inserire dati non attendibili l'input HTML, se non si seguono il resto dei passaggi riportati di seguito.</span><span class="sxs-lookup"><span data-stu-id="a62e7-111">Never put untrusted data into your HTML input, unless you follow the rest of the steps below.</span></span> <span data-ttu-id="a62e7-112">Dati non attendibili sono tutti i dati che possono essere controllati da un utente malintenzionato, gli input di form HTML, le stringhe di query, le intestazioni HTTP, anche i dati originati da un database come un utente malintenzionato potrebbe essere in grado di violare il database, anche se essi non è possibile violare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a62e7-112">Untrusted data is any data that may be controlled by an attacker, HTML form inputs, query strings, HTTP headers, even data sourced from a database as an attacker may be able to breach your database even if they cannot breach your application.</span></span>

2. <span data-ttu-id="a62e7-113">Prima di inserire i dati non attendibili all'interno di un elemento HTML verificare che sia codificato in formato HTML.</span><span class="sxs-lookup"><span data-stu-id="a62e7-113">Before putting untrusted data inside an HTML element ensure it's HTML encoded.</span></span> <span data-ttu-id="a62e7-114">La codifica HTML accetta caratteri, ad esempio &lt; e li converte in un formato sicuro come &amp;lt;</span><span class="sxs-lookup"><span data-stu-id="a62e7-114">HTML encoding takes characters such as &lt; and changes them into a safe form like &amp;lt;</span></span>

3. <span data-ttu-id="a62e7-115">Prima di inserire i dati non attendibili in un attributo HTML verificare che sia l'attributo HTML con codificata.</span><span class="sxs-lookup"><span data-stu-id="a62e7-115">Before putting untrusted data into an HTML attribute ensure it's HTML attribute encoded.</span></span> <span data-ttu-id="a62e7-116">La codifica dell'attributo HTML è un superset di codifica HTML e consente di codificare i caratteri aggiuntivi, ad esempio "e".</span><span class="sxs-lookup"><span data-stu-id="a62e7-116">HTML attribute encoding is a superset of HTML encoding and encodes additional characters such as " and '.</span></span>

4. <span data-ttu-id="a62e7-117">Prima di inserire i dati non attendibili in JavaScript, inserire i dati in un elemento HTML il cui contenuto è recuperare in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="a62e7-117">Before putting untrusted data into JavaScript place the data in an HTML element whose contents you retrieve at runtime.</span></span> <span data-ttu-id="a62e7-118">Se questo non è possibile garantire che i dati viene codificato JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a62e7-118">If this isn't possible then ensure the data is JavaScript encoded.</span></span> <span data-ttu-id="a62e7-119">Codifica JavaScript accetta caratteri pericolosi per JavaScript e li sostituisce con i relativi esadecimale, ad esempio &lt; potrebbe essere codificato come `\u003C`.</span><span class="sxs-lookup"><span data-stu-id="a62e7-119">JavaScript encoding takes dangerous characters for JavaScript and replaces them with their hex, for example &lt; would be encoded as `\u003C`.</span></span>

5. <span data-ttu-id="a62e7-120">Prima di inserire i dati non attendibili in una stringa di query URL verificare è codificato in URL.</span><span class="sxs-lookup"><span data-stu-id="a62e7-120">Before putting untrusted data into a URL query string ensure it's URL encoded.</span></span>

## <a name="html-encoding-using-razor"></a><span data-ttu-id="a62e7-121">Codifica HTML utilizzando Razor</span><span class="sxs-lookup"><span data-stu-id="a62e7-121">HTML Encoding using Razor</span></span>

<span data-ttu-id="a62e7-122">Il motore Razor utilizzato automaticamente in MVC consente di codificare tutti output originati da variabili, a meno che non si lavora effettivamente duramente per evitare che in questo modo.</span><span class="sxs-lookup"><span data-stu-id="a62e7-122">The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.</span></span> <span data-ttu-id="a62e7-123">Viene utilizzato l'attributo HTML regole di codifica quando si usa il  *@*  direttiva.</span><span class="sxs-lookup"><span data-stu-id="a62e7-123">It uses HTML Attribute encoding rules whenever you use the *@* directive.</span></span> <span data-ttu-id="a62e7-124">HTML codifica dell'attributo è un superset di codifica HTML che ciò significa che non è necessario preoccuparsi se è necessario utilizzare la codifica HTML o la codifica dell'attributo HTML.</span><span class="sxs-lookup"><span data-stu-id="a62e7-124">As HTML attribute encoding is a superset of HTML encoding this means you don't have to concern yourself with whether you should use HTML encoding or HTML attribute encoding.</span></span> <span data-ttu-id="a62e7-125">È necessario assicurarsi di utilizzare solo in un contesto HTML, non quando si tenta di inserire direttamente input non attendibile JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a62e7-125">You must ensure that you only use @ in an HTML context, not when attempting to insert untrusted input directly into JavaScript.</span></span> <span data-ttu-id="a62e7-126">Gli helper di tag verranno inoltre codificare l'input che utilizzare nei parametri di tag.</span><span class="sxs-lookup"><span data-stu-id="a62e7-126">Tag helpers will also encode input you use in tag parameters.</span></span>

<span data-ttu-id="a62e7-127">Eseguire la seguente vista Razor;</span><span class="sxs-lookup"><span data-stu-id="a62e7-127">Take the following Razor view;</span></span>

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

<span data-ttu-id="a62e7-128">Questa vista restituisce il contenuto del *untrustedInput* variabile.</span><span class="sxs-lookup"><span data-stu-id="a62e7-128">This view outputs the contents of the *untrustedInput* variable.</span></span> <span data-ttu-id="a62e7-129">Questa variabile include alcuni caratteri utilizzati negli attacchi XSS, vale a dire &lt;, "e &gt;.</span><span class="sxs-lookup"><span data-stu-id="a62e7-129">This variable includes some characters which are used in XSS attacks, namely &lt;, " and &gt;.</span></span> <span data-ttu-id="a62e7-130">Esaminare l'origine viene codificato come output sottoposto a rendering:</span><span class="sxs-lookup"><span data-stu-id="a62e7-130">Examining the source shows the rendered output encoded as:</span></span>

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> <span data-ttu-id="a62e7-131">Componenti di base ASP.NET MVC fornisce un `HtmlString` classe che non è codificato automaticamente al momento di output.</span><span class="sxs-lookup"><span data-stu-id="a62e7-131">ASP.NET Core MVC provides an `HtmlString` class which isn't automatically encoded upon output.</span></span> <span data-ttu-id="a62e7-132">Non si devono mai utilizzato in combinazione con l'input non attendibile come questo esporrà una vulnerabilità XSS.</span><span class="sxs-lookup"><span data-stu-id="a62e7-132">This should never be used in combination with untrusted input as this will expose an XSS vulnerability.</span></span>

## <a name="javascript-encoding-using-razor"></a><span data-ttu-id="a62e7-133">JavaScript codifica utilizzando Razor</span><span class="sxs-lookup"><span data-stu-id="a62e7-133">Javascript Encoding using Razor</span></span>

<span data-ttu-id="a62e7-134">È possibile che si desidera inserire un valore JavaScript per l'elaborazione della visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="a62e7-134">There may be times you want to insert a value into JavaScript to process in your view.</span></span> <span data-ttu-id="a62e7-135">È possibile ottenere questo risultato in due modi.</span><span class="sxs-lookup"><span data-stu-id="a62e7-135">There are two ways to do this.</span></span> <span data-ttu-id="a62e7-136">Il modo più sicuro per inserire valori semplici è di inserire il valore in un attributo di dati di un tag e recuperarli in JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a62e7-136">The safest way to insert simple values is to place the value in a data attribute of a tag and retrieve it in your JavaScript.</span></span> <span data-ttu-id="a62e7-137">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a62e7-137">For example:</span></span>

```none
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

<span data-ttu-id="a62e7-138">Questa sintassi produrrà il seguente codice HTML</span><span class="sxs-lookup"><span data-stu-id="a62e7-138">This will produce the following HTML</span></span>

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

<span data-ttu-id="a62e7-139">Che, quando viene eseguito, verrà eseguito il rendering la procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="a62e7-139">Which, when it runs, will render the following;</span></span>

```none
<"123">
   <"123">
   ```

<span data-ttu-id="a62e7-140">È inoltre possibile chiamare direttamente, il codificatore di JavaScript</span><span class="sxs-lookup"><span data-stu-id="a62e7-140">You can also call the JavaScript encoder directly,</span></span>

```none
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
   ```

<span data-ttu-id="a62e7-141">Questo verrà eseguito il rendering nel browser come segue:</span><span class="sxs-lookup"><span data-stu-id="a62e7-141">This will render in the browser as follows;</span></span>

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> <span data-ttu-id="a62e7-142">Non concatenare l'input non attendibile in JavaScript per creare gli elementi DOM.</span><span class="sxs-lookup"><span data-stu-id="a62e7-142">Don't concatenate untrusted input in JavaScript to create DOM elements.</span></span> <span data-ttu-id="a62e7-143">È consigliabile utilizzare `createElement()` e assegnare i valori delle proprietà in modo appropriato, ad esempio `node.TextContent=`, oppure utilizzare `element.SetAttribute()` / `element[attribute]=` in caso contrario si espone al XSS basato su DOM.</span><span class="sxs-lookup"><span data-stu-id="a62e7-143">You should use `createElement()` and assign property values appropriately such as `node.TextContent=`, or use `element.SetAttribute()`/`element[attribute]=` otherwise you expose yourself to DOM-based XSS.</span></span>

## <a name="accessing-encoders-in-code"></a><span data-ttu-id="a62e7-144">L'accesso a codificatori nel codice</span><span class="sxs-lookup"><span data-stu-id="a62e7-144">Accessing encoders in code</span></span>

<span data-ttu-id="a62e7-145">I codificatori HTML, JavaScript e URL sono disponibili al codice in due modi diversi, è possibile inserire tramite [inserimento di dipendenze](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) o è possibile usare i codificatori predefinito contenuti nel `System.Text.Encodings.Web` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="a62e7-145">The HTML, JavaScript and URL encoders are available to your code in two ways, you can inject them via [dependency injection](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) or you can use the default encoders contained in the `System.Text.Encodings.Web` namespace.</span></span> <span data-ttu-id="a62e7-146">Se si utilizzano i codificatori predefinito qualsiasi applicati a intervalli di caratteri per essere considerato sicuro non avranno effetto: i codificatori predefinito utilizzano le regole di codifica più sicure possibile.</span><span class="sxs-lookup"><span data-stu-id="a62e7-146">If you use the default encoders then any  you applied to character ranges to be treated as safe won't take effect - the default encoders use the safest encoding rules possible.</span></span>

<span data-ttu-id="a62e7-147">Per usare i codificatori configurabili tramite devono richiedere i costruttori DI una *HtmlEncoder*, *JavaScriptEncoder* e *UrlEncoder* parametro come appropriato.</span><span class="sxs-lookup"><span data-stu-id="a62e7-147">To use the configurable encoders via DI your constructors should take an *HtmlEncoder*, *JavaScriptEncoder* and *UrlEncoder* parameter as appropriate.</span></span> <span data-ttu-id="a62e7-148">Ad esempio;</span><span class="sxs-lookup"><span data-stu-id="a62e7-148">For example;</span></span>

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

## <a name="encoding-url-parameters"></a><span data-ttu-id="a62e7-149">Codifica i parametri URL</span><span class="sxs-lookup"><span data-stu-id="a62e7-149">Encoding URL Parameters</span></span>

<span data-ttu-id="a62e7-150">Se si desidera compilare una stringa di query URL con input non attendibile come utilizzare un valore di `UrlEncoder` per codificare il valore.</span><span class="sxs-lookup"><span data-stu-id="a62e7-150">If you want to build a URL query string with untrusted input as a value use the `UrlEncoder` to encode the value.</span></span> <span data-ttu-id="a62e7-151">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="a62e7-151">For example,</span></span>

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

<span data-ttu-id="a62e7-152">Dopo la codifica di encodedValue variabile conterrà `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span><span class="sxs-lookup"><span data-stu-id="a62e7-152">After encoding the encodedValue variable will contain `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span></span> <span data-ttu-id="a62e7-153">Gli spazi, virgolette, segni di punteggiatura e altri caratteri non sicuri sarà percentuale con codifica in base al valore esadecimale, ad esempio un carattere di spazio diventerà % 20.</span><span class="sxs-lookup"><span data-stu-id="a62e7-153">Spaces, quotes, punctuation and other unsafe characters will be percent encoded to their hexadecimal value, for example a space character will become %20.</span></span>

>[!WARNING]
> <span data-ttu-id="a62e7-154">Non utilizzare l'input non attendibile come parte di un percorso URL.</span><span class="sxs-lookup"><span data-stu-id="a62e7-154">Don't use untrusted input as part of a URL path.</span></span> <span data-ttu-id="a62e7-155">Passare sempre l'input non attendibile come valore di stringa di query.</span><span class="sxs-lookup"><span data-stu-id="a62e7-155">Always pass untrusted input as a query string value.</span></span>

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a><span data-ttu-id="a62e7-156">I codificatori di personalizzazione</span><span class="sxs-lookup"><span data-stu-id="a62e7-156">Customizing the Encoders</span></span>

<span data-ttu-id="a62e7-157">Per impostazione predefinita i codificatori usano un elenco provvisoria limitato all'intervallo Unicode latino di base e codificare tutti i caratteri di fuori di tale intervallo, come gli equivalenti di codice di carattere.</span><span class="sxs-lookup"><span data-stu-id="a62e7-157">By default encoders use a safe list limited to the Basic Latin Unicode range and encode all characters outside of that range as their character code equivalents.</span></span> <span data-ttu-id="a62e7-158">Questo comportamento influisce anche sulle helper di tag Razor HtmlHelper il rendering come i codificatori verrà utilizzato per le stringhe di output.</span><span class="sxs-lookup"><span data-stu-id="a62e7-158">This behavior also affects Razor TagHelper and HtmlHelper rendering as it will use the encoders to output your strings.</span></span>

<span data-ttu-id="a62e7-159">La giustificazione è evitare i bug browser sconosciuto o futuro (bug browser precedenti hanno attivato backup l'analisi basata sull'elaborazione dei caratteri non inglesi).</span><span class="sxs-lookup"><span data-stu-id="a62e7-159">The reasoning behind this is to protect against unknown or future browser bugs (previous browser bugs have tripped up parsing based on the processing of non-English characters).</span></span> <span data-ttu-id="a62e7-160">Se il sito web abbia un uso massiccio di caratteri non latini, ad esempio il cinese, alfabeto cirillico o altri utenti non si tratta probabilmente il comportamento desiderato.</span><span class="sxs-lookup"><span data-stu-id="a62e7-160">If your web site makes heavy use of non-Latin characters, such as Chinese, Cyrillic or others this is probably not the behavior you want.</span></span>

<span data-ttu-id="a62e7-161">È possibile personalizzare gli elenchi di sicuro codificatore per includere gli intervalli appropriati per l'applicazione durante l'avvio, in Unicode `ConfigureServices()`.</span><span class="sxs-lookup"><span data-stu-id="a62e7-161">You can customize the encoder safe lists to include Unicode ranges appropriate to your application during startup, in `ConfigureServices()`.</span></span>

<span data-ttu-id="a62e7-162">Ad esempio, utilizzando la configurazione predefinita, è possibile utilizzare un HtmlHelper Razor come illustrato di seguito;</span><span class="sxs-lookup"><span data-stu-id="a62e7-162">For example, using the default configuration you might use a Razor HtmlHelper like so;</span></span>

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

<span data-ttu-id="a62e7-163">Quando si visualizza l'origine della pagina web verrà visualizzato che è stato eseguito il rendering come indicato di seguito, con il testo cinese codificato.</span><span class="sxs-lookup"><span data-stu-id="a62e7-163">When you view the source of the web page you will see it has been rendered as follows, with the Chinese text encoded;</span></span>

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

<span data-ttu-id="a62e7-164">Per ampliare i caratteri considerati attendibili dal codificatore si inserisce la riga seguente nel `ConfigureServices()` metodo `startup.cs`;</span><span class="sxs-lookup"><span data-stu-id="a62e7-164">To widen the characters treated as safe by the encoder you would insert the following line into the `ConfigureServices()` method in `startup.cs`;</span></span>

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

<span data-ttu-id="a62e7-165">In questo esempio viene convertito nell'elenco per includere il CjkUnifiedIdeographs intervallo Unicode.</span><span class="sxs-lookup"><span data-stu-id="a62e7-165">This example widens the safe list to include the Unicode Range CjkUnifiedIdeographs.</span></span> <span data-ttu-id="a62e7-166">Output sottoposto a rendering diventerebbe ora</span><span class="sxs-lookup"><span data-stu-id="a62e7-166">The rendered output would now become</span></span>

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

<span data-ttu-id="a62e7-167">Come grafici di codice Unicode, le lingue non vengono specificati gli intervalli di attendibili.</span><span class="sxs-lookup"><span data-stu-id="a62e7-167">Safe list ranges are specified as Unicode code charts, not languages.</span></span> <span data-ttu-id="a62e7-168">Il [standard Unicode](http://unicode.org/) include un elenco di [codice grafici](http://www.unicode.org/charts/index.html) è possibile utilizzare per trovare il grafico contenente i caratteri.</span><span class="sxs-lookup"><span data-stu-id="a62e7-168">The [Unicode standard](http://unicode.org/) has a list of [code charts](http://www.unicode.org/charts/index.html) you can use to find the chart containing your characters.</span></span> <span data-ttu-id="a62e7-169">Ogni codificatore, Html, JavaScript e Url, deve essere configurato separatamente.</span><span class="sxs-lookup"><span data-stu-id="a62e7-169">Each encoder, Html, JavaScript and Url, must be configured separately.</span></span>

> [!NOTE]
> <span data-ttu-id="a62e7-170">Personalizzazione dell'elenco influisce solo sui codificatori originati tramite DI.</span><span class="sxs-lookup"><span data-stu-id="a62e7-170">Customization of the safe list only affects encoders sourced via DI.</span></span> <span data-ttu-id="a62e7-171">Se si accede direttamente a un codificatore tramite `System.Text.Encodings.Web.*Encoder.Default` quindi il valore predefinito, latino di base da utilizzare solo dell'elenco indirizzi attendibili.</span><span class="sxs-lookup"><span data-stu-id="a62e7-171">If you directly access an encoder via `System.Text.Encodings.Web.*Encoder.Default` then the default, Basic Latin only safelist will be used.</span></span>

## <a name="where-should-encoding-take-place"></a><span data-ttu-id="a62e7-172">In cui devono essere inseriti take codifica?</span><span class="sxs-lookup"><span data-stu-id="a62e7-172">Where should encoding take place?</span></span>

<span data-ttu-id="a62e7-173">Generale accettato pratica è che la codifica viene eseguita al momento di output e valori codificati non devono essere mai archiviati in un database.</span><span class="sxs-lookup"><span data-stu-id="a62e7-173">The general accepted practice is that encoding takes place at the point of output and encoded values should never be stored in a database.</span></span> <span data-ttu-id="a62e7-174">Codifica nel punto di output consente di modificare l'utilizzo di dati, ad esempio, da HTML a un valore di stringa di query.</span><span class="sxs-lookup"><span data-stu-id="a62e7-174">Encoding at the point of output allows you to change the use of data, for example, from HTML to a query string value.</span></span> <span data-ttu-id="a62e7-175">Inoltre, consente di cercare facilmente i dati senza la necessità di codificare i valori prima di eseguire ricerche e consente di sfruttare i vantaggi di eventuali modifiche apportate ai codificatori di correzioni di bug.</span><span class="sxs-lookup"><span data-stu-id="a62e7-175">It also enables you to easily search your data without having to encode values before searching and allows you to take advantage of any changes or bug fixes made to encoders.</span></span>

## <a name="validation-as-an-xss-prevention-technique"></a><span data-ttu-id="a62e7-176">Convalida come tecnica di prevenzione XSS</span><span class="sxs-lookup"><span data-stu-id="a62e7-176">Validation as an XSS prevention technique</span></span>

<span data-ttu-id="a62e7-177">La convalida può essere uno strumento utile per limitare gli attacchi XSS.</span><span class="sxs-lookup"><span data-stu-id="a62e7-177">Validation can be a useful tool in limiting XSS attacks.</span></span> <span data-ttu-id="a62e7-178">Ad esempio, una semplice stringa numerica contenente solo i caratteri 0-9 non verrà generato un attacco XSS.</span><span class="sxs-lookup"><span data-stu-id="a62e7-178">For example, a simple numeric string containing only the characters 0-9 won't trigger an XSS attack.</span></span> <span data-ttu-id="a62e7-179">Convalida diventa più complessa se lo si desidera accettare HTML nell'input dell'utente, l'analisi di input HTML è difficile se non impossibile.</span><span class="sxs-lookup"><span data-stu-id="a62e7-179">Validation becomes more complicated should you wish to accept HTML in user input - parsing HTML input is difficult, if not impossible.</span></span> <span data-ttu-id="a62e7-180">MarkDown e altri formati di testo sarebbe un'opzione più sicura per l'input RTF.</span><span class="sxs-lookup"><span data-stu-id="a62e7-180">MarkDown and other text formats would be a safer option for rich input.</span></span> <span data-ttu-id="a62e7-181">Mai affidamento sulla convalida da solo.</span><span class="sxs-lookup"><span data-stu-id="a62e7-181">You should never rely on validation alone.</span></span> <span data-ttu-id="a62e7-182">Codificare sempre l'input non attendibile prima di output, indipendentemente dal tipo di convalida è stata eseguita.</span><span class="sxs-lookup"><span data-stu-id="a62e7-182">Always encode untrusted input before output, no matter what validation you have performed.</span></span>
